# Event Handling `React 18`

## Essentials

```jsx
// Basic event handlers
function Button() {
  const handleClick = () => {
    console.log('Button clicked!');
  };

  return <button onClick={handleClick}>Click me</button>;
}

// Inline handlers
<input onChange={(e) => setName(e.target.value)} />

// Form submission
<form onSubmit={(e) => { e.preventDefault(); handleSubmit(); }}>
```

**Key Concepts**: SyntheticEvent | Event Propagation | preventDefault | Handler Functions
**When to use**: User interactions, form inputs, button clicks, and dynamic UI updates

## Syntax Reference

### Basic Structure

```jsx
// Function component with event handler
function MyComponent() {
  const handleEvent = (event) => {
    // Access event properties
    console.log(event.target.value);
    console.log(event.type);
  };

  return <element onEventName={handleEvent} />;
}
```

### Common Patterns

```jsx
// Pattern 1: Click handlers with state updates
const [count, setCount] = useState(0);
<button onClick={() => setCount(count + 1)}>
  Count: {count}
</button>

// Pattern 2: Form input handling
const [email, setEmail] = useState('');
<input 
  type="email"
  value={email}
  onChange={(e) => setEmail(e.target.value)}
/>

// Pattern 3: Form submission with validation
const handleSubmit = (e) => {
  e.preventDefault();
  const formData = new FormData(e.target);
  // Process form data
};
<form onSubmit={handleSubmit}>

// Pattern 4: Passing arguments to handlers
const handleItemClick = (id) => {
  console.log('Clicked item:', id);
};
{items.map(item => 
  <div key={item.id} onClick={() => handleItemClick(item.id)}>
    {item.name}
  </div>
)}

// Pattern 5: Event object access
const handleKeyPress = (e) => {
  if (e.key === 'Enter') {
    handleSubmit();
  }
};
<input onKeyPress={handleKeyPress} />
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use arrow functions for inline handlers | Call functions directly: `onClick={handleClick()}` |
| Call `preventDefault()` for form submissions | Forget to prevent default on forms |
| Use `e.target.value` for input values | Mutate state directly in handlers |
| Define handlers outside JSX for complex logic | Create new functions on every render |
| Use `useCallback` for expensive handlers | Pass strings to event handlers |

## Quick Fixes

- **Handler not firing** → Check spelling: `onClick` not `onclick`
- **Function called immediately** → Remove parentheses: `onClick={handler}` not `onClick={handler()}`
- **Form submits and refreshes page** → Add `e.preventDefault()` in onSubmit handler
- **Can't access input value** → Use `e.target.value` not `e.value`
- **Event undefined in handler** → Ensure parameter is named (e.g., `(e) =>` not `() =>`)

## Gotchas

⚠️ **SyntheticEvent**: React wraps native events - access `.nativeEvent` for native properties
⚠️ **Event Pooling**: In React <17, events are pooled and nullified after handler execution
⚠️ **Arrow Functions**: Create new functions on each render - use `useCallback` for performance
⚠️ **Form Submission**: Always preventDefault() or form will reload the page
⚠️ **Event Binding**: `this` binding issues in class components - use arrow functions or bind in constructor

## Checklist

- [ ] Event handler names start with "handle" or "on" (e.g., `handleClick`, `onButtonClick`)
- [ ] Forms use `onSubmit` with `preventDefault()` to avoid page refresh
- [ ] Input handlers update state using `e.target.value`
- [ ] Complex handlers defined outside JSX return statement
- [ ] Event handlers don't call functions immediately (no parentheses in JSX)
- [ ] Conditional logic uses proper event properties (`e.key`, `e.type`, etc.)
- [ ] Performance-critical handlers wrapped in `useCallback` when dependencies change