# Maybe

Maybe is a context that allows you to handle null and undefined in an easy manner without having to manually check for a value at every turn.  See intro on context in [of](JS/of.md).

```js
const Just = (x) => ({
  map: (f) => Maybe(f(x)),
  chain: (f) => f(x),
  fold: (f, g) => g(x),
  log: () => console.log(x)
});

const Nothing = () => ({
  map: () => Nothing(),
  chain: () => Nothing(),
  fold: (f, g) => f("Nothing"),
  log: () => console.log("Nothing")
});

export const Maybe = (x) =>
  x === null || x === undefined ? Nothing() : Just(x);
```

---

## Examples

To start of simple, we can see that a Maybe of null returns a 'Nothing':

```js
const maybeResult = Maybe(null);

console.log(maybeResult.value()); // Nothing
```

The beauty of this is that we can `map` on a `Nothing`, which is to run a function on our `Maybe` and we won't receive an error because if the value is `Nothing`, then the `Maybe` skips the map execution.

```js
const getTokenPayload = (token) => token.split(".")[1];
const atob = (str) => new Buffer(str, "base64"); //handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];

const maybeResult = Maybe(null)
  .map(getTokenPayload)
  .map(atob)
  .map(parse)
  .map(retrieveProperty('exp'));

console.log(maybeResult); // Maybe(Nothing)
```

However, if our `Maybe` has a value, then we can map through it without any issue:

```js
const jwt =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyMzkwMjJ9.4Adcj3UFYzPUVaVF43FmMab6RlaQD8A9V8wFzzht-KQ";

const getTokenPayload = (token) => token.split(".")[1];
const atob = (str) => new Buffer(str, "base64"); //handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];

const maybeResult = Maybe(jwt)
  .map(getTokenPayload)
  .map(atob)
  .map(parse)
  .map(retrieveProperty("exp"));

console.log(maybeResult); // Maybe(1516239022)
```

Most are probably familiar with `map`, but what about `chain`, `fold`, and `log`?  Well the last one should be pretty easy to guess.... `log` will console.log out the value without leaving the context of the `Maybe`.  That leaves `chain` and `fold`.  

`fold` is another name of `reduce`, which can transform a value from one form to another, often from a more complex form to a less complex form.  A canonical example of reduce is summing an array of numbers into a single number.  See [reduce](JS/reduce.md) for more examples.  In this case, we are reducing or folding our object from a Maybe, to the intrinsic value, much in the same way a Promise returns a `then` or an error in the `catch` block.  One notable difference here is that our `fold` is written so that we must provide the error function first.  This ensures that we cannot be lazy and only handle the success path and forget to handle the error path.

```js
const maybeResult = Maybe(jwt)
  .map(getTokenPayload)
  .map(atob)
  .map(parse)
  .map(retrieveProperty("exp"))
  .map((_) => null);

maybeResult.fold(
  (err) => console.error(`uh oh we got Nothing`),
  (_) => console.log(`yay we got ${_}`)
); // uh oh we got Nothing 
```

`chain` helps us when we want to `map` a function that already returns a `Maybe` on it's own. When a `Maybe` maps a function that already wraps it's response in a `Maybe` then we would end up with a `Maybe(Maybe(value))` instead of a `Maybe(value)`.  `chain` solves this by not wrapping the response of the function because it is already a `Maybe`.  So for example if our `getTokenPayload` returns a `Maybe` because it performs a potentially unsafe operation by splitting the string, we could use `chain` instead of `map`.

```js
const getTokenPayload = (token) =>  Maybe(token.split(".")[1]);
const atob = (str) => new Buffer(str, "base64"); //handle atob for Node
const parse = (obj) => JSON.parse(obj);
const retrieveProperty = (propName) => (obj) => obj[propName];
516239022;

const maybeResult = Maybe(jwt)
  .chain(getTokenPayload)
  .map(atob)
  .map(parse)
  .map(retrieveProperty("exp"));

maybeResult.fold(
  (err) => console.error(`uh oh we got Nothing`),
  (_) => console.log(`yay we got ${_}`)
); // yay we got 1516239022 
```