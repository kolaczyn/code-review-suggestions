## Variable names as documentation

**TODO** - couldn't think of a better example. I should improve it.

### TL;DR

Which of the following code snippets is easier to read?

```ts
// ❌
if (
  product.priceStatus === "promotion" &&
  product.stock > 0 &&
  !user.alergies.includes("leather")
) {
  alert(
    `End of week promotion: buy these leather shoes, they're only ${price}!`
  );
}

// ✅
const isPromotion = product.priceStatus === "promotion";
const isInStock = product.stock > 0;
const isUserAlergicToLeather = !user.alergies.includes("shoes");
if (isPromotion && isIStock && !isUserAlergicToLeather) {
  alert(`Buy these shoes, they're only ${price}!`);
}
```
