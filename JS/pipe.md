# Pipe

```js
const pipe = (...fns) => fns.reduce((acc, val) => (...args) => val(acc(...args)));
```