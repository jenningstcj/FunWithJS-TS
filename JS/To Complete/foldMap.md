# FoldMap

Foldmap takes a collection, a reducing function, and a mapping function and performs the map a single time after the reduction and thereby reduces the number of iterations compared to a traditional mapReduce.  

```js
const of = x => [x];

const foldMap = (collection, reduceFn, mapFn) =>
  of(collection.reduce(reduceFn,0)).map(mapFn)[0];
```

MapReduce does a mapping over the collection and then the reduction iterating over the collection twice.
```js
const mapReduce = (collection, mapFn, reduceFn) =>
  collection.map(mapFn).reduce(reduceFn, 0);
```

---

## Examples