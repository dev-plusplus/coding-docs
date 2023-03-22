# Manage errors and exceptional situations with Exceptions

Is a common mistake for developers with little experience in large code bases or learning a new programming language to not use exceptions to handle errors and exceptional situations. But in fact, this is one sign of ineffective code.

Errors and Exceptions constructs are a fundamental part of any programming language. They are used to handle exceptional situations and errors. They are a way to communicate to the user of the function what happened and what to do next and is not a good practice to replace them with return values such as False, null, None or void, depending on the language.

## DON'T

```js
function doSomethingHeavy() {
    try {
        const file = fs.readFileSync('file.txt'); // Statement that throws an exception
        const content = parseFile(file); // Statement that throws an exception
        return count(content);         // Statement that throws an exception
    } catch (e) {
        // Catch all Logic
        return false;
    }
}

function trySomethingComplicated(a, b) {
    let c;
    try {
        c = a / b; // Statement that throws an exception
    } catch (e) {
        return 0;
    }
}
```

- Difficult to debug, as is not 100% clear which sentences are generating an exception. An error here, can be from any of the 3 sentences.
- Returning different types is a bad practice, even for dynamic languages like javascript. 
- Promote a value to mean an error scenario means that any other function that want to use this value as a valid return will have a different expected behavior.
- The user of the function doesn't have information about what happened and what to do next.

## DO

```js
function doSomethingHeavy() {
    let file;
    try {
        file = fs.readFileSync('file.txt'); // Statement that throws an exception
    } catch (e) {
        if (e instanceof FileException) {
            throw new FileNotFoundError('File not found');
        } else {
            throw e; // Propagate error and crash the app or maybe just log the error.
        }
    }
    let content;
    try {
        content = parseFile(file); // Statement that throws an exception
    } catch (e) {
        if (e instanceof ParseFileException) {
            throw new ParseFileError('Malformed file');
        } else {
            if (e instanceof FileException) {
                throw new FileNotFoundError('Definitions not found');
            } else {
                throw e; // Propagate error and crash the app or maybe just log the error.
            }
        }
    }
    let sizeOf;
    try {
        sizeOf =  count(content);         // Statement that throws an exception
    } catch (e) {
        // Catch all Logic. Propagate erros from native function.
        throw e;
    }
}

function trySomethingComplicated(a, b) {
    let c;
    try {
        c = a / b; // Statement that throws an exception
    } catch (e) {
        if (e instanceof DivideByZeroException) {
            throw new DivideByZeroError('Cannot divide by zero');
        } else {
            throw e; // Propagate error and crash the app or maybe just log the error.
        }
    }
}
```

- Well-designed API must be sensitive to how the user of the function will be used. If you return a value that is not the expected type, the user of the function will have to check the type of the return value and handle the error scenario. This is not a good practice.
- Meaningful values like false or null or None or void do not represent properly an error scenario.
- Raising exceptions allows you to be consistent with the result type of the function, in terms of types, shape, etc.
- Provides Contextual information: Exceptions allow to communicate to the user of the function what happen and where.
- Avoids boilerplate code requiring to users of the API for comparing return values.
