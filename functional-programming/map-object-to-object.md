## Reduce object to object

### Premise

Let's image we're working with this kind of data:

```ts
const products = {
  2137: {
    price: 99.99,
    name: "Shampoo",
  },
  100: {
    price: 11.99,
    name: "Battery",
  },
  999: {
    price: 4.99,
    name: "Poster",
  },
};
```

And we want to "filter-out" products that cost more than $50, like so:

```ts
const expectedResult = {
  // 2137 is gone :c
  100: {
    price: 11.99,
    name: "Battery",
  },
  999: {
    price: 4.99,
    name: "Poster",
  },
};
```

### Solutions

```ts
// ❌
// This code is quite declerative, but we have to declare the array with `let` instead of `const`
let cheapProducts = {};
Object.keys(products)
  .filter((id) => products[id].price <= 50)
  .forEach((id) => {
    cheapProducts[id] = products[id];
  });
```
