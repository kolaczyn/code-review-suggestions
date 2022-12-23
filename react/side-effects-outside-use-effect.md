## Side Effects outside `useEffect`

### Premise

Let's imagine that we want to create a simple counter, which saves current count to local storage.  
We have these functions available:

```ts
const getFromLocalStorage = () => Number(window.localStorage.getItem("count"));

const saveToLocalStorage = (count: number) => {
  window.localStorage.setItem("count", count);
};
```

### Incorrect usage

```tsx
// ❌
const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount((prev) => prev + 1);
    saveToLocalStorage(count);
  };

  // ❌ This is a red flag.
  // If you use a function without using its output, you're very likely doing something wrong
  setCount(getFromLocalStorage());

  return <button onClick={increment}>{count}</button>;
};
```

### Correct usage

```tsx
// ✅
const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount((prev) => prev + 1);
    saveToLocalStorage(count);
  };

  // ✅ run side effects in useEffect. That's where  useEffect's name comes from
  useEffect(() => {
    setCount(getFromLocalStorage());
  }, []);

  return <button onClick={increment}>{count}</button>;
};
```
