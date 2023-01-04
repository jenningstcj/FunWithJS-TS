# Range

Creates an array of a range of numbers.

```js
export const range = (start, end) => [...Array(end + 1).keys()].slice(start);
```