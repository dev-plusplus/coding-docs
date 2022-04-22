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

## USE CASE 2: Initial Local Data on components:

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

## USE CASE 3: Event propagation / notification
## USE CASE 4: GLOBAL STATE: Shared state between components (siblings or long relationships)
## USE CASE 5: LOCAL STATE: props, state and children state (prop drilling)
## USE CASE 6: Complex local state objects: 
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
## USE CASE 8: Persist state on Session or Local Storage

## TODOs:
