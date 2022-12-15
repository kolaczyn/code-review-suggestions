## When to add type annotations

### Type Inferance

TypeScript is quite smart when it comes to type inference. For example:

```ts
// the `number` annotation is redundant; it doesn't really do anything
let one: number = 1;
// you might as well omit it and save yourself a few keystrokes:
let one = 1;
```

### When to add types to variables

But TypeScript has it limits. Here's when you have to be more specific:

```ts
let primeNumbers = [];
primeNumbers.push(2);
primeNumbers.push(3);
primeNumbers.push(5);
// oops, your probably didn't mean to do that
primeNumbers.push("seven");
```

TypeScript infers `primeNumbers` to be of type `any[]`. So even though the author of the code probably meant it to be of type `number[]`, TS still allows a `string` to be pushed to the array.  
The reason TS wasn't able to infer the type, because TS had to little information about the variable - if the array is empty, how am I supposed to know what's in it?
That wouldn't have been an issue, if you init an array with some valuese:

```ts
let primeNumbers = [2];
primeNumbers.push(3);
primeNumbers.push(5);
primeNumbers.push(7);
// this won't compile
primeNumbers.push("eleven");
```

If you have to start with emptyarray, why you have to explicitly add the type:

```ts
let primeNumbers: number[] = [];
primeNumbers.push(2);
primeNumbers.push(3);
primeNumbers.push(5);
// this won't compile
primeNumbers.push("seven");
```
