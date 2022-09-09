
## **Separate LAYOUT Components from UI Components**

Layout Components: Are components that defining positioning in elements or structure, 

UI Components: Components that define how an element looks and behave. 

Avoid mixing in components layout properties with look and feel properties unless is strictly necessary.

*PREFER THIS:*

```javascript 1.8
const View = () => {
    return (<div className='float-right'> // Layout Components
              <OptionButton text='Create' onClick='' icon='new' /> // UI Components
          </div>);
};
```

*AND NOT THIS:*
```javascript 1.8
const View = () => {
    return (
          <Button onClick={this.goToImportDeals} style={{ position: 'absolute', right: '0px' }}>
                Import Deals
          </Button>);
};
```
