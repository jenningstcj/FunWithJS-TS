# Deep Merge

Probably can be updated with deepClone

```js
export const deepMerge = (source, target) => Object.assign({},JSON.parse(JSON.stringify(source)), JSON.parse(JSON.stringify(target)));
```

---

## Examples