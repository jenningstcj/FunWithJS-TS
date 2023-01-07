# Maybe

```js
const Just = x => ({
  map: f => Maybe(f(x)),
  chain: f => f(x),
  fold: (f, g) => g(x),
  log: () => console.log(x),
  match: f => x
});

const Nothing = () => ({
  map: () => Nothing(),
  chain: () => Nothing(),
  fold: (f, g) => f("Nothing"),
  log: () => console.log("Nothing"),
  match: f => f
});

export const Maybe = x => (x === null || x === undefined ? Nothing() : Just(x));
```

---

## Examples