# AP

```js
// ap applies a list of functions to a list of values
const ap = (functions, values) =>
  functions.reduce((acc, val) => acc.concat(values.map(val)), []);
```