# NestedKeyOf

Gets a union type of all properties and sub-properties in a type.

```ts
const arr: [] = [];

type ArrayMethods = keyof typeof arr;

type NestedKeyOf<T> = {
    [P in keyof T & (string | number)]: T[P] extends object
    ? `${P}` | `${P}.${NestedKeyOf<T[P]>}`
    : `${P}`;
}[Exclude<keyof T & (string | number), ArrayMethods>];
```