# State management for ReactJS and React Native Applications


For easy state management: local and global, we have developed a simple state management library: https://github.com/cobuildlab/react-simple-state

## GENERAL BEST PRACTICES:

- Be Lazy always, until you don't need to
- Use caching tools

## USE CASE 1: Graphql API: mutations and queries (not for initial data loading)

- For Graphql mutations and queries than do not need to load initial data, or communicate events, the prefered method to execute this type of functions and act on it is by using the generated hooks from: https://www.graphql-code-generator.com/ Generated hooks work well with Typescript, genrating types based on the Graphql schema.


  See: [HOW TO: The Graphql Code Generator ](../how-tos/graphql.md)

**EXAMPLE**

```typescript

export function JobCreateView(): JSX.Element {
   //...
  const [submitCreateJob, { loading }] = useJobCreateMutation({
    /**
     * @param result - Result.
     */
    onCompleted: (result) => {
      snackbar.success(`Job ${result.jobCreate.name} created`);
      history.goBack();
    },
    /**
     * @param cache - Apollo chache.
     */
    update: clearQuerysFromCache(['jobsList', 'fieldworker', 'customer']),
  });

   //...
}

```
  
- In the case of data validation, data normalization, filters and in general, treatment that needs to be done to inputs or outputs, an external utility function must be implemented.


```typescript

// utils.ts
export function normalizeCustomer(customer:Customer): Customer {
  const normalizedCustomer:Customer = {};
  // Manage complex transformations or mapping
  // Example: 8base disconnects
  return normalizedCustomer
}

export const treeFilter: TreeFilterType = ({
  propertyId,
  treeSpecieId,
  text,
}) => {
  const resp = {
    property: propertyId
      ? { id: { equals: propertyId } }
      : { customer: { id: { equals: clientIdEvent.get() } } },
    treeSpecie: (treeSpecieId || text) && {
      id: { equals: treeSpecieId },
      name: { contains: text },
    },
  } as TreeFilter;

  return resp;
};

export function JobCreateView(): JSX.Element {
   const [customer,setCustomer] = useState({...INITIAL_STATE});
   
    //...
   const [createCustomer, { loading, error }] = useCustomCreateCustomerMutation({
    onCompleted: () => {} ,
    onUpdate: (cache) => {} ,
    onError: ()=> {}
  });
  // ...

  const [fetchTrees] = useGetTreeLazyQuery({
    onCompleted: (res) => setTrees(res.treesList.items as Tree[]),
  });
  
  return (
    // ...Mutation
    <Button onClick={() => {
        const normalizedCustomer = normalizeCustomer(customer);
        createCustomer(normalizedCustomer);
      }
    } />
    // ...Query
    <Button onClick={() => {
        const filter = { ...treeFilter(treeFilterObject), archived: { is_empty: true } };
        fetchTrees({variables:{filter}});
      }
    } />
    // ...
  );
}
```
## USE CASE 2: REST API and Web Services: for initial data loading and based on user events

The prefered way of performing requests to REST APIs or Web Services is to use the State Library and the actions tooling or the `usePromise` of the Hooks Library 

Ref: React Simple State: https://github.com/cobuildlab/react-simple-state#4-actions-can-be-created-with-createaction-helper
Ref: Hooks Utils: https://github.com/cobuildlab/hooks-utils

- `useCallAction` for performing requests based on users events
- `useFetchAction` to perform requests when the component mounts or the deps change
- `usePromise` for perfoming requests both autimatically or in response to a user event


