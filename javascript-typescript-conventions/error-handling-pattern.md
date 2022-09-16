This is an alternative to the conventional try/catch block.

## JUSTIFICATION:
- The use of the Try / Catch block is very open, leading developers to implement bad practices: [REFERENCE](https://github.com/cobuildlab/coding-docs/blob/main/effective-code-conventions/avoid-multi-statement-try-catch-block.md)
- With the use of these functions we aim to avoid these bad practices and provide a consistent single way of managing exceptions in Javascript and Typescript


### Don't Do

```js
let result;
try {
 result = await action();
}catch (Exception e){
     // error management
    throw e;
}
```

### Do Instead:


```js
import {tryCatch, tryCatchSync} from "@cobuildlab/error-handling";

// Async code
const [result, error] = await tryCatch(action());
if (error){
    // error management
}

// Sync code
const [result, error] = tryCatchSync(action());
if (error){
    // error management
}

```

Pending: https://github.com/cobuildlab/error-handling
