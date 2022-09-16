## Don't use more than one sentence in a try/catch block**

Is a common mistake for new developers to surround long function calls or views inside try-catch blocks as a failsafe or catch all exception handling. But in fact, this is one sign of ineffective code.

Multi sentence try catch blocks are **ineffective** because:

- Difficult to debug, as is not 100% clear which sentences are generating an exception.
- Promote a bad pattern of handling all exceptions in the same way
- Usually this broad catch blocks make use of a generic and catch all object or class that does not prepare your code for unknown / undesired scenarios, and promotes the handling of all the exceptions in the same way. 

Keep your code predictable, robust, solid and **effective** by:

- For readability and robustness keep your try/catch blocks limited to 1 sentence
- Use specific Exceptions subclasses and not broads classes or definitions to prevent unknown scenarios to behave like known scenarios
- For easier bug hunting, make sure to handle different Exceptional scenarios appropriately  



### Don't do
```js
try{
    // Multiple statements
    ...//
    ...//
    ...//
}catch(e){
    // Catch all Logic
}

```


### Do instead:

```js
try{
    // Single statement that can generate an exception
}catch(e){
    // Catch all Logic
}
...
// Code that is not expected to generate an exception

try{
    // Single statement that can generate an exception
}catch(e){
    // Catch all Logic
}

```
