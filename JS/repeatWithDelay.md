# RepeatWithDelay

Repeats a function for 'x' times with a delay in between.  Uses the [pause](pause.md) and [range](range.md) functions.

```js
const repeatWithDelay = (times, delay, fn) => range(1, times).map(i => pause(i * delay).then(_ => fn());
```

---

## Examples

```js
repeatWithDelay(3, 5000, () => console.log(new Date()));
```