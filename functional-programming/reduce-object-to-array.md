## Reduce object to array

Let's image we're working with this kind of data:

### Premise

```ts
const idToProductData = {
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

And we want to just get the names of all the products:

```ts
const expectedResult = ["Shampoo", "Battery", "Poster"];
```

### Solutions

```ts
// ✅
const allNames = Object.values(idToProductData).map((product) => product.name);
```

```ts
// ❌
// Wrong, because we're declaring array as mutable, the code is not imperative and we rely on side-effects
let allNames = [];
Object.values(idToProductData).forEach((product) => {
  allNames.push(product.name);
});
```

```ts
// ❌❌❌
// Wrong, for the same reasons as the code above.
// Also, this looks ugly af
let allNames = [];
for (const id in idToProductData) {
  allNames.push(idToProductData[id].name);
}
```

### Resources

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
