# Sort

Root level type-safe sort:

```ts
const sortAscending = <T>(prop: keyof T) => (a:T, b:T): number => {
    const valueA = a[prop];
    const valueB = b[prop];
    const objA = typeof valueA === 'string'
    ? valueA.toUpperCase() : valueA;
    const objB = typeof valueB === 'string'
    ? valueB.toUpperCase() : valueB;

    if(objA < objB) return -1;
    if(objA > objB) return 1;
    return 0;
};

const sortDescending = <T>(prop: keyof T) => (a:T, b:T): number => {
    const valueA = a[prop];
    const valueB = b[prop];
    const objA = typeof valueA === 'string'
    ? valueA.toUpperCase() : valueA;
    const objB = typeof valueB === 'string'
    ? valueB.toUpperCase() : valueB;

    if(objA < objB) return 1;
    if(objA > objB) return -1;
    return 0;
};

const sort = <T>(arr: T[], prop: keyof T, order: "asc" | "desc"): T[] => order === "asc"
    ? arr.sort(sortAscending(prop))
    : arr.sort(sortDescending(prop));
```


---

## Examples

TODO add deep sort using lens
