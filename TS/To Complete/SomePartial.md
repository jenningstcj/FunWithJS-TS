# SomePartial

```ts
type SomePartial<T, K extendds keyof T> = Omit<T, K> & Partial<Pick<T, K>>;
```

---

## Examples