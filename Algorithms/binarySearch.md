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


The Binary Search really shines when there is a very large list of data.  I've run the following searches on 100 randomized lists of strings and averaged the performance:

| Method | Time |
| --- | --- |
| 100 item lists | --- |
| Array.Find | 0.015ms |
| Array.Some | 0.015ms |
| Array.Includes | 0.000988ms |
| Array.IndexOf | 0.000978 |
| Binary Search | 0.00619 |
| --- | --- |
| 1,000 item lists | --- |
| Array.Find | 0.0335ms |
| Array.Some | 0.0298ms |
| Array.Includes | 0.023ms |
| Array.IndexOf | 0.0226ms |
| Binary Search | 0.0040ms |
| --- | --- |
| 100,000 item lists | --- |
| Array.Find | 0.144ms |
| Array.Some | 0.123ms |
| Array.Includes | 0.055ms |
| Array.IndexOf | 0.049ms |
| Binary Search | 0.013ms |

The Binary Search is indeed faster than the other methods so if you are looking to eek out every bit of performance you can and your list is over 100 items, this might be it. For smaller arrays, most of the time you can get by with one of the other methods and the user of your application will not know the difference.  For small things, all of these are faster than the human eye.  In small arrays, IndexOf or Includes can be fastest, even faster than binary search.  Even in larger arrays when comparing single executions, I've seen IndexOf and Includes be faster than the binary search, but the average over time clearly states that the binary search is faster on larger arrays.  If looking to keep things simple, out of the built-in functions to search arrays in JavaScript, IndexOf usually wins out.  
