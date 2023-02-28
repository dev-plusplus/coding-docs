

## **Enforce the difference Between Presentational Components and Container Components (Views)**

Reference: [React Patterns Presentational and Container Components]([https://cobuildlab.com/development-blog/react-patterns-container-and-presentational-components/](https://cobuildlab.com/development-blog/react-patterns-container-and-presentational-components/))

React components can be classified in 2 major groups depending on how they fit in the Architecture of your application, and how they interact with the App and the User:

### 1) **Presentational Components** 

Or just Components are responsible present or render the user interface. They sit in the middle of the communication between the User and the Application State only through the **Container Components**.

### 3) **Container Components** 

Or Views, or Controller, are responsible for "connecting" the application state with the User Interface by listening to changes to the Application State and rendering **Presentational Components**. The way they interact with the Application State or stores depends of the technology used (Redux, Flux, MobX, Context API)


| Feature | Presentational Components (Components)                                                                                                   | Container Components (Views)                                  |
|---|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| **External Communication** | They are not connected directly to the Application State nor listen to events. Their communication is handled via functions or arguments | They subscribe and react to changes on the Application State. |
| **Internal State** | Manages little or no state. Complex presentational components like virtualized lists, manage internal state for optimized rendering.     | Handled internally with local state or state variables.       |
| **Renders**| Mostly understands **HOW** to render                                                                                                     | It understands **WHAT** to render.                            |

