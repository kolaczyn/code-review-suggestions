## Defining Component inside Component

### Don't do this:

Creating component inside componet is very ineffecient - it's an antipattern and should never be used.

```tsx
const ProductTile = ({ isInStock, name }) => {
  const cartThingy = isInStock ? <button>Buy now</button> : <span>Out of stock</span>;

  return <div>
    {cartThingy}
    <span>{name}</name>
  </div>
};
```

### Do this:

Just define it _outside_:

```tsx
const CartThingy = ({isInStock}) =>
  isInStock ? <button>Buy now</button> : <span>Out of stock</span>;

const ProductTile = ({ isInStock, name }) => {
  return <div>
    <CartThingy isInStock={isInStock} />
    <span>{name}</name>
  </div>
};
```

Another approach is to just define the logic inline (without defining a component):

```tsx
const ProductTile = ({ isInStock, name }) => {
  return <div>
  {isInStock ? <button>Buy now</button> : <span>Out of stock</span>}
    <span>{name}</name>
  </div>
};
```

This might not be applicable if the logic is more complicatd.
