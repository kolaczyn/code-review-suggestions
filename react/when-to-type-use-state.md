## When to type `useState`

You might want to read about [When to add type annotations](/typescript/when-to-add-type-annotations.md) before reading this.

Here's when and when to add types

```tsx
const Example = () => {
  // ⚠️ `number` is redundant. Not a mistake, but better avoid it
  const [count, setCount] = useState<number>(0);

  // ✅ TS infers it as `number`
  const [count, setCount] = useState(0);

  // ❌ TS infers it as `any[]` so you don't have the full type safety
  const [selectedTodoIds, setSelectedTodoIds] = useState([]);

  // ✅ it's correctly typed
  const [selectedTodoIds, setSelectedTodoIds] = useState<number[]>([]);
};
```
