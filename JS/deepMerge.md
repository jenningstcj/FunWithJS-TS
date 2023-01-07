# Deep Merge

Deep copies objects before merging them to prevent mutation when the copy is mutated.  See [copy](/JS/copy.md) for details on `deepCopy`.

```js
export const deepMerge = (source, target) =>
  Object.assign(
    {},
    deepCopy(source),
    deepCopy(target)
  );
```

---

## Examples

```js
const source = {
  firstName: "Tyler",
  hobbies: ["camping", "hiking"],
};

const target = {
  firstName: "Tyler",
  hobbies: ["camping", "canoing"],
};

export const deepMerge = (source, target) =>
  Object.assign(
    {},
    deepCopy(source),
    deepCopy(target)
  );

const n = deepMerge(source, target);
n.hobbies.pop();
console.log(source); // { firstName: 'Tyler', hobbies: [ 'camping', 'hiking' ] }
//original object unmodified
console.log(target); // { firstName: 'Tyler', hobbies: [ 'camping', 'canoing' ] }
//newObject is not modified due to deep copying
console.log(n); //{ firstName: 'Tyler', hobbies: [ 'camping' ] }
// only new object is modified

```


Without the deepCopy, the target object is modified when we modify the copy.

```js
const source = {
  firstName: "Tyler",
  hobbies: ["camping", "hiking"],
};

const target = {
  firstName: "Tyler",
  hobbies: ["camping", "canoeing"],
};

const n = Object.assign({}, source, target);
n.hobbies.pop();
console.log(source); // { firstName: 'Tyler', hobbies: [ 'camping', 'hiking' ] }
//original object unmodified
console.log(target); // { firstName: 'Tyler', hobbies: [ 'camping' ] }
//newObject modified due to shared reference with 'n'
console.log(n); //{ firstName: 'Tyler', hobbies: [ 'camping' ] }

```