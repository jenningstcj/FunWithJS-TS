# Compose

```js
export const compose = (...fns) =>
  fns.reduce((acc, val) => (...args) => acc(val(...args)));
```