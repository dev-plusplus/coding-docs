## Keep complex logic on the body of the component with local variables and functions

Avoid clutter your rendering markup with complex conditional logic.

### DONT

```javascript 1.8
const View = () => {
    return (
        <Content>
               <Header>
                  {job.employee && job.employee.firstName ? `${job.employee.first_name} ${job.employee.last_name}` : t('JOBS.notAsigned')}
              </Header>
                <Body>
                    {condition ? {<Job key={i}></Job> } : `No rows to show`}
                </Body>
                <Button color='primary'
                  onClick={() => {
                    onChange && onChange('');
                    setShowSearchBar(false);
                    startAnimation(state.name)
                  }}
                >
                  <FaTimes />
                </Button>
        </Content>
    );
};            
```
- Complex conditional logic is hard to read and understand.
- Mixing markup with logic makes the component hard to read and understand.
- The component is not reusable, as the logic is tied to the component.
- The component is not portable, as the logic is tied to the component.


### DO

```javascript 1.8
```javascript 1.8
const View = () => {
    const employee = job.employee && job.employee.firstName ? `${job.employee.first_name} ${job.employee.last_name}` : t('JOBS.notAsigned');
    // Body
    let content = "No rows to show!";    
    if(condition && hasPermissions){
        // complex calculations
        content = items.map((item, i ) => <Job key={i}></Job>);
    }
    const onClickSearch = () => {
        onChange && onChange('');
        setShowSearchBar(false);
        startAnimation(state.name)
    }
    return (
        <Content>
               <Header>
                  {employee}
              </Header>
                <Body>
                    {content}
                </Body>
                <Button color='primary' onClick={() => onClickSearch()}>
                  <FaTimes />
                </Button>
        </Content>
    );   
};
```

* Increase component usability, debugging, and testing 
* Increase component portability as the logic is not tied to the component
* Increase component readability as the logic is not mixed with the markup
