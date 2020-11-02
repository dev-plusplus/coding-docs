# Cobuild Lab Coding Conventions

## 1. Applications Starters:

Application starters to help build the initial boilerplate to develop a project at Cobuild Lab:

* ESLINT configuration
* Prettier configuration
* Github Actions for: testing, building and linting
* Husky + Lint Stageg for pre-commit and pre-push git hooks
* Jest configuration and example test
* Cypress configuration and example test (Web applications only)
* Git ignore file
* npm ignore file
* Pull Request Templates
* Issue Templates

### 1️⃣ 8base React SPA Typescript Application Starter:  
https://github.com/cobuildlab/create-react-typescript-app
### 2️⃣ NextJS Typescript Application Starter:  
`TODO:`
### 3️⃣ NodeJs Typescript Application Starter:  
https://github.com/cobuildlab/create-nodejs-app
### 4️⃣ 8base Typescript cloud functions Starter:  
`TODO:`
### 5️⃣ React Native Typescript Starter:  
https://github.com/cobuildlab/create-react-native-typescript-app


## 2. State Library: 

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

## 3. Naming for Git Environment Branches, Feature Branches, and Pull Requests.

### Environment Branches:

- `main` for the main development branch, where all the integration happens first.
- `stage` for the staging or pre production area. Here the clients can have access to the latest stable features. 
- `master` for the main production environment.

### Feature Branches

```shell script
git checkout -b [Parent Branch][Type]/[Issue Number]-[Short Desccription]
```

- `Parent Branch`: The branch from were the new branch was branched.  
- `Type `: The purpose of the new branch, it can be: `fix`, `feature`, `tests`, `migration`, `docs`
- `Issue Number`: The github issue number.    
- `Short Description`: A Short description    


#### Example

```shell script
git checkout -b prod/fix/1224-typos
git checkout -b tests/24-add-tests
```

### Pull Requests

```shell script
[Type] [Envrionment] [Issue Number] [Short Description]
```
  
- `Type `: The purpose of the pull requests, it can be: `[fix]`, `[feature]`, `[tests]`, `[migration]`, `[docs]`
- `Environment`: The environment target of the Pull Request.
- `Issue Number`: The github issue number.    
- `Short Description`: A Short description    


#### Example

```shell script
[fix] [prod] #1128 Adding 3d Secure support
[docs] [master] #11 README
[fix] [prod] #1 Typos
```


## 4. 8base conventions: : [Here](./conventions/conventions-for-creating-8base-applications.md)
## 5. Effective Programming: A guide for conventions and best practices in software development: [Here](./conventions/effective-programming-at-cobuildlab.md)
## 6. Conventions for creating a React or React Native Application: : [Here](./conventions/conventions-for-creating-a-react-application.md)
## 7. Conventions for Javascript / Typescript source code: : [Here](./conventions/conventions-for-javascript-typescript-source-code.md)
## 8. Git branches strategy for environments deployment on web applications
