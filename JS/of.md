# Of

Creates a context to perform operations.  In thise case, using the array prototype as the context to provide operations on an object such as `map`.  Contexts wrap our object and give us special capabilities that we would not have otherwise.

```js
const of = (...args) => [...args];
```

---

## Examples

We can use `of` to easily wrap an object and use `map` to perform operations on the data.

```js
const of = (...args) => [...args];

const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) => token.split(".")[1];
const atob = (str) => new Buffer(str, "base64");//handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];

const result = of(jwt)
  .map(getTokenPayload)
  .map(atob)
  .map(parse)
  .map(retrieveProperty("exp"))[0];

console.log(result); // 1516239022

```


In this example, we could also create a context equivalent to our needs with:

```js
const of = (val) => ({
    map: (fn) => of(fn(val)),
    result: () => val
});
```

This would allow us to change the final `[0]` that unwraps the array in the first version to a `.result()` in this version:

```js
const of = (val) => ({
    map: (fn) => of(fn(val)),
    result: () => val
});

const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) => token.split(".")[1];
const atob = (str) => new Buffer(str, "base64");//handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];

const result = of(jwt)
  .map(getTokenPayload)
  .map(atob)
  .map(parse)
  .map(retrieveProperty("exp"))
  .result();

console.log(result); // 1516239022
```