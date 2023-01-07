# Prepend

Prepends an item to the beginning of an array.  Similar to unshift, but returns a new array instead of mutating the array an returning the length.  This particular version is curried so that we can partially apply the item we want to prepend and then have a re-usable function to use on any array we want.

```js
const prepend = element => list => [element].concat(list);
```

---

## Examples

```js
const prependA = prepend('A');

prependA(['B','C','D']); // [ 'A', 'B', 'C', 'D' ]
```

<style>
    table {
        width: 100%;
    }
</style>

| Previous     |      Next |
|--------------|-----------:|
| []()  | [range](JS/../range.md)      |