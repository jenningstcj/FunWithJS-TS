# Sequence

Sequence can be used to find every possible combination of values from an array of arrays.

```js
const prepend = element => list => [element].concat(list);

// ap applies a list of functions to a list of values
const ap = (functions, values) =>
  functions.reduce((acc, val) => acc.concat(values.map(val)), []);

const sequence = (lists) =>
  lists.reduceRight((acc, val) => console.log(acc) || ap(val.map(prepend), acc), [[]]);

const arr = [
  ["small", "medium", "large"],
  ["red", "blue", "green"],
  ["square", "circle", "star"]
];
const result = sequence(arr);
console.log(result);
```

A more imperative approach might involve nesting several for/foreach loops and would be very rigid.

---

## Examples