# Reduce 

Reduce, sometimes called Fold, is one of those magical functions that can do almost anything.  The concept of reduce is the ability to reduce or fold a value into something smaller or simpler or map it to a completely new type.

The canonical example of reduce is how it can sum up an array of numbers:

```js
const sum = [1, 2, 3].reduce((acc, val) => acc + val, 0);
// 6
```

## Map Via Reduce

But reduce can also map over an array and transform each value of the array into something new and output a new array, just like the `map` function:

```js
const map = (f, arr) => 
        arr
        .reduce((acc, val) => acc.concat(f(val)), []);
map(_ => _ + 1, [1, 2, 3]);
// 2, 3, 4
//This could also be curried to make a reusable, partially applied function:
const map = (f,) => (arr) => 
        arr
        .reduce((acc, val) => acc.concat(f(val)), []);
const addOne - map(_ => _ + 1)
addOne([1, 2, 3]);
//2, 3, 4
```

## Filter Via Reduce

`Filter` can also be written using `reduce`:

```js
const filter = f => arr => 
arr.reduce((acc, val) => f(val) ? acc.concat(val) : acc, []);

const findEvenNumbers = filter(_ => _ % 2 === 0);
findEvenNumbers([1, 2, 3, 4]);
// [2, 4]
```

Reduce can be used to do so many things that it is impossible to enumerate all the possibilities here.  Think of all the way to iterate over values and build up a new object.