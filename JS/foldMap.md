# FoldMap

Foldmap takes a collection, a reducing function, and a mapping function and performs the map a single time after the reduction and thereby reduces the number of iterations compared to a traditional mapReduce.  

```js
const foldMap = (collection, reduceFn, mapFn) =>
  collection.reduce(reduceFn(mapFn), 0);
```

MapReduce does a mapping over the collection and then the reduction iterating over the collection twice.
```js
const mapReduce = (collection, mapFn, reduceFn) =>
  collection.map(mapFn).reduce(reduceFn, 0);
```

---

## Examples

Let's walk through why `foldMap` can be preferable to `mapReduce` by examples.  For this exercise we will use the following shopping cart array:

```js
const cart = [
  {
    item: 1,
    name: "Chicken",
    price: 5.99,
    taxRate: 0.0885,
    quantity: 5,
  },
  {
    item: 2,
    name: "Steak",
    price: 8.99,
    taxRate: 0.0885,
    quantity: 2,
  },
  {
    item: 3,
    name: "Fish",
    price: 7.99,
    taxRate: 0.0885,
    quantity: 1,
  },
  {
    item: 4,
    name: "Turkey",
    price: 4.99,
    taxRate: 0.0885,
    quantity: 1,
  },
];
```

If we were to go the traditional route of using `map` and `reduce` separately, it might look something like this:

```js
const getSubtotal = (item) => item.price * item.quantity;
const getTax = (item) => item.price * item.quantity * item.taxRate;

//traditional map then reduce, iterates over collection twice
const total = cart
  .map((x) => getSubtotal(x) + getTax(x))
  .reduce((acc, val) => acc + val, 0);
console.log(total); // 66.300535
```

Here we are very clearly iterating over the cart twice by calling `map` and then `reduce`.  It works, but is a waste of execution.

We could even codify idea of `mapReduce`, but it's still double the iterations:

```js
//codify the map then reduce logic into a single function, but still iterates over the collection twice
const total = mapReduce(
  cart, 
  cartItem => (getSubtotal(cartItem) + getTax(cartItem)),
  (acc, val) => acc + val
  );
console.log(total); // 66.300535
```

What would it look like to correctly combine `map` and `reduce` into an efficient single iteration over the shopping cart?  All we need to do is move the `map` function to inside the `reduce`:

```js
//move mapping of values inside the reduce to only iterate once
const total = cart.reduce(
  (acc, val) => acc + (getSubtotal(val) + getTax(val)),
  0
);
console.log(total); // 66.300535
```

Or we could use the `foldMap` from the top of this page:

```js
const total = foldMap(
  cart,
  (mappingFn) => (acc, val) => acc + mappingFn(val),
  cartItem => (getSubtotal(cartItem) + getTax(cartItem)),
);
console.log(total); // 66.300535
```

