## **Avoid import/export from `index.js` on application modules**

> **NOTE: This convention only affects code style. Does not affect performance or readability, only consistency. And falls in the category of "choosing a single way of doing a task"**
 
> **NOTE: This convention is not applicable to libraries or shared node packages, just for Application Development**

Avoid module exports from index.js in the codebase. For some developers this is a common practice to avoid long paths of imports, example:

Having these components:

```js
modules/A/View1.js
modules/A/View2.js
modules/B/View3.js
```

To use `View1.js` and `View2.js` in `View3.js`

*PREFER THIS:*

`modules/B/View3.js`
```js
import {View1} from "../A/View1";
import {View2} from "../A/View2";

const View3 = () => {
  return(
      <>
        <View1/>
        <View2/>
    </>
  )
}
```

*AND NOT THIS:*

`modules/A/index.js`
```js
import {View1} from "View1";
import {View2} from "View2";
```

`modules/B/View3.js`
```js
import {View1, View2} from "../A";

const View3 = () => {
    return(
        <>
            <View1/>
            <View2/>
        </>
    )
}
```


### Justification

* Maintaining an unnecessary file is usually a source of errors. Adopting this technique requires creating components and remembering to export them in their index.js module file.
* The real purpose of this strategy is `encapsulation`, hiding the intrinsics of a module to the outside world, which is rarely the case for Application Development.
* Keeping this option open to do it, or not to do it, without a proper set of rules creates inconsistency in the source code of a project.
* Complete imports give an exact location of the resource, making easier the debugging process.
* Tools that rely on introspection like type checkers, doc generators or IDE navigation or hints, won't work well with this approach.
* More than style there is no real gain on do module exporting.
