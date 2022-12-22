## Prefer arrow function expression

### TL;DR

```ts
type Product = {
  materials: string[];
  price: number;
};
```

```ts
const leatherProducts = products.filter((product) =>
  product.materials.includes("leather")
);
// ✅
```

```ts
// ❌
const leatherProducts = products.filter((product) => {
  return product.materials.includes("leather");
});
```

```ts
// ❌❌❌
const leatherProducts = products.filter((product) => {
  if (product.materials.includes("leather")) {
    return true;
  } else {
    return false;
  }
});
```
