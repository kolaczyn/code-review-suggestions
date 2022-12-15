## Pointless useCallback

### Incorrect usage

`useCallback` in the code snippet below is pointless:

```tsx
const Counter = () => {
  const [count, setCount] = useState(0);

  // `useCallback` is pointless
  const handleIncrement = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);

  return <button onClick={handleIncrement}>{count}</button>;
};
```

By "pointless" I mean that it doesn't really do anything - it doesn't make the code run a little bit faster. On the contrary - it makes it a little bit slower, compared to not using `useCallback` at all.

**Why is it not faster?**

- `useCallback` is used to keep the same reference to a function, to minimize the number of re-renders of a React componet. But it doesn't apply to the native elements like `button`s
  **Why is it not slower?**
- `useCallback` reconstructs the function on each re-render. So you re-cache the function on each rerender. Caching is used to make things go faster. But if you're not using cache, it means that you're just allocating memory you're not gonna access.

### Correct usage

```tsx
const Counter = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = () => setCount((prev) => prev + 1);

  return <button onClick={handleIncrement}>{count}</button>;
};
```

### When to actually use `useCallback`?

#### Another incorrect usage

You might want to consider using `useCallback` if you're passing the function to a React component:

```tsx
const AppButton = ({ label, onClick }) => (
  <button onClick={onClick} className="btn">
    {label}
  </button>
);

const Counter = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);

  // the AppButton still re-render, even if you memoize the function with `useCallback`
  return <AppButton onClick={handleIncrement} label={count} />;
};
```

But contrary to popular belief, **that is not enought**.

#### Correct usage

You have to combine `memo` with `useEffect` to make it work:

```tsx
// use `memo` to memoize the component
const AppButton = memo(({ label, onClick }) => (
  <button onClick={onClick} className="btn">
    {label}
  </button>
));

const Counter = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);

  return <AppButton onClick={handleIncrement} label={count} />;
};
```

### Further reading

- https://beta.reactjs.org/apis/react/useCallback#examples-rerendering
- https://beta.reactjs.org/apis/react/memo#memo
