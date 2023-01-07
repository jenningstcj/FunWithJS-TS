# Either

Either, sometimes called Result, is another type of context (see [of](JS/of.md) and [maybe](JS/maybe.md) for other examples) that can help us with error handling without having to check for null or errors at every turn.  Also see [Railway Oriented Programming](https://fsharpforfunandprofit.com/rop/) for more information on this concept.

```js
export const Ok = x => ({
  map: f => Ok(f(x)),
  chain: f => f(x),
  fold: (f, g) => g(x),
  log: () => console.log(`Ok(${JSON.stringify(x)})`)
});

export const Error = x => ({
  map: f => Error(x),
  chain: f => Error(x),
  fold: (f, g) => f(x),
  log: () => console.error(`Error(${JSON.stringify(x)})`)
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