# Copy

Items can be copied in several different ways.  There are shallow copies, which will copy root level items and copy deeper items by reference.  This means that on a shallow copy if a deeper property is mutated, then the original copy will also be modified.  There are also deep copies which create a true full clone of the item including all deeper properties beyond the root level.

```js
//shallow copy arrays
export const copyArrayBySpread = (x) => [...x];
export const copyArrayByObjectAssign = (x) => Object.assign([], x);

//shallow copy objects
export const copyBySpread = (x) => ({ ...x });
export const copyByObjectAssign = (x) => Object.assign({}, x);

//deep copy
export const deepCopy = (x) => JSON.parse(JSON.stringify(x));

```

---

## Examples

Shallow Copy:

```js
const originalObject = {
  firstName: "Tyler",
  hobbies: ["camping", "hiking"],
};

const shallowCopy = copyBySpread(originalObject);
shallowCopy.hobbies.pop();
console.log(shallowCopy); // { firstName: 'Tyler', hobbies: [ 'camping' ] }
console.log(originalObject); // { firstName: 'Tyler', hobbies: [ 'camping' ] }
// both are modified by popping a hobby off of the shallow copy.
```

Deep Copy:

```js
const originalObject = {
  firstName: "Tyler",
  hobbies: ["camping", "hiking"],
};

const deepCopyObject = deepCopy(originalObject);
deepCopyObject.hobbies.pop();
console.log(deepCopyObject); // { firstName: 'Tyler', hobbies: [ 'camping' ] }
console.log(originalObject); // { firstName: 'Tyler', hobbies: [ 'camping', 'hiking' ] }
// only the new deep copied object is modified
```