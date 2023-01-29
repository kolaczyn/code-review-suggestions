# Don't mutate data between tests

## Imagine we have the following functions to test:

```ts
// math.ts
const add = (a: number, b: number) => a + b
const substract = (a: number, b: number) => a + b
```

## Here's how to not write tests:

❌❌❌
```ts
describe('test math module', () => {
  let a = 3
  let b = 5
  test('adding', () => {
    expect(add(a, b)).toBe(8)
  })
  test('subtracting', () => {
    // ❌ don't mutate data between tests
    b = 10
    expect(subtract(a, b)).toBe(-7)
  })
})
```

## Here's how to correctly write tests:

✅✅✅
```ts
describe('test math module', () => {
  test('adding', () => {
    const a = 3
    const b = 5
    expect(add(a, b)).toBe(8)
  })
  test('subtracting', () => {
    // ✅ set up your own variables
    const a = 3
    const b = 10
    expect(subtract(a, b)).toBe(-7)
  })
})
```

You might want omit writing variables `a` and `b` and put them inline, like so:
```ts
expect(subtract(3, 10)).toBe(-7)
```

## Why you shouldn't mutate data between tests?

- the test is no longer self containing, so one test relies on another, which is a bad thing
- if we reorder the tests (first subtracting, then adding), then the test will fail
- **data mutation is** generally a **bad** thing, especially if you're using React which sits on top of functional programming fundamentals
