# State management for ReactJS and React Native Applications


For easy state management: local and global, we have developed a simple state management library: https://github.com/cobuildlab/react-simple-state

## GENERAL BEST PRACTICES:
- 

## USE CASE 1: Graphql API: mutations and queries

- For Graphql mutations and queries than do not need to propagate side effects to other components, or communicate events, the prefered method to execute this type of functions and act on it is by using the generated hooks from: https://www.graphql-code-generator.com/ Generated hooks work well with Typescript, genrating types based on the Graphql schema.

  See: [HOW TO ]()

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
  
- In the case of data validation, data preparation, filters and in general, treatment that needs to be done to inputs or outputs, an external custom hook can be defined to abstract the logic of the operation



```typescript

// hooks.ts
export function useCustomCreateCustomerMutation({onCompleted: <>, onUpdate: <>, onError: <>}): JSX.Element {
  
  const mutate = useCallback(()=>{
    // Manage complex transformations, mapping or filerts
    // Example: 8base disconnects
  });
  
  const [createCustomer, { loading, error }] = useCreateCustomerMutation({
    /**
     * @returns {void} - Result.
     */
    onCompleted: () => {
      if (error === undefined) {
        snackbar.success('Customer created');
        history.push('/customers');
      } else {
        setOpen(true);
      }
    },

    /**
     * @param cache - Cache.
     * @returns Void.
     */
    update: (cache) =>
      clearQuerysFromCache([
        'customersList',
        'customer',
        'address',
        'addressesList',
      ])(cache),
  });
  
  return [mutate, createCustomer, loading, error]
}

export function JobCreateView(): JSX.Element {
    //...
   const [createCustomer, { loading, error }] = useCustomCreateCustomerMutation({
    onCompleted: () => {} ,
    onUpdate: (cache) => {} ,
    onError: ()=> {}
  });
  // ...
}
```

## USE CASE 2: Event propagation / notification
## USE CASE 3: GLOBAL STATE: Share state between components 
## USE CASE 4: REST API: Share state between components
## USE CASE 5: LOCAL STATE: props, state and children state (prop drilling)
## USE CASE 6: Forms
## USE CASE 7: Persist state on Session or Local Storage

## TODOs:
