# Pause

Pauses for a time period.

```js
const pause = (time) => new Promise(resolve => setTimeout(resolve, time));
```

---

## Examples

```js
pause(5000).then(_ => console.log(new Date()));
```