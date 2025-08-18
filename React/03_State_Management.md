# React State Management `React 18`

## Essentials

```javascript
// Basic useState syntax
const [count, setCount] = useState(0);
const [user, setUser] = useState({ name: '', email: '' });
const [items, setItems] = useState([]);
```

**Key Concepts**: useState Hook | State Updates | Batching | Derived State
**When to use**: Managing component-level data that changes over time and triggers re-renders

## Syntax Reference

### Basic Structure

```javascript
import { useState } from 'react';

function MyComponent() {
  // Primitive state
  const [count, setCount] = useState(0);
  
  // Object state
  const [user, setUser] = useState({ name: '', age: 0 });
  
  // Array state
  const [todos, setTodos] = useState([]);
  
  // Function as initial state (lazy initialization)
  const [expensiveValue, setExpensiveValue] = useState(() => {
    return calculateExpensiveValue();
  });
}
```

### Common Patterns

```javascript
// Pattern 1: Updating primitive state
setCount(count + 1);
setCount(prevCount => prevCount + 1); // Preferred for updates based on previous state

// Pattern 2: Updating object state (immutable)
setUser({ ...user, name: 'John' });
setUser(prevUser => ({ ...prevUser, age: prevUser.age + 1 }));

// Pattern 3: Updating array state
setTodos([...todos, newTodo]); // Add item
setTodos(todos.filter(todo => todo.id !== id)); // Remove item
setTodos(todos.map(todo => todo.id === id ? { ...todo, completed: true } : todo)); // Update item

// Pattern 4: Multiple state updates (batched in React 18)
const handleClick = () => {
  setCount(count + 1);
  setUser({ ...user, lastAction: 'clicked' });
  // Both updates batched into single re-render
};

// Pattern 5: Conditional state updates
const handleUpdate = (newValue) => {
  if (newValue !== currentValue) {
    setValue(newValue);
  }
};
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use functional updates when new state depends on previous state | Mutate state directly: `user.name = 'John'` |
| Initialize expensive state with function: `useState(() => heavy())` | Pass expensive calculations directly: `useState(heavy())` |
| Use multiple useState calls for unrelated state | Put everything in one massive state object |
| Keep state as simple as possible | Store derived values in state when they can be calculated |
| Use objects/arrays immutably with spread operator | Rely on object reference equality for complex state |

## Quick Fixes

- **State not updating immediately** → State updates are asynchronous; use useEffect to react to state changes
- **Infinite re-renders** → Move derived calculations outside render or use useMemo
- **State reset on parent re-render** → Move useState inside component, not conditionally declared
- **Previous state in closure** → Use functional update: `setState(prev => prev + 1)` instead of `setState(state + 1)`
- **Lost input focus** → Ensure consistent key props and avoid creating components inside render

## Gotchas

⚠️ **State updates are batched**: Multiple setState calls in event handlers are batched into single re-render in React 18

⚠️ **Stale closures**: Event handlers and effects capture state values from when they were created - use functional updates or dependencies

⚠️ **Object/array mutations**: `user.name = 'John'` won't trigger re-render; always create new objects/arrays

⚠️ **Derived state anti-pattern**: Don't store calculated values in state; compute them during render or use useMemo

⚠️ **Conditional useState**: Never call hooks conditionally or inside loops - always at top level of component

## Checklist

- [ ] State is initialized with appropriate default values
- [ ] Object and array updates use immutable patterns (spread operator)
- [ ] Functional updates used when new state depends on previous state
- [ ] No derived state stored unnecessarily (calculate during render instead)
- [ ] useState calls are at component top level, not conditional
- [ ] Event handlers don't rely on stale state values in closures