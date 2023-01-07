# Compose

Compose is similar to [pipe](JS/pipe.md), but composes right to left instead of left to right.

```js
export const compose = (...fns) =>
  fns.reduce(
    (acc, val) =>
      (...args) =>
        acc(val(...args))
  );
```

---

## Examples

```js
const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) => token.split(".")[1];
const atob = (str) => new Buffer(str, "base64"); //handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];

const readExpiration = compose(
  retrieveProperty("exp"),
  parse,
  atob,
  getTokenPayload
);

const result = readExpiration(jwt);

console.log(result); // 1516239022
```