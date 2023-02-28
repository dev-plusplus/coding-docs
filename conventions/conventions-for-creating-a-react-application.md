


## 7. State Library: 

### Application State (Shared state): [React Simple State](https://github.com/cobuildlab/react-simple-state)

**Events Based Approach**
```js
// module-events.js

const event = createEvent();

// module-actions.js

const action = ()=> {
  // Some logic
  event.dispatch();
}

// View.js

const MyComponent = ()=> {
  const data = useEvent(event);
  
  useSubscription(event, ()=> {
  
  });
}
```

### Local (ONLY) Component State: [Hooks utils](https://github.com/cobuildlab/hooks-utils)

```js
// module-actions.js

const action = ()=> {
  // Some logic
  event.dispatch();
}

// View.js

const MyComponent = ()=> {
  // usePromise
  const [data, loading, {error, call: ()=> action()} ] = usePromise(()=>action(), {
    onComplete: (response)=>{
      console.log(response) //  response
    },
    onError: (error)=>{
      console.log(error) //  Error
    },
  });
  
  // useAction
  const [data, loading, error, actionCaller] = useAction(()=> createProject(),
      (data) => {
        // Success result
      },
      (error) => {
        // Errro result
      }
  ); 
}

```


## 9) Prefer `Session` pattern for managing protected routes instead of `ProtectedRoute` pattern

For declarative routers like [React Router](https://reacttraining.com/react-router/) or [React Reach](https://reach.tech/router) prefer a Parent `Session` component that individuals `ProtectedRoute` components.

*PREFER THIS*

```javascript

const App = ()=> {
  return (
    <Provider>
        <Switch>
            {/*Public Routes*/}
            <Route path="/auth" component={AuthCallback} />
            <Route>
                <Session>
                {/*Protected Routes*/}
                      <Route path="/management" component={ManagementView} />
                      <Route path="/reports" component={ReportsView} />
                      <Route path="/active-items" component={ActiveItemsView} />
                      <Route path="/settings" component={SettingsView} />
                </Session>
            </Route>
        </Switch>
    </Provider>  
)
sddsf}
```

*AND NOT THIS*

```javascript

const App = ()=> {
  return (
    <Provider>
        <Switch>
            {/*Public Routes*/}
            <Route path="/auth" component={AuthCallback} />
            {/*Protected Routes*/}
            <ProtectedRoute path="/management" component={ManagementView} />
            <ProtectedRoute path="/reports" component={ReportsView} />
            <ProtectedRoute path="/active-items" component={ActiveItemsView} />
            <ProtectedRoute path="/settings" component={SettingsView} />
        </Switch>
    </Provider>  
)
sddsf}
```

### Justification:

* Single point of control over a user session over redirects, on-boardings, profile completion, etc 
* In React the Session component can be used for [Error Boundaries](https://reactjs.org/docs/error-boundaries.html). 


## 10) Use shared `Permission` functions to control access to features and business operations:

- Isolate permission logic in functions that are easy to and share.
- A permission function should only depend dynamically on Business Objects.
- All permission functions should maintain the same signature:

```typescript
export const canApproveProject = (user:User, project:Project): [boolean, string?] => {
  if(project.status === FROZEN){
    return [false, "Project is on invalid status and can't be approved"];  
  }

  if(user.role === ROLE)
    return [true];

  return [false, "User can't approve this project"];
}; 
```

```typescript
/**
 * Permission to check If a User can delete an Alliance.
 * @param {User}user - The User requesting permissions.
 * @param {Alliance}alliance - The Alliance to be deleted.
 *
 * @returns {[boolean, string?]} - If we can execute the deletion or not.
 */
export const canDeleteAllianceV2 = (user: User, alliance: Alliance): [boolean, string?] => {
  if (!isAllianceCompleted(alliance)) {
    return [false, 'The Alliance is not Completed.'];
  }
  const userRole = getRoleOnAlliance(user, alliance);
  if (userRole === null) {
    return [false, 'Current User doesn\'t belongs to this alliance.'];
  }
  if (!isUserFromClientCompany(user, alliance)) {
    return [false, 'Only Users from the Client Company can delete an Alliance'];
  }
  if (alliance.status === ALLIANCE_IN_PROGRESS) {
    if (user.id === alliance.createdBy.id) {
      return [true];
    }
    return [false, 'When an Alliance is In Progress only the creator of the Alliance can delete it.'];
  } else {
    if (userRole.name === ALLIANCE_SER || userRole.name === ALLIANCE_ADMINISTRATOR) {
      return [true];
    }
    return [false, "Only Alliance SERs and Alliance Administrators can delete an Alliance."]
  }
};
```

## 11) Use shared `Validation` functions to control data integrity and consistency:

- Isolate validation logic in functions that are easy to share and reuse.
- A validation function should only depend dynamically on Business Objects.
- A validation must response to invalid scenarios with Exceptions with enough information to all the invalid scenarios.
- All validations functions should maintain the same signature:

```typescript
/**
 * Validator to check If a Project can be created.
 * @param {User}user - The User creating the project.
 * @param {Project}project - The Project to be created.
 * @throws {ValidationError} - If one or more constraints are not met.
 */
export const validateProject = (user:User, project:Project): void  => {
  const errors = [];

  if(project.name === ""){
    errors.push(["name", "Missing required name"]);      
  }

  if(user.role === EMPLOYEE && user.age < 45)
    errors.push(["role", "Invalid role for user"]);

  if(errors.length > 0)
    throw new ValidationError(errors);
}; 
```

## 12) Use Role based code splitting

### Create a intermediate file which transfrom component views to lazy components
```tsx
import { lazy } from 'react';

export const HomeLazy = lazy(() =>
  import('../home/Home').then((module) => ({ default: module.Home })),
);

export const AboutLazy = lazy(() =>
  import('../home/About').then((module) => ({ default: module.About })),
);
```
In order to allow tree shaking to work, the files `../home/Home` and `../home/About` need to only the export `Home` and `About`

### Use the `ComponentsLazy` with `Suspense`

```tsx

import React, { Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import { HomeLazy, AboutLazy } from './lazy-routes';

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={HomeLazy}/>
        <Route path="/about" component={AboutLazy}/>
      </Switch>
    </Suspense>
  </Router>
);
```



TODO:  
- YUP + hook react form
- ROLE BASED COMPONENTS: donde decidir que cargar
- Share state: Custom Hooks vs React simple state: Events vs Providers
- update lint config
- update jsdocs
- create logging rules
- create protect public endpoints
- Breadcrumbs
