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
## USE CASE 2: REST API and Web Services (not for initial data loading)

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

## USE CASE 3: Initial Data Loading (External APIs: Grapqhl, REST or Web Services)
## USE CASE 4: Initial Local State on components:

Query initial Data from an API with Graphql, REST or an SDK.

- For Graphql queries with Apollo: we prefer to use the generated `useQuery` directly on the component and not on an `useEffect`. For lazy dependencies we can use the `skip` parameter to avoid executing the query before we have a lazy value.

```typescript
export function JobListView(): JSX.Element {
  const {query:{id}, ready} = useParam({});
  const {loading, error, data, refetch } = useGetCustomerQuery({variables:{id}, skip:ready});
  ....
}
```

- For REST APIs and SDK Promises: we prefer to use the action structure with the [React Simple State](https://github.com/cobuildlab/react-simple-state#5-fetch-can-be-done-with-usefetchaction-hook) library:

```typescript
const OnSuccess = createEvent<number[]>();
const OnError = createEvent<Error>();
const action = createAction(OnSuccess, OnError ()=> {
  // Call an API or a Promise
});

export function JobListView(): JSX.Element {
  const {query:{id}, ready} = useParam({});
  const [loading, error, data, refetch] = useFetchAction(action, {variables:{id}, skip:ready});
  ....
}

```

## USE CASE 5: Event propagation / bradcasting
## USE CASE 6: GLOBAL STATE: Shared state between components (siblings or long relationships)
## USE CASE 7: LOCAL STATE: props, state and children state (prop drilling)
## USE CASE 8: Complex local state objects: 
- 
```typescript
export default function App() {
  const { register, handleSubmit, watch, setValue, formState: { errors } } = useForm<{
    autocomplete: NestedValue<Option[]>;
    select: NestedValue<number[]>;
  }>({
    defaultValues: { autocomplete: [], select: [] },
  });
  const onSubmit = handleSubmit((data) => console.log(data));

  React.useEffect(() => {
    register('autocomplete', {
      validate: (value) => value.length || 'This is required.',
    });
    register('select', {
      validate: (value) => value.length || 'This is required.',
    });
  }, [register]);

  return (
    <form onSubmit={onSubmit}>
      <Autocomplete
        options={options}
        getOptionLabel={(option: Option) => option.label}
        onChange={(e, options) => setValue('autocomplete', options)}
        renderInput={(params) => (
          <TextField
            {...params}
            error={Boolean(errors?.autocomplete)}
            helperText={errors?.autocomplete?.message}
          />
        )}
      />

      <Select value="" onChange={(e) => setValue('muiSelect', e.target.value as number[])}>
        <MenuItem value={10}>Ten</MenuItem>
        <MenuItem value={20}>Twenty</MenuItem>
      </Select>

      <input type="submit" />
    </form>
  );
}
```



## USE CASE 7: Forms
## USE CASE 8: Persisted state on Session or Local Storage

## TODOs:
