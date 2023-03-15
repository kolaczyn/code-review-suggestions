# When too many args, use object

## TL;DR

```ts
❌
const calculatePrice = (price: number, promotion: number, userCoupon: number): number => {
  // logic here
}
```

```ts
✅
type Args = {
  price: number,
  promotion: number,
  userCoupon: number,
}

const calculatePrice = ({price, promotion, userCoupon}: Args): number => {
  // logic here
}
```

## Why

In the first example, it is very easy to make a mistake. And type system won't be able to catch that, because all of the arguments are of the same time.  
So you don't have to use it in the folowing situation:

```ts
✅
// all of the args have different types, so It's harder to make a mistake
const productLabel = (label: string, price: number, isAvailable: boolean) => {
  // logic here
}
```
