
# 3) **Formatting and Linting:**

We use one of the most populars libraries for javascript linting [eslint](https://eslint.org/), along with (prettier)[https://prettier.io/] for code formatting.

We enforce this tools by using (husky + lint-staged)[https://github.com/okonet/lint-staged] for files on the staging area when you do a commit to git.

To set it up:

## 1) place `.eslintrc.js` and `.prettierrc.json` 

The rules on these files depend on whether you are coding on javascript or typescript, for Frontend applications or for Backend projects.

## 2) And Add the following sections to your `package.json`:

```json
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": [
      "prettier --write",
      "eslint --fix",
      "git add"
    ]
  },
  "eslintConfig": {
    "extends": "react-app"
  }
```

## 3) Install the following libraries:

```bash
npm i --save-dev eslint-config-prettier husky lint-staged prettier
``` 

## 4) Run `npm i` 

# 4) **Naming Conventions**

Naming is important for quickly understand the purpose of an element: Classes, constants, variables or methods.

## General Rules:
- For React Components file names, use <Entity Name><Purpose><Object Type>. Example: `UserCreateView.js`, `TaskEditView.js`, `CompanyMembersListComponent.js`
- For non React Components file names, use <entity-name>-<object-type>. Example: `user-actions.js`, `user-store.js`, `company-permissions.js`
- For Module names follow file name conventions separating words using the hyphen `-`
- Don't use short or ambiguous names like: `q`, `search`, `getById`, `Member`, be specific
- A filename must always be exact to its default export
- Acronyms and initialisms should always be all uppercase or all lowercase. [Reference](https://github.com/airbnb/javascript#naming--Acronyms-and-Initialisms)

## React components

React components are divided into [Views and Components](https://cobuildlab.com/development-blog/react-patterns-container-and-presentational-components/)

- All React components, both [functional or class based](https://cobuildlab.com/development-blog/react-patterns-functional-components-vs-class-components/) must be name PascalCase. 

Example: `LoginView`, `CompanyMembersView`

- Container components are suffixed with the word `View` and presentational components can optionally be suffixed with the word `Component`. 

Example: `MyProfileView`, `ListItemComponent`

## Classes 

- Classes should always be PascalCase. 

Example: `AdminUser`, `Alliance`, `Company`

## Methods, Functions, and Instances

- Methods, functions, and instances must be always camelCase. 
- Events should always start with `on` prefix
- Non events should start with a verb

Example: `onSubmit`, `activateUser`

## Variables and Constants

- Variables must always be on camelCase.

Example: `user`, `someCalculation`

- Module level Constants must always be Uppercase with underscores for readability.

Example: `API_KEY`, `INITIAL_STATUS`, `PI`

- Function level constants adopt the rules of Variables.

## Files

- React components should live on a File with the same name of the Component with `.js` extensions

Example: `MyProfileView`, `ListItemComponent`

- Any other file must be named lowercase with hyphens for clarity

Example: `user-actions.js`, `user-store.js`, `company-permissions.js`

## Private members

- Private names of a module adopt the same previous rules of naming. In addition to this, an underscore `_` can be prefixed to explicitly indicate its condition.

Example: `_extractKeys`, `_compute`

## Events name

- Event names literals and functions callbacks to events must be named on camelCase prefixed by the word `on`

Example: `onClick`, `onLoad`, `onListMembers`


# 4) **JS Docs**

Good documentation should be part of every source code for make it easy to use, to debug and understand. 

Properly document your functions and classes following these rules:

- Use `JSDOC` to annotate comments in your source code. 
- Provide a clear explanation of what the function or class does, and it's intentions:

*PREFER THIS:*

```typescript
/**
*  Signs Up the user in the platform. Generates a token for the user an creates a record on the Session Table.
*/
const signup = () => {

}
```

*AND NOT THIS:*

```typescript
/**
*  Signs Up the user.
*/
const signup = () => {

}
```

- Provide documentation for parameters: name, type and description. 
- Provide notices of `@deprecated` indicating the reason of deprecation and the replacement mechanism.
- Document exceptions with `@throw` indicating the type of Exception and under which conditions is/are raised.
- Provide additional information for side effects like mutation of the data, use of globals, etc.

### Example:

```typescript
/**
*  Signs Up the user in the platform. Generates a token for the user an creates a record on the Session Table.
*  @deprecated since version 2.0. Use `signupWithAuth0` instead.
*
*  @param username{string} - The username of the user to sign in.
*  @param password{string} - The password of the user to sign in. The password will be validated.
*  @returns {User} - The user created in the operation with the full data from the database.
*  @throws {ValidationError} - If the username or password are invalid.
*  @throws {IntegrityError} - If the username is already used.
*/
const signup = (username: string, password: string): User => {

}
```


### Justification

- Clear documentation helps other developers to quickly understand the purpose of a function, class or sentence.
- Clear documentation helps maintain and reuse the functions and classes.
- Clear documentation helps identify bugs easier. 


# 5) **Prefer async/await syntax over Promises and callbacks**

Ref: https://github.com/sindresorhus/eslint-plugin-unicorn/issues/652


### Justification

- Easier to read and understand to new developers.
- Manage synchronous flow with asynchronous capabilities. 
- Conventional preference: Even if both are acceptable solutions, one is preferred over the other one.
