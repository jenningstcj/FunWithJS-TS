# NestedKeyOf

Gets a union type of all properties and sub-properties in a type.

```ts
const arr: [] = [];

type ArrayMethods = keyof typeof arr;

type Subtract<T extends number, U extends number> = [...Array<T>]['length'] extends U ? number : never;

type NestedKeyOf<T, Depth extends number = 5> = { //default to 5 levels deep
    [P in keyof T & (string | number)]: Depth extends 0 ? never :  T[P] extends object
    ? `${P}` | `${P}.${NestedKeyOf<T[P], Subtract<Depth, 1>>}`
    : `${P}`;
}[Exclude<keyof T & (string | number), ArrayMethods>];
```

---

## Examples