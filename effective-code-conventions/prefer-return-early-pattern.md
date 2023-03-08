## Prefer the use the **RETURN EARLY** pattern instead of complex conditionals or nested blocks**

- The `Return Early` pattern is a way to reduce the complexity of your code by reducing the number of nested blocks and conditionals.

- This pattern reduces the complexity of your code by evaluating simple cases first and returning early if the conditions are met. This way, you can avoid nesting the rest of your code in an `if` or `else` block.

>>> NOTE: This pattern is bradly used on permission or validation checks early on functions and methods.

### **JUSTIFICATION:**

- Return early pattern forces you to first consider exceptional situations or error conditions in your functions like validations and variables or input inconsistencies on the first lines of a function or method.
- Reduce Visual Noise on the code
- Reduce complexity of conditionals
- Increase readability by excluding error and edge conditions early on the code and letting focusing on the complex part of the function


### **DON'T DO:**

```python
def is_valid_string (text, allow_empty = false):
	if text !== None :
        #
        #
            if ...
                #
                #
                #
                return True
    else:
	    return False
);

```

```javascript
const isValidString = (text, allowEmpty = false) => (
	if(text !== null){
		if(text !== undefined){
			if(allowEmpty === false){
				if (text === ''){
					return false;
				}else{
					return true
				}					
			}
			else...
		}
	}else{
		return false;
	}
);

const TableView = ({children, loading}) => {
	//	... logic
	return (
		{loading ? <Loading size={10}> :
	    <div className='inline-block'>
	        <h1 className='header-note'>
	            {children}
	            <span class='icon-header-note'>
	                <img src={icon} />
	            </span> 
	        </h1>
	    </div>}
	);
}
```


### **DO INSTEAD:**

```python
def is_valid_string (text, allow_empty = false):
	if text === None: 
		return False
	if text === '' and allow_empty == False: 
		return False
	return True
);
```

```javascript
const isDivisibleBy = (value, divisor) => (
	if(value === 0) 
		return true;
	if(divisor === 0) 
		return false;
	if(value === divisor) 
		return true;
	return value % divisor === 0;
);

const TableView = ({children, loading}) => {
	if(loading)
		return <Loading size={10} />;

	//... logic
	return (
	    <div className='inline-block'>
	        <h1 className='header-note'>
	            {children}
	            <span class='icon-header-note'>
	                <img src={icon} />
	            </span> 
	        </h1>
	    </div>
	);
}
```
