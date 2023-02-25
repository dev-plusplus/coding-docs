## **No UI components definition in a Component Level**

UI, or look and feel properties, are CSS properties that modify how an element is drawn in the user interface.

Avoid using UI properties, or defining UI components directly in Views.

[Reference](./separate-layout-components-from-ui-components.md)

Rely on abstractions for UI components, and place those components on `shared` folders. 

*PREFER THIS:*

```javascript
const NormalText = ({children}) => (
    <p className='blue-text'>
        {children}
    </p>
);

const H1 = ({children, icon}) => (
    <div className='inline-block'>
        <h1 className='header-note'>
            {children}
            <span class='icon-header-note'>
                <img src={icon} />
            </span> 
        </h1>
    </div>
);

export class View extends Component {
    render(){
        <section>
            <H1 icon={plus}>Note Header</H1>
            <NormalText>Note Text</NormalText>
        </section>
    }
}
...
```

*AND NOT THIS:*

```javascript
export class View extends Component {
    render(){
        <section>
            <div className='inline-block'>
                <h1 className='header-note'>
                    Note Header
                    <span class='icon-header-note'>
                        <img src={plus} />
                    </span> 
                </h1>
            </div>
        <p className='blue-text'>
            Note Text
        </p>
        </section>
    }
}
...
```
### Justification:

* Reduce Visual Noise
* Increase component readability
* Increase speed of development avoiding design decisions
* Maintainability by isolation of the styling options
