
##  **Naming Conventions**

Naming is important for quickly understand the purpose of an element: Classes, constants, variables or methods.

## General Rules:
- For every javascript module (folder or file) use kebab-case. Except for React Components, that should be named with the same name of the file by convention in a PascalCase format.
- Start by reading the folder structure conventions [here](https://github.com/dev-plusplus/coding-docs/blob/main/javascript-typescript-source-code/folder-structure.md)
- For React Components file names, use <Entity Name><Purpose><Object Type>. Example: `UserCreateView.js`, `TaskEditView.js`, `CompanyMembersListComponent.js`
- For React Components that group other components to present information to the user, use the suffix `View` instead of `Screen` or `Page`.
- For non React Components file names, use <entity-name>-<object-type>. Example: `user-actions.js`, `user-store.js`, `company-permissions.js`. 
- Don't use short or ambiguous names like: `q`, `search`, `getById`, `Member`, be specific
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

- Enums follow a PascalCase convention. And its members a camelCase convention.

## Files

- React components should live on a File with the same name of the Component with `.jsx` extensions

Example: `MyProfileView`, `ListItemComponent`

- Any other file must be named lowercase with hyphens or kebab-case for clarity.

Example: `user-actions.js`, `user-store.js`, `company-permissions.js`


## Events name

- Event names literals and functions callbacks to events must be named on camelCase prefixed by the word `on`

Example: `onClick`, `onLoad`, `onListMembers`