```js
import { createAction } from '@cobuildlab/react-simple-state';
import { OnFecthUserEvent, OnFetchUserErrorEvent } from './events';
import { apiClient } from './api';

// single declarition of the async service and the action
export const fetchUserAction = createAction(
  OnFecthUserEvent,
  OnFetchUserErrorEvent,
  async (id) => {
    const user = await apiClient.fetch({ user: id });

    return user;
  },
);

// Or we could declare the async service and then use in with diferent actions

export const fetchUserService = async (id) => {
  const user = await apiClient.fetch({ user: id });
  return user;
};

export const fetchMainUserAction = createAction(
  OnFecthMainUserEvent,
  OnFetchMainUserErrorEvent,
  fetchUserService,
);
export const fetchSecondaryUserAction = createAction(
  OnFecthSecondaryUserEvent,
  OnFetchSecondaryUserErrorEvent,
  fetchUserService,
);

export const UserProfile = ({ userId }) => {
  const userData = userDataState;
  const [user, loadingUser] = useFetchAction(fetchUser, [userId]);
  const [user, loadingUser, { refetch }] = useFetchAction(fetchUser, [userId], {
    onCompleted: () => {
      toast.success('user fetched');
    },
    onError: () => {
      toast.error('Error when fetching user');
    },
  });
  
  const [save, loadingSubmit] = useCallAction(saveUser);
  // OR...

  // setup the action, and return a function that will trigger the action when it needed.
  const [save, loadingSubmit] = useCallAction(saveUser, {
    onCompleted: () => {
      toast.success('user saved');
    },
    onError: () => {
      toast.error('Error when saving user');
    },
  });

  return (
    <Form>
      <SubmitButton onClick={() => save(userId, useCallAction)} />
    </Form>
  );
};

import { useCallback } from "react";
import { usePromise } from "@cobuildlab/hooks-utils";
import { fetchAgencies, fetchRoles } from "./agency-actions.js"

const AgencyView = ()=> {
  // NOTE: be aware that we using the same names for the error keys returned by the hook
  // This is just for keep the example simplest as posible
  const [agencies, loadingAgencies,  {error, call: fetchAgencies()} ] = usePromise(fetchAgencies);
  const [roles, loadingRoles, {error, call: fetchRoles()} ] = usePromise(()=>fetchRoles(agency), {
    onCmplete: (response)=>{
      console.log(response) // Roles response
    },
    onError: (error)=>{
      console.log(error) // Handle Error
    },
  });

  const fetchData = useCallback(() => {
    // Do something
    fetchRoles();
    fetchAgencies();
  });

  return ();
}

```

## USE CASE 3: GLOBAL STATE: Shared state between components (siblings or long relationships) 

For use cases when is necesary to share data between components that are is not a parent or a children, examples:

### 3.1 Reading state from grand parents (Ex. Theme, Session, Routes, etc)

For reading global state that is likely to not to change frequently (not to trigger re-renders) we can use the [Context API](https://reactjs.org/docs/context.html)

```js
function App(): JSX.Element {
  return (
    <BrowserRouter>
      <Auth0ProviderWithHistory>
        <ApolloProvider>
          <ThemeProvider theme={theme}>
            <Routes />
          </ThemeProvider>
        </ApolloProvider>
      </Auth0ProviderWithHistory>
    </BrowserRouter>
  );
}
```

### 3.2 Share state between components that are unrelated directly: siblings, grand children

For reading and sharing state the preferred way is to use the Events and Subscription API proposed on the [React Simple State Library](https://github.com/cobuildlab/react-simple-state)

Optionally, this events and subscriptions can be wrapped into custom hooks. 

```javascript
const useSelectedCompany = () => {
  const [company, setCompany] = useState(null);
  useSubscription((company) => setCompany(company));
  return [company, setCompany];
}

const useSelectedProject = () => {
  const [project, setProject] = useEvent(ProjectEvent);
  return [project,setProject];
}
const ProjectSettingsView = ()=>{
  const [company,setCompany] = useSelectedCompany();
  const [project,setProject] = useSelectProject();

  return (
    <>
      <h1></h1>
    </>
  );
}
```

### 3.3 Remote State and Trigger state change (events) on siblings or grand parents and grand children

**#### Graphql**

Based on the architecture for the [3factor.app](https://3factor.app/), the preferred way to listen to and react to events that change data on the system is to use [Grapqhl Subscriptions](https://www.apollographql.com/docs/react/data/subscriptions/)
 
**### REST**

There is no standard for trigger changes for Remote state yet. Similar solution can be found here:

- https://www.apollographql.com/docs/react/api/link/apollo-link-rest/
- https://react-query.tanstack.com/ 
- https://swr.vercel.app/docs/advanced/cache

For trigger events locally the preferred way is to use the Events and Subscription API proposed on the [React Simple State Library](https://github.com/cobuildlab/react-simple-state)

## USE CASE 4: Complex local state objects: 

- Busines Objects should be managed using forms with the [React Hook Forms library](https://react-hook-form.com/)
- Other complex states, that combine variables and callbacks are suggested to be managed independently with the `useState` hook
- Other variations of complex states are still to be reviewed to suggest best practices and conventions


## USE CASE 6: Persisted state on Session or Local Storage or Cookies

Sometimes is is required to persist data from the server or extenal services. For this we have implemented multiple solutions with any clear patterns, but some solutions are:

### 6.1 Reading / storing data from Persisted Storage
- https://github.com/apollographql/apollo-cache-persist

### 6.2 Batching change data operations: Mutations, Updates, Creates, etc
- https://www.npmjs.com/package/@wora/apollo-offline

### 6.3 Session data

- Pending discussion to evalute what data if any is safe to store on a Persisted Storage
