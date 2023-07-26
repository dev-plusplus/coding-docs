## **Folder Structure**

> **NOTE: This convention only affects coding style. Does not affect performance or readability, only consistency. And falls in the category of "choosing a single way of doing a task"**

Organizing files is one of the most important conventions to keep a consistency and easy to maintain Application. Especially for large codebases and large teams.

I will only consider the structure under the `src` folder. Other folders and files of the project structure depends on other factors like the platform, linting tools, environment files, version control tools, react version, etc, and we will not cover that here.

I use the terms **App Specific** to resources that can be used across the entire app, and **Module Specific** to resources that can only be used inside a module folder.

Based in my experience for the last years working with large applications, this is the proposed file Structure with examples:

```
src/ 
└───modules/
│   └───login/
|	│   └───components/
|	|	|	└───LoginForm.jsx
|	|	|	└───LoginView.jsx
|	|	|	└───styles.js
|	|	|	└───styles.scss
|	│   └───state/
|	|	|	└───store.js
|	|	|	└───actions.js
|	|	|	└───selectors.js
|	|	|	└───reducers.js
|	|	└───services.js
|	|	└───models.js
|	|	└───permissions.js
|	|	└───handlers.js
|	|	└───queries.js
|	|	└───events.js
|	|	└───...
│   └───management/						// Nested modules
|	│   └───school/
| 	|	│   └───components/
|	|	|	|	└───DashboardGraph.jsx
|	|	|	|	└───NavitationBar.jsx
|	|	|	|	└───SchoolCreateView.jsx
|	|	|	|	└───SchoolUpdateView.jsx
|	|	|	|	└───SchoolDetailView.jsx
|	|	|	└───actions.js
|	|	|	└───utils.js
|	|	|	└───events.js
|	|	|	└───...
|	│   └───house/
| 	|	│   └───components/
|	|	|	|	└───AnotherView.js
|	|	|	|	└───...
|	|	|	└───store.js
|	|	|	└───...
└───shared/
│   └───navigation/
│   |   └───navigator.js
│   └───components/
│	|   └───buttons/
|	|	|	└───MainButton.jsx
│	|   └───text/
│	|   └───forms/
|   │   SomeComponent.jsx
|   │   AnotherUnclassifiedComponent.jsx
│   └───constants/
│       │   user-types.js
│   └───typings/
│       │   ...
    └───assets/
│   |   └───images/
|	|   |	└───logo.png
|	|   |	└───background_main.jpg
|	|   |	└───...
│   string-utils.js
│   validation-utils.js
│   routes.js
│   app.js
 ```

### General Rules

- Follow the naming convention proposed in this repository. [See Naming Convention](https://github.com/dev-plusplus/coding-docs/blob/main/javascript-typescript-source-code/naming-convention.md)
- Javascript files (modules) can contain multiple classes, functions, constants, etc, so by design the name on a file (module) should refer to a grouping of symbols with a purpose. The only exception for this is the React Components, that should be named with the same name of the file by convention.
- Don't use `index.js` files to export members of complicated folder structures. Instead, don't use complicated folder structures in application code. If you have a complicated folder structure, perhaps you should consider refactor it into a library to abstract the complexity. [See why not to use index files on Application Code](https://github.com/dev-plusplus/coding-docs/blob/main/javascript-typescript-source-code/avoid-import-export-from-index-on-application-modules.md)
- If the number of files in a folder grows beyond 10, consider branching into a new folder.
- Don't create folders (modules) to host 1 or 2 purpose-specific components. Instead, create a folder (module) to host a group of components that are related to a specific purpose. 

###  `modules` folder
Main folder to hold the Module Specific folders 

**IMPORTANT:**

The most non-deterministic task in this structure is to decide which ones are the "modules" of your Application. There is no rule of thumb for this, but I will give you the closest thing that I've used for this in frontend and backend applications:

**Frontend:** Use as modules the highest elements of the Navigation of the UI. For Example: Users, Administration, Inbox, Notifications, My Profile, Invoices, Tickets, etc. And branch sub modules from it if necessary. 

**Backend** Use as modules the strong entities of the Business Object Domain. For Example: Users, Invoices, Tickets, etc

**EXTRA**: You can add any additional modules that you consider is Application Specific and that can be beneficial to isolate in its own folder. 

### `shared` folder
App Specific folder to hold any asset that is not an image and that it should be shared across modules.

Think about this folder as any potential code that could be put in a node package and be distributed as a library.

### *assets* folder
App Specific folder that contains images files like png, svg, jpg, etc

## *components* folder
React components that can be App specific under `shared/components/` or module specific under: `modules/<module-name>/components/`

## *\*-actions* files
Module specific files that holds Actions of the module. (See Architecture)

## *\*-models* files
Module specific files that holds constants and Model Objects or Classes.

## *\*-store* files (or redux)
Module specific files that that holds the store and state components. (See Architecture)

## *\*-events* files
Module specific files that that holds the events constants. (See Architecture)

## *\*-permissions* files
Module specific files that that holds functions related to permissions.

## *\*-utils* files
Functions or Classes that can be App specific under `shared/` or module specific under: `modules/<module-name>/`
