An ACTION is an architectural element for Frontend applications. Not to be confused with elements of Backend applications such as `handlers`, `controllers`, `routers` or `services` 

Given the special of modifying data of the actions, things like composition can only be achieved on parts of the action and not the action itself.

## Definition:

An action is a function that modifies the state of an application.  An action can be seen as the abstraction of CRUD operations on a Front end Application.

Actions should always communicate errors using Exceptions. 
Actions should always have Business Objects as return types: Customers, List of Invoices, etc

## Elements of an Action:

- Validations: Data integrity and schema validation.
- Normalization: data transformation between domains: Multiple objects to Domain Objects
- Query or Mutations: normally with external services
- Events propagation: dispatches, event bus, etc
- Exceptions: For validation, services communication, success, server errors, etc
- Logging

## Naming

Actions should de named using CRUD verbs and Business Object Namesm, Examples:

- CREATE: `createCustomer`, `createInvoice`, `createUser`, `createSession`
- READ: `fetchCustomer`, `fetchInvoice`, `fetchUser`, `fetchSession`
- UPDATE: `updateCustomer`, `updateInvoice`, `updateUser`, `updateSession`
- DELETE: `deleteCustomer`, `deleteInvoice`, `deleteUser`, `deleteSession`

Other operations should follow the same convention for naming, for example: ARCHIVE, DESTROY, UP SERT, etc


### Example 1: Basic Action
```typescript
export const fetchSlackUserList = async (slackUsers) => {
  console.log('fetchSlackUserList', slackUsers);
  const client = sessionStore.getState(APOLLO_CLIENT);
  const filter = {
    OR: slackUsers.map((user) => ({ slackUserId: { equals: user.value.slackUserId } })),
  };
  console.log('fetchSlackUserList:filter:', filter);
  let response;
  try {
    response = await client.query({
      query: SLACK_USERS_LIST_QUERY,
      fetchPolicy: 'network-only',
      variables: { filter },
    });
  } catch (e) {
    error(e);
  }
  console.log('fetchSlackUserList:response:', response.data);
  return response.data;
};
```

### Example 1: `createAction` shortcut

```typescript
export const getStripeCustomerAction = createAction(
  OnGetStripeCustomerActionEvent,
  OnGetStripeCustomerActionErrorEvent,
  async (data) => {
    const response = await fetch('/api/checkout/customer', {
      method: 'POST',
      body: JSON.stringify(data),
    }).then((res) => res.json());

    return response?.customer;
  },
);
```
