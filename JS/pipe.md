# Pipe

Pipe is an easy way to compose functions together so that each function passes it's result as the next argument to the next function.  Pipe composes the functions from left to right.

```js
const pipe = (...fns) =>
  fns.reduce(
    (acc, val) =>
      (...args) =>
        val(acc(...args))
  );
```

---

## Examples

To use the example of parsing the expiration from a JWT from the [of](JS/of.md) example:

```js
const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) => token.split(".")[1];
const atob = (str) => new Buffer(str, "base64"); //handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];

const readExpiration = pipe(
  getTokenPayload,
  atob,
  parse,
  retrieveProperty("exp")
);

const result = readExpiration(jwt);

console.log(result); // 1516239022
```

This creates a new function by piping functions together to read a JWT and parse out the expiration value.