## Derived state

### TL;DR

The component's state should be as minimal as possible. Compute state if needed.
**Why?**  
If you have a lot of redundant data, you have to keep it all in sync. If you don't have any redundant data, you don't have to worry about that.

### Explanation

Let's look at this simple Todo application. We can add todos to a list and remove them from the list.  
Here's Todo type:

```ts
type Todo = {
  id: number;
  label: string;
};
```

And here's the component:

```tsx
const TodosList = () => {
  const [todos, setTodos] = useState<Todo[]>([]);

  const handleRemoveTodo = (id: number) =>
    setTodos((prev) => prev.filter((x) => x.id !== id));

  const handleAddTodo = (todo: Todo) => setTodos((prev) => [...prev, todo]);

  return (
    <>
      <TodoForm onAddTodo={handleAddTodo} />
      {todos.map((todo) => (
        <div key={todo.id}>
          <span>{todo.label}</span>
          <button onClick={() => handleAddRemoveTodo(todo.id)}>Remove</button>
        </div>
      ))}
    </>
  );
};
```

Let's add a new feature - we want to allow user to mark one todo as favourite. Here's how you could implement it.

**This is incorrect**

```tsx
const TodosList = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  // ❌ some of the date is duplicated
  const [favouriteTodo, setFavouriteTodo] = useState<Todo>(null);

  const handleRemoveTodo = (id: number) =>
    setTodos((prev) => prev.filter((x) => x.id !== id));

  const handleAddTodo = (todo: Todo) => setTodos((prev) => [...prev, todo]);

  const handleSetFavourite = (todo: Todo) => setFavouriteTodo(todo);

  return (
    <>
      <TodoForm onAddTodo={handleAddTodo} />
      {/* ❌ what if the data gets stale? */}
      {favouriteTodo && <span>Favourite Todo: {favouriteTodo.label}</span>}
      {todos.map((todo) => (
        <div key={todo.id}>
          <span>{todo.label}</span>
          <button onClick={() => handleAddRemoveTodo(todo.id)}>Remove</button>
          <button onClick={() => handleSetFavourite(todo)}>
            Make Favourite
          </button>
        </div>
      ))}
    </>
  );
};
```

There's a problem with this approach. What if you want to be able to edit todos? If you edit a todo which is the user's favourite, then the `favouriteTodo` state will be stale.  
If you want to react to changes, you will have to sync the states together with `useEffect`. So you _can_ make it work, but that's a lot of extra code which is hard to read, reason about and maintain.

### Correct usage

Just keep state to the minimum:

```tsx
const TodosList = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  // ✅ keep track of just the id
  const [favouriteTodoId, setFavouriteTodoId] = useState<number | null>(null);

  const handleRemoveTodo = (id: number) =>
    setTodos((prev) => prev.filter((x) => x.id !== id));

  const handleAddTodo = (todo: Todo) => setTodos((prev) => [...prev, todo]);

  const handleSetFavourite = (id: number) => setFavouriteTodoId(id);

  // ✅ compute the state
  const favouriteTodo = todos.find((todo) => todo.id === favouriteTodoId);

  return (
    <>
      <TodoForm onAddTodo={handleAddTodo} />
      {favouriteTodo != null && (
        <span>Favourite Todo: {favouriteTodo.label}</span>
      )}
      {todos.map((todo) => (
        <div key={todo.id}>
          <span>{todo.label}</span>
          <button onClick={() => handleAddRemoveTodo(todo.id)}>Remove</button>
          <button onClick={() => handleSetFavourite(todo.id)}>
            Make Favourite
          </button>
        </div>
      ))}
    </>
  );
};
```
