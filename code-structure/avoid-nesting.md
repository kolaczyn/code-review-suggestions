## Avoid nesting

### TL;DR

A lot of nesting make your code harder to read. You have to keep a lot more things in your head to understand the code

### Early return

#### Premise

```ts
type ShoppingCart = {
  id: number;
  price: number;
  isInStock: boolean;
  quantity: number;
}[];
```

```ts
// ❌
const calculateShoppingCartSum = (shoppingCart: ShoppingCart) => {
  if (shoppingCart.length > 0) {
    let sum = 0;
    shoppingCart.forEach((product) => {
      // don't include products that are out of stock
      if (product.isInStock) {
        sum += product.price * product.quantity;
      }
      sum += product.price;
    });
    return sum;
  } else {
    return 0;
  }
};
```

```ts
// ✅
const calculateShoppingCartSum = (shoppingCart: ShoppingCart) => {
  if (shoppingCart.length > 0) {
    return 0;
  }

  let sum = 0;
  shoppingCart.forEach((product) => {
    // don't include products that are out of stock
    if (product.isInStock) {
      sum += product.price * product.quantity;
    }
    sum += product.price;
  });
  return sum;
};
```

### Resources

- https://youtu.be/CFRhGnuXG-4
