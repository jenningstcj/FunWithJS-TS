# Reduce 

```js
const sum = [1, 2, 3].reduce((acc, val) => acc + val, 0);
```


## Map Via Reduce

```js
const map = (f, arr) => 
        arr
        .reduce((acc, val) => acc.concat(f(val)), []);
```

## Filter Via Reduce

```js
const filter = f => arr => 
arr.reduce((acc, val) => f(val) ? acc.concat(val) : acc, []);
```