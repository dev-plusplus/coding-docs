## **Folder Structure**

> **NOTE: This convention only affects code style. Does not affect performance or readability, only consistency. And falls in the category of "choosing a single way of doing a task"**

Organizing files is one of the most important conventions to keep a consistency and easy to maintain Application. Especially for large codebases and large teams, specially remote teams.

I will only consider the structure under the `src` folder. Other folders and files of the project structure depends on other factors like the platform, linting tools, environment files, version control tools, react version, etc, and we will not cover that here.

I use the terms **App Specific** to resources that can be used across the entire app, and **Module Specific** to resources that can only be used inside a module folder.


Based in my experience for the last years working with large applications, I propose this file Structure with examples:

```
src/ 
└───modules/
│   └───login/
|	│   └───components/
|	|	|	└───LoginForm.js
|	|	|	└───LoginView.js
|	|	└───actions.js
|	|	└───models.js
|	|	└───permissions.js
|	|	└───services.js
|	|	└───handlers.js
|	|	└───queries.js
|	|	└───store.js
|	|	└───events.js
|	|	└───...
│   └───management/						// Nested modules
|	│   └───school/
| 	|	│   └───components/
|	|	|	|	└───DashboardGraph.js
|	|	|	|	└───NavitationBar.js
|	|	|	|	└───SchoolCreateView.js
|	|	|	|	└───SchoolUpdateView.js
|	|	|	|	└───SchoolDetailView.js
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
│   └───components/
│	|   └───buttons/
│	|   └───text/
│	|   └───forms/
|   │   SomeComponent.js
|   │   AnotherUnclassifiedComponent.js
│   └───constants/
│       │   user-types.json
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
│   App.js
 ```

###  `modules` folder
Main folder to hold the Module Specific folders 

**IMPORTANT:**

The most non-deterministic task in this structure is to decide which ones are the "modules" of your Application. There is no rule of thumb for this, but I will give you the closest thing that I've used for this in frontend and backend applications:

**Frontend:** Use as modules the highest elements of the Navigation of the UI. For Example: Users, Administration, Inbox, Notifications, My Profile, Invoices, Tickets, etc

**Backend** Use as modules the strong entities of the Business Object Domain. For Example: Users, Invoices, Tickets, etc

**EXTRA**: You can add any additional modules that you consider is Application Specific and that can be benefitial to isolate in its own folder. 

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

## *\*-store* files
Module specific files that that holds the store components. (See Architecture)

## *\*-events* files
Module specific files that that holds the events constants. (See Architecture)

## *\*-permissions* files
Module specific files that that holds functions related to permissions.

## *\*-utils* files
Functions or Classes that can be App specific under `shared/` or module specific under: `modules/<module-name>/`

## *\*.css* files
As of these conventions the project should only have one `index.css` file, that holds general purpose styles like: Text, body styles, etc.

