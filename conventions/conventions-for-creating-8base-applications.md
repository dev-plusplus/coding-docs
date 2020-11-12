
# Conventions for creating 8base applications

This is a proposal to document conventions and patterns for working with 8base.com platform

## 1. Apollo Client:

This pattern explains how to generate the reference for the Apollo Client:

 - Single way to obtain the reference to the apollo `client`
 - Compatible with the apollo Provider - Consumer Pattern
 - The Auth Token can't be store it in the local storage for security safety
 - Support for 8base subscriptions

>>> src/shared/apollo.ts
```javascript
import { BatchHttpLink } from 'apollo-link-batch-http';
import { HttpLink } from 'apollo-link-http';
import { ApolloLink } from 'apollo-link';
import { onError } from 'apollo-link-error';
import { ApolloClient } from 'apollo-client';
import { InMemoryCache } from 'apollo-cache-inmemory';
import { getMainDefinition } from 'apollo-utilities';
import { SubscriptionLink } from '@8base/apollo-links';
import { setContext } from 'apollo-link-context';
import { rollbar } from 'shared/rollbar';

const WORKSPACE_ENDPOINT = process.env.REACT_APP_WORKSPACE_ENDPOINT;
const WORKSPACE_ID = process.env.REACT_APP_WORKSPACE_ID;
const EIGHTBASE_WS_ENDPOINT = process.env.REACT_APP_8base_WS_ENDPOINT; //`wss://ws.8base.com`
const batchHttpLink = new BatchHttpLink({ uri });
const httpLink = new HttpLink({ uri });

const subscriptionLink = new SubscriptionLink({
    uri: EIGHTBASE_WS_ENDPOINT,
    getAuthState: () => ({ token, workspaceId, }),
    onAuthError: (error) => {
      console.log('apollo:subscriptionLink:log', '[Subscription error]:', error);
      rollbar.error('[Subscription error]:', error);
    },
});

const onErrorLink = onError(({ graphQLErrors, networkError }) => {
    if (graphQLErrors) {
      graphQLErrors.forEach(({ message, locations, path }) => {
        const errorMessage = `[GraphQL error]: Message: ${message}, Location: ${locations}, Path: ${path}`;
        rollbar.error(errorMessage);
        console.log('apollo:onErrorLink:log', errorMessage);
      });
    }
    
    if (networkError) {
      rollbar.error('[Network error]:', networkError);
      console.log('apollo:onErrorLink:log', '[Network error]:', networkError);
    }
});

const networkLink = ApolloLink.split(
    ({ query }) => {
      const definition = getMainDefinition(query);
      return definition.kind === 'OperationDefinition' && definition.operation === 'subscription';
    },
  subscriptionLink,
    ApolloLink.split(
      (operation) => operation.getContext().important === true,
      httpLink,
      batchHttpLink,
    ),
);
const cache = new InMemoryCache();

/**
**/
export function createApolloClient(getToken, headers={} ) {
  const authLink = setContext((_, { _headers }) => {
  const token = getToken();
  return {
           headers: {
             ..._headers,
             ...headers,
             authorization: token ? `Bearer ${token}` : '',
           },
         };
  });
 
  const client = new ApolloClient({
    link: ApolloLink.from([authLink, onErrorLink, networkLink]),
    cache,
  });

  return client;
}

```

## use as Provider
>>> src/Application.ts

```js
import React from 'react';
import { ApolloProvider } from '@apollo/client';
import { BrowserRouter } from 'react-router-dom';
import { createApolloClient } from './shared/apollo';
import { Routes } from './routes/routes';
import { Auth0ProviderWithHistory } from './shared/components/Auth0ProviderWithHistory';
import {OnAuthToken} from 'modules/auth/events'

export const Application: React.FC = () => {
  const client = createApolloClient(()=>OnAuthToken.get());
  return (
  <BrowserRouter>
    <Auth0ProviderWithHistory>
      <ApolloProvider client={client}>
        <Routes />
      </ApolloProvider>
    </Auth0ProviderWithHistory>
  </BrowserRouter>
  )
};

```

## Use for Actions
>>> src/modules/auth/auth-actions.ts
```js
import { createApolloClient } from './shared/apollo';
import {OnAuthToken} from 'modules/auth/events'

const createProject = ()=> {
     const client = createApolloClient(()=>OnAuthToken.get());
    ....
};
```

## 2. Signup / Login with Auth0 

### For ReactJS
### For React Native
### Common

## 3. 8base database fields relationship naming
## 4. Create User programmatically from 8base triggers to Auth0
