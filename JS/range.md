# Range

Creates an array of a range of numbers.

```js
export const range = (start, end) => [...Array(end + 1).keys()].slice(start);
```

---

## Examples

```js
export const range = (start, end) => [...Array(end + 1).keys()].slice(start);

console.log(range(1,5)); // [ 1, 2, 3, 4, 5 ]
```

<style>
    table {
        width: 100%;
    }
</style>

| Previous     |      Next |
|--------------|-----------:|
| [prepend](JS/../prepend.md)  | [copy](JS/../copy.md)      |