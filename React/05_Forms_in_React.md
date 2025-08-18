# React Forms `v 18.x`

## Essentials

```jsx
// Controlled component with useState
function MyForm() {
  const [email, setEmail] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Email:', email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Key Concepts**: Controlled Components | Event Handling | Form Validation | State Management
**When to use**: Building interactive forms with real-time validation and React state management

## Syntax Reference

### Basic Structure

```jsx
// Controlled input - React manages the value
<input 
  value={state}           // React controls the value
  onChange={handleChange} // Update state on every keystroke
/>

// Uncontrolled input - DOM manages the value
<input 
  ref={inputRef}          // Access DOM directly
  defaultValue="initial"  // Set initial value only
/>
```

### Common Patterns

```jsx
// Pattern 1: Multiple inputs with single handler
function MultiInputForm() {
  const [form, setForm] = useState({ name: '', email: '' });
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setForm(prev => ({ ...prev, [name]: value }));
  };

  return (
    <form>
      <input name="name" value={form.name} onChange={handleChange} />
      <input name="email" value={form.email} onChange={handleChange} />
    </form>
  );
}

// Pattern 2: Checkbox handling
function CheckboxForm() {
  const [isChecked, setIsChecked] = useState(false);
  
  return (
    <input 
      type="checkbox"
      checked={isChecked}
      onChange={(e) => setIsChecked(e.target.checked)}
    />
  );
}

// Pattern 3: Radio buttons
function RadioForm() {
  const [selected, setSelected] = useState('option1');
  
  return (
    <div>
      <input 
        type="radio" 
        name="options" 
        value="option1"
        checked={selected === 'option1'}
        onChange={(e) => setSelected(e.target.value)}
      />
      <input 
        type="radio" 
        name="options" 
        value="option2"
        checked={selected === 'option2'}
        onChange={(e) => setSelected(e.target.value)}
      />
    </div>
  );
}

// Pattern 4: Form validation
function ValidatedForm() {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');

  const validateEmail = (email) => {
    return /\S+@\S+\.\S+/.test(email);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!validateEmail(email)) {
      setError('Please enter a valid email');
      return;
    }
    setError('');
    // Submit form
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      {error && <span className="error">{error}</span>}
      <button type="submit">Submit</button>
    </form>
  );
}

// Pattern 5: Uncontrolled with useRef
function UncontrolledForm() {
  const nameRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Name:', nameRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} defaultValue="John" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use controlled components for validation | Mix controlled and uncontrolled in same form |
| Call `e.preventDefault()` in submit handler | Forget to prevent default form submission |
| Use `name` attributes for multiple inputs | Manually track every input with separate state |
| Validate on submit and/or onChange | Only validate after user submits |
| Use `defaultValue` for uncontrolled inputs | Use `value` prop with uncontrolled inputs |

## Quick Fixes

- **"Warning: changing uncontrolled input to controlled"** → Initialize state with empty string `useState('')` instead of `useState()`
- **Form submits and page reloads** → Add `e.preventDefault()` in submit handler
- **Checkbox not updating** → Use `e.target.checked` instead of `e.target.value`
- **Multiple inputs not updating** → Use computed property names `[name]: value` in setState
- **Can't access uncontrolled input value** → Use `ref.current.value` not `ref.value`

## Gotchas

⚠️ **Controlled vs Uncontrolled**: Once you provide a `value` prop, input becomes controlled. Use `defaultValue` for uncontrolled inputs

⚠️ **State Updates are Async**: Don't rely on state immediately after setState - use useEffect for side effects

⚠️ **Event Object Timing**: Don't access `e.target` in async functions - extract values first or call `e.persist()`

⚠️ **Radio Button Groups**: All radio buttons in a group need the same `name` attribute

## Checklist

- [ ] Added `e.preventDefault()` to prevent page reload
- [ ] Used `value` + `onChange` for controlled inputs
- [ ] Used `checked` + `onChange` for checkboxes/radios  
- [ ] Implemented form validation before submission
- [ ] Used `name` attributes for multiple input handling
- [ ] Chose controlled vs uncontrolled strategy consistently
- [ ] Added proper error handling and user feedback