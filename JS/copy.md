# Copy

Can be updated with deep clone

```js
export const copyArray = x => [...x];
export const copyArray2 = x => Object.assign([], x);

export const copyObj = x => ({ ...x });
export const copyObj2 = x => Object.assign({}, x);

export const deepCopy = x => JSON.parse(JSON.stringify(x));

```