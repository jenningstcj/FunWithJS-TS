# SomeRequired

Makes some properties required.

```ts
type SomeRequired<T, K extends keyof T> = Omit<T, K> & Required<Pick<T, K>>;
```

---

## Examples