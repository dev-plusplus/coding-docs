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
    //...
   const [createCustomer, { loading, error }] = useCustomCreateCustomerMutation({
    onCompleted: () => {} ,
    onUpdate: (cache) => {} ,
    onError: ()=> {}
  });
  // ...
  // 

  const filter = { ...treeFilter(treeFilterObject), archived: { is_empty: true } };
  // vs
  const filter = useMemo(  () => {
    return { ...treeFilter(treeFilterObject), archived: { is_empty: true } };
  }, [treeFilterObject] );
  
  const [getTree] = useGetTreeLazyQuery({
    variables: {
      filter,
    },
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
        const filter = treeFilter(tree, page, otherFilter);
        fetchCustomers(filter);
      }
    } />
    // ...
  );
}
```

1.1. Query before components get rendered
1.2. Query after the components get rendered
1.3. query for Callbacks on events

## USE CASE 2: Event propagation / notification
## USE CASE 3: GLOBAL STATE: Share state between components 
## USE CASE 4: REST API: Share state between components
## USE CASE 5: LOCAL STATE: props, state and children state (prop drilling)
## USE CASE 6: Forms
## USE CASE 7: Persist state on Session or Local Storage

## TODOs: