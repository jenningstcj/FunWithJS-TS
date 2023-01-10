# Either

Either, sometimes called Result, is another type of context (see [of](JS/of.md) and [maybe](JS/maybe.md) for other examples) that can help us with error handling without having to check for null or errors at every turn.  Also see [Railway Oriented Programming](https://fsharpforfunandprofit.com/rop/) for more information on this concept.

```js
export const Ok = x => ({
  map: f => Ok(f(x)),
  chain: f => f(x),
  fold: (f, g) => g(x),
  log: () => {
    console.log(`Ok(${JSON.stringify(x)})`);
    return Ok(x);
  }
});

export const Error = x => ({
  map: f => Error(x),
  chain: f => Error(x),
  fold: (f, g) => f(x),
  log: () => {
    console.error(`Error(${JSON.stringify(x)})`);
    return Error(x);
  }
});

export const fromNullable = x => (x != null ? Ok(x) : Error(null));

export const tryCatch = f => {
  try {
    return Ok(f());
  } catch (e) {
    return Error(e);
  }
};
```

---

## Examples

We can use our familiar example of parsing a JWT, but in this case we can update several of our functions that could potentially run into errors with error handling using the `tryCatch` helper function above that returns an `Ok` or `Error`.  Since these functions return a new `Result` or `Ok/Error` then we can use `chain` to compose our functions together:

```js
const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) =>  tryCatch(() => token.split(".")[1]);
const atob = (str) => tryCatch(() => new Buffer(str, "base64")); //handle atob for Node
const parse = (obj) => tryCatch(() => JSON.parse(obj));
const retrieveProperty = (propName) => (obj) => obj[propName];
516239022;

const eitherResult = fromNullable(jwt)
  .log()
  .chain(getTokenPayload)
  .chain(atob)
  .chain(parse)
  .map(retrieveProperty("exp"));

eitherResult.fold(
  (err) => console.error(`uh oh we got ${err}`),
  (_) => console.log(`yay we got ${JSON.stringify(_)}`)
); // yay we got 1516239022 
```

What would happen if we passed in a null value?

```js
const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) =>  tryCatch(() => token.split(".")[1]);
const atob = (str) => tryCatch(() => new Buffer(str, "base64")); //handle atob for Node
const parse = (obj) => tryCatch(() => JSON.parse(obj));
const retrieveProperty = (propName) => (obj) => obj[propName];
516239022;

const eitherResult = fromNullable(null)
  .log()
  .chain(getTokenPayload)
  .chain(atob)
  .chain(parse)
  .map(retrieveProperty("exp"));

eitherResult.fold(
  (err) => console.error(`uh oh we got ${err}`),
  (_) => console.log(`yay we got ${JSON.stringify(_)}`)
); // uh oh we got null 
```

By passing in null, our `Either` safely handles the value and skips executing any of the functions are returns our result.

What about a string that when split, does not contain and index of 1?

```js
const getTokenPayload = (token) =>  tryCatch(() => token.split(".")[1]);
const atob = (str) => tryCatch(() => new Buffer(str, "base64")); //handle atob for Node
const parse = (obj) => tryCatch(() => JSON.parse(obj));
const retrieveProperty = (propName) => (obj) => obj[propName];
516239022;

const eitherResult = fromNullable("test.")
  .log()
  .chain(getTokenPayload)
  .chain(atob)
  .chain(parse)
  .map(retrieveProperty("exp"));

eitherResult.fold(
  (err) => console.error(`uh oh we got ${err}`),
  (_) => console.log(`yay we got ${JSON.stringify(_)}`)
); // uh oh we got SyntaxError: Unexpected end of JSON input 
```

By passing in a string that when split does not contain an index of 1, ordinarily our base64 decode or JSON parse functions would throw an exception.  However, our tryCatch helper that returns an Either catches the exception and returns the Error value.

Each step of the way our Either error handling safely handles any issues and returns the appropriate Ok or Error.  This makes exception handling very easy.