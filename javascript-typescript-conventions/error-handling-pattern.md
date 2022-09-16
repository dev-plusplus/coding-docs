This is an alternative to the conventional try/catch block.

## JUSTIFICATION:
- The use of the Try / Catch block is very open, leading developers to implement bad practices: [REFERENCE](https://github.com/cobuildlab/coding-docs/blob/main/effective-code-conventions/avoid-multi-statement-try-catch-block.md)
- With the use of these functions we aim to avoid these bad practices and provide a consistent single way of managing exceptions in Javascript and Typescript


### Don't Do

```
let result;
try {
 result = await action();
}catch (Exception e){
     // error management
    throw e;
}
```

### Do Instead:


```
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


```js
/**
 * Short version of try and catch
 * @param promise
 * @param finallyFunction
 * @returns {*}
 */
exports.tryAndCatch = (promise, finallyFunction = null, throwIfError = false) => {
  return promise
    .then((result) => {
      return [result, null];
    })
    .catch((error) => {
      if (throwIfError === true) {
        throw error;
      }
      return [null, error];
    })
    .finally(() => {
      if (finallyFunction !== null) {
        if (typeof finallyFunction === 'function') {
          finallyFunction();
        }
      }
    });
};


/**
 * @param {Function | Promise} promise - Promise function to resolve.
 * @returns {Promise<[Error, undefined]|[null, object]>} Promise - Promise function to resolve.
 */
export async function handleTryCatch<T>(
  promise: (() => Promise<T>) | Promise<T> | (() => T),
): Promise<[T, undefined] | [undefined, Error]> {
  const currentPromise = typeof promise === 'function' ? promise() : promise;

  try {
    const result = await currentPromise;
    return [result, undefined];
  } catch (error: unknown) {
    return [undefined, error as Error];
  }
}
```
