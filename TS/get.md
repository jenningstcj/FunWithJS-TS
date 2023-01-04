# Get Lens

```ts
const arr: [] = [];

type ArrayMethods = keyof typeof arr;

type NestedKeyOf<T> = {
    [P in keyof T & (string | number)]: T[P] extends object
    ? `${P}` | `${P}.${NestedKeyOf<T[P]>}`
    : `${P}`;
}[Exclude<keyof T & (string | number), ArrayMethods>];

const get = <T>(value: T, path: NestedKeyOf<T>) => ...
```

---

## Examples

TODO finish
