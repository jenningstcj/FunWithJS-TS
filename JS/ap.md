# AP

`AP` applies a list of functions to a list of values.  This can seem kind of odd at first, but there are useful applications.  

```js
const ap = (functions, values) =>
  functions.reduce((acc, val) => acc.concat(values.map(val)), []);
```

---

## Examples

```js
const ap = (functions, values) =>
  functions.reduce((acc, val) => acc.concat(values.map(val)), []);


const names = ["Tyler", "Garrett", "Avery"];

const uppercase = (str) => str.toUpperCase();
const split = str => str.split('').join(' ');
const celebrate = str => `!!!!!!${str}!!!!!`;

const r = ap([uppercase, split, celebrate], names);
console.log(r);
/*
[ 'TYLER',
  'GARRETT',
  'AVERY',
  'T y l e r',
  'G a r r e t t',
  'A v e r y',
  '!!!!!!Tyler!!!!!',
  '!!!!!!Garrett!!!!!',
  '!!!!!!Avery!!!!!' ]
*/
```

```js
const r2 = ap([x => 10/x, x => 100/x], [2,5]);
console.log(r2);
// [ 5, 2, 50, 20 ] 
```

See [sequence](JS/sequence.md) for another example.