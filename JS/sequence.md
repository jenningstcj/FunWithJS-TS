# Sequence

Sequence can be used to find every possible combination of values from an array of arrays.  This function utilizes a couple of other functions we have already looked at such as [prepend](JS/prepend.md) and [ap](JS/ap.md).

```js
const prepend = (element) => (list) => [element].concat(list);

// ap applies a list of functions to a list of values
const ap = (functions, values) =>
  functions.reduce((acc, val) => acc.concat(values.map(val)), []);

const sequence = (lists) =>
  lists.reduceRight(
    (acc, val) => ap(val.map(prepend), acc),
    [[]]
  );
```

---

## Examples

```js
const arr = [
  ["small", "medium", "large"],
  ["red", "blue", "green"],
  ["square", "circle", "star"]
];
const result = sequence(arr);
console.log(result);
/*
[ 
  [ 'small', 'red', 'square' ],
  [ 'small', 'red', 'circle' ],
  [ 'small', 'red', 'star' ],
  [ 'small', 'blue', 'square' ],
  [ 'small', 'blue', 'circle' ],
  [ 'small', 'blue', 'star' ],
  [ 'small', 'green', 'square' ],
  [ 'small', 'green', 'circle' ],
  [ 'small', 'green', 'star' ],
  [ 'medium', 'red', 'square' ],
  [ 'medium', 'red', 'circle' ],
  [ 'medium', 'red', 'star' ],
  [ 'medium', 'blue', 'square' ],
  [ 'medium', 'blue', 'circle' ],
  [ 'medium', 'blue', 'star' ],
  [ 'medium', 'green', 'square' ],
  [ 'medium', 'green', 'circle' ],
  [ 'medium', 'green', 'star' ],
  [ 'large', 'red', 'square' ],
  [ 'large', 'red', 'circle' ],
  [ 'large', 'red', 'star' ],
  [ 'large', 'blue', 'square' ],
  [ 'large', 'blue', 'circle' ],
  [ 'large', 'blue', 'star' ],
  [ 'large', 'green', 'square' ],
  [ 'large', 'green', 'circle' ],
  [ 'large', 'green', 'star' ] 
]
*/
```

A more imperative approach might involve nesting several for/foreach loops and would be very rigid and require modification anytime the number of arrays changed.

```js
for(x in arr1){
  for(y in arr2){
    for(z in arr3){
      c.push([arr1[x], arr2[y], arr3[z]]);
    }
  }
}
```