# General coding conventions

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


## 2. Naming for Git Environment Branches, Feature Branches, and Pull Requests.

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