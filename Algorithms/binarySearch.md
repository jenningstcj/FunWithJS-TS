# Binary Search

The binary search algorithm is a searching algorithm which repeatedly cuts the array in half in an attempt to find the value most quickly.  This requires that the array be sorted so that the values are in order.  So if given an array of 1000 items, the binary search will first check if the value at the mid point of the array is the value searched for, if it isn't, then the algorithm knows which half to continue searching.  It does this over and over again until it finds the value searched for or the search is complete.  

As a simple example, if you have an array of numbers 1 to 1000 and are searching for 800, the binary search will:

- Iteration 1: Checks if the mid point in the array if the value at index 499 (value 500) is the value searched for.  If it was, the search would be over.  Then the search knows that the search can be isolated to between index 500 and 999, cutting the array in half.
- Iteration 2: Again the algorithm checks the mid point between indexes 500 and 999, which is index 749 (value 750).  Since value of index 749 is not the value 800, it keeps searching isolated to indexes 750 and 999.
- Iteration 3: Again the algorithm checks the mid point between 750 and 999, which is index 874 (value 875).
- Iteration 4: Because the algorithm has guessed a number too high (more than 800) it updates it's ending index to search to 873, so the starting search index is still 750 andd the ending search index is 873.  Again the algorithm checks the mid point between 750 and 873, which is index 811 (value 812).
- Iteration 5: Continuing on, the algorithm updates it's start and end indexes to 750 and 810 and checks the midpoint index of 780.
- Iteration 6: Continuing on, the algorithm updates it's start and end indexes to 781 and 810 and checks the midpoint index of 795.
- Iteration 7: Continuing on, the algorithm updates it's start and end indexes to 796 and 810 and checks the midpoint index of 803.
- Iteration 8: Continuing on, the algorithm updates it's start and end indexes to 796 and 802 and checks the midpoint index of 799 (value 800). BINGO! WE HAVE A WINNER!

So in 8 steps the binary search finds the value 800 in a sorted array of 1 to 1000.  That is pretty efficient, but it's important to note that the array ought to be sorted in order for this to work correctly.

Here is what a binary search looks like:

```ts
const binarySearch = (arr, val) => {
  let start = 0;
  let end = arr.length - 1;

  while (start <= end) {
    let mid = Math.floor((start + end) / 2);

    if (arr[mid] === val) {
      return mid;
    }

    if (val < arr[mid]) {
      end = mid - 1;
    } else {
      start = mid + 1;
    }
  }
  return -1;
}
```

This could also be written recursively:

```ts
const recursiveBinarySearch = (arr, val, start = 0, end = arr.length - 1) => {
  const mid = Math.floor((start + end) / 2);

  if (val === arr[mid]) {
    return mid;
  }

  if (start >= end) {
    return -1;
  }

  return val < arr[mid]
    ? recursiveBinarySearch(arr, val, start, mid - 1)
    : recursiveBinarySearch(arr, val, mid + 1, end);
}
```


The Binary Search really shines when there is a very large list of data.  Let's compare the performance to other methods:

| Method | Time |
| --- | --- |
|1000 sorted integers| |
| Array.Find | 0.197ms |
| Array.Some | 0.194ms |
| Array.Includes | 0.014ms |
| Binary Search | 0.165ms |
| Recursive Binary Search | 0.159ms |
| --- | --- |
|1000 sorted strings| |
| Array.Find | 0.178ms |
| Array.Some | 0.194ms |
| Array.Includes | 0.057ms |
| Array.indexOf | 0.035ms |
| Binary Search | 0.091ms |
| Recursive Binary Search | 0.085ms |

Interesting note, that while many of these times are similar or have negligible difference, the Array.indexOf and Array.Includes is faster than all other methods when the data set is reasonable small such as 1000 items.


| Method | Time |
| --- | --- |
|10,000 sorted strings| |
| Array.Find | 0.733ms |
| Array.Some | 0.264ms |
| Array.Includes | 0.082ms |
| Array.indexOf | 0.037ms |
| Binary Search | 0.109ms |
| Recursive Binary Search | 0.092ms |

Even at 10,000 random strings, Array.indexOf is fastest and Array.Includes is faster than a binary search, but not by much.

| Method | Time |
| --- | --- |
|500,000 sorted strings| |
| Array.Find | 0.707ms |
| Array.Some | 1.002ms |
| Array.Includes | 0.279ms |
| Array.indexOf | 0.097ms |
| Binary Search | 0.34ms |
| Recursive Binary Search | 0.263ms|


Even at 500,000 items Array.indexOf is the fastest and Array.Includes is on par with the binary search, but both the binary search and Array.Includes is substantially faster than Array.Find and Array.Some.  

In short, Array.indexOf might be your fastest way to find an item in an array, with Array.includes being second.  Binary Search can still be worth it especially when searching a large list of complex object by some nested value. But binary search also adds in complexity that not all engineers on your team may understand.  For simple arrays of values, it just might be simplest and fastest to use a built-in function such as Array.indexOf or Array.includes.  Simplicity can be genius.

For those interested in running their own experiments, here is the code I used to generate the array of strings:

```ts
export const range = (start, end) => [...Array(end + 1).keys()].slice(start);
function makeid(length) {
    let result = '';
    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    const charactersLength = characters.length;
    let counter = 0;
    while (counter < length) {
      result += characters.charAt(Math.floor(Math.random() * charactersLength));
      counter += 1;
    }
    return result;
}

const arr = range(1, 500000)
            .map(x => makeid(`${x}`.length * 10))
            .sort();

const valueToSearch = arr[799];

///////////////////////////////////////////////////
console.time("Find");

arr.find((x) => x === valueToSearch);

console.timeEnd("Find");
//////////////////////////////////////////////////

///////////////////////////////////////////////////
console.time("Includes");
//uses sameValueZero algorithm
arr.includes(valueToSearch);

console.timeEnd("Includes");
//////////////////////////////////////////////////

///////////////////////////////////////////////////
console.time("indexOf");

arr.indexOf(valueToSearch);

console.timeEnd("indexOf");
//////////////////////////////////////////////////

///////////////////////////////////////////////////
console.time("Some");

arr.some((x) => x === valueToSearch);

console.timeEnd("Some");
//////////////////////////////////////////////////

///////////////////////////////////////////////////
console.time("BinarySearch");

binarySearch(arr, valueToSearch);

console.timeEnd("BinarySearch");
//////////////////////////////////////////////////

///////////////////////////////////////////////////
console.time("RecursiveBinarySearch");

recursiveBinarySearch(arr, valueToSearch);

console.timeEnd("RecursiveBinarySearch");
//////////////////////////////////////////////////
```