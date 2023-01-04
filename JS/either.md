# Try/Catch

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