## **Components should not include styling props**

Avoid using `style` or `className` for Components in Views to reduce visual noise, unless it is completely necessary.

Rely on abstractions for defining styles for your components.

See: [Proxy Pattern](https://cobuildlab.com/development-blog/react-patterns-proxy-pattern-for-components/)


*PREFER THIS:*

```javascript
const NormalText = ({children}) => (
    <p className='blue-text'>
        {children}
    </p>
);

const H1 = ({children, icon}) => (
    <div className='inline-block' style={{borderColor:'blue'}}>
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

### Exceptions:

* Layout Components: Components that are in charge of "where" does an element gets render instead of a Presentation Component, that is in charge on "how" an element looks and feel
* Layout properties: Properties related to positioning and sizing
