# General coding conventions

## 1. Project Conventions:

These are the things that every project must have:

### ESLINT Configuration

Every project must have an `eslintrc.js` with the standard configuration that we have decided for the company projects:

- For nodejs applications: [NodeJS ESLint configuration file](../assets/template-nodejs-.eslintrc.js)
- For React Typescript applications: [React Typescript ESLint configuration file](../assets/template-react-.eslintrc.json)
- [ESLint Ignore](../assets/template-.eslintignore)

### Prettier Configuration

Every project must have an `.prettierrc.json` with the standard configuration for the prettier formatting:

- For javascript applications: [Prettier configuration file](../assets/template-.prettierrc.json)
- [Prettier Ignore](../assets/template-.prettierignore)

### Github Actions for: testing, building and linting

Every project must have automatic actions on Pull Requests to the main branches for running linting, building and testing

- For typescript projects: [Github Workflow for Linting, Building and Testing](../assets/github/template-buildd-eslint.yml) 

### Husky + Lint Stageg for pre-commit and pre-push git hooks

Every project must have automatic pre-commit and pre-push actions with `husky` and `lint-stageg` on the package.json:

```json
  "husky": {
    "hooks": {
      "pre-commit": "npx lint-staged",
      "pre-push:": "npm test"
    }
  },
  "lint-staged": {
    "*.{js,ts}": [
      "prettier --write",
      "eslint --fix",
      "npm run build"
    ]
  }
```

### Jest configuration and example test

Every project must have Jest configurated. Frontend or backend automated testing are not a mandatory requirement but a desired feature. 

### Git ignore file
### npm ignore file

For projects that are published in the npm directory, we needd to exclude certain files and folders of being uploaded with the package:

- .npmignore
```
src
tsconfig.json
tslint.json
.prettierrc
```

### Github Pull Request / Issues Templates

For every project it is required to have the Pull Requests and Issue templates for the Github Platform:

- [Issues Templates](../assets/github/ISSUE_TEMPLATE)
- [Pull Request Template](../assets/github/pull_request_template.md)


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
  
- `Type `: The purpose of the pull requests, it can be: `[fix]`, `[feature]`, `[tests]`, `[migration]`, `[docs]`, `[release]`
- `Environment`: The environment target of the Pull Request.
- `Issue Number`: The github issue number.    
- `Short Description`: A Short description    


#### Example

```shell script
[fix] [prod] #1128 Adding 3d Secure support
[docs] [master] #11 README
[fix] [prod] #1 Typos
[release] [v2.0.1] [prod <- main] New onboarding module
```
### Versioning
```
8.4.9
[FIRST#].[SECOND#].[THIRD#]
```
| `First Number` | `Second Number` | `Third Number` | 
|----------------|-----------------|----------------|
| Major changes that affects the whole app such as conceptual changes, new, altered or removed features or modules, Major UI changes, Comprehensive Releases.| New features or alterations, changes that don't overwhelmingly alter the funtionality of the project, partial UI Updates, Major Bug fixes and security enhacements (dev stage). | Minor Bug fixes, UI fixes, security enhacements, mainteance. |
 

#### Example

```
0.0.0 | New Project
1.0.0 | Release of a brand new onboarding module
1.1.0 | Added a new credit card feature to the existing onboarding module
1.1.1 | Hotfix pushed because credit card field was not working correctly
```
