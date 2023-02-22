## Don't use more than one sentence in a try/catch block**

Is a common mistake for new developers to surround long function calls or views inside try-catch blocks as a failsafe or catch all exception handling. But in fact, this is one sign of ineffective code.

### DON'T: Multi sentence try catch blocks are **ineffective** because:

```js
try{
    doSomethingHeavy();                       // Statement that throws an exception
    const result = trySomethingComplicated(); // Statement that throws an exception
    const {a,b} = result;                     // Statement that do not throw an exception
    combineResults(a,b,otherVariable);        // Statement that throws an exception
}catch(e){
    // Catch all Logic
}
```

- Difficult to debug, as is not 100% clear which sentences are generating an exception. An error here, can be from any of the 3 sentences.
- Promote a bad pattern of handling all exceptions in the same way. This is not always the case, and can lead to unexpected behaviors
- Impossible to code recovery logic for specific scenarios. If `doSomethingHeavy` fails, you can't recover from it. 

### DO: Keep your code predictable, robust, solid and **effective** by:


### Do instead:

```js
try {
    doSomethingHeavy();                       // Statement that throws an exception
} catch (e) {
    if (e instanceof HeavyException) {
        // Handle Heavy Exception
    } else {
        throw e; // Propagate error and crash the app or maybe just log the error.
    }
}

let result;
try {
    result = trySomethingComplicated(); // Statement that throws an exception
} catch (e) {
    result = {a: 0, b: 0}; // Handle exception and set default values
}

try {
    combineResults(a, b, otherVariable);        // Statement that throws an exception
} catch (e) {
    if (e instanceof CombineException) {
        return NaN;
    } else {
        throw e; // Propagate error and crash the app or maybe just log the error.
    }
}

```

- For readability and robustness keep your try/catch blocks limited to 1 sentence.
- Code that does not throw exceptions should not be inside a try/catch block.
- Use specific Exceptions subclasses and not broads classes or definitions to prevent unknown scenarios to behave like known scenarios
- For easier bug hunting, make sure to handle different Exceptional scenarios appropriately  
