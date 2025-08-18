# React Fundamentals `v 18.2.0`

## Essentials

```jsx
// Basic React component
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Using the component
<Welcome name="World" />
```

**Key Concepts**: Components | JSX | Virtual DOM | Props | State
**When to use**: Building interactive user interfaces with reusable components

## Syntax Reference

### Basic Component Structure

```jsx
// Function Component (recommended)
function MyComponent({ title, children }) {
  return (
    <div>
      <h1>{title}</h1>
      {children}
    </div>
  );
}

// Class Component (legacy)
class MyComponent extends React.Component {
  render() {
    return <h1>{this.props.title}</h1>;
  }
}
```

### JSX Patterns

```jsx
// JSX expressions - JavaScript in curly braces
const name = "React";
const element = <h1>Hello {name.toUpperCase()}!</h1>;

// Conditional rendering
const greeting = isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign up.</h1>;

// List rendering
const items = ['apple', 'banana', 'orange'];
const listItems = items.map(item => <li key={item}>{item}</li>);

// Event handling
function Button() {
  const handleClick = () => alert('Clicked!');
  return <button onClick={handleClick}>Click me</button>;
}
```

### App Setup

```jsx
// main.jsx or index.js (React 18)
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

const root = createRoot(document.getElementById('root'));
root.render(<App />);

// App.jsx
function App() {
  return (
    <div className="App">
      <h1>My React App</h1>
    </div>
  );
}

export default App;
```

## What is React

**React** is a JavaScript library for building user interfaces, especially web applications. Created by Facebook, it uses a component-based architecture where UI is broken into reusable pieces.

**Why use React:**
- **Reusable Components** - Write once, use everywhere
- **Virtual DOM** - Fast, efficient updates
- **Large Ecosystem** - Extensive libraries and tools
- **Strong Community** - Wide adoption, good job prospects

**React DOM vs React Native:**
- **React DOM** - Renders to web browsers (HTML elements)
- **React Native** - Renders to mobile platforms (iOS/Android components)
- Same React concepts, different rendering targets

## SPA Concept

**Single Page Application (SPA)** - Entire app runs in one HTML page, JavaScript handles navigation and content updates without full page reloads.

Benefits: Faster navigation, app-like experience, better user experience
Drawbacks: SEO challenges, initial load time, complexity

## Project Setup

### Create React App (CRA)
```bash
npx create-react-app my-app
cd my-app
npm start
```

### Vite (Faster alternative)
```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

### Basic Folder Structure
```
my-app/
├── public/
│   └── index.html          # Single HTML file
├── src/
│   ├── components/         # Reusable components
│   ├── App.jsx            # Main app component
│   ├── main.jsx           # Entry point
│   └── index.css          # Global styles
├── package.json           # Dependencies
└── vite.config.js         # Build configuration
```

## Virtual DOM vs Real DOM

| Virtual DOM | Real DOM |
|-------------|----------|
| JavaScript representation of UI | Actual HTML elements in browser |
| Fast to create/modify | Slow to manipulate directly |
| React's internal optimization | Browser's native structure |
| Batched updates | Individual updates cause reflow |

**How it works:**
1. State changes trigger Virtual DOM update
2. React compares (diffs) old vs new Virtual DOM
3. Only necessary changes applied to Real DOM
4. Result: Better performance with complex UIs

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use PascalCase for components | Use lowercase component names |
| Always provide `key` prop in lists | Modify props directly (they're immutable) |
| Keep components small and focused | Put everything in one huge component |
| Use function components by default | Use class components for new code |

## Quick Fixes

- **"Adjacent JSX elements must be wrapped"** → Wrap in `<div>` or `<React.Fragment>` / `<>`
- **"Cannot read property of undefined"** → Use optional chaining `user?.name` or check if exists
- **"Each child should have a unique key prop"** → Add `key={uniqueValue}` to list items
- **"Hook can only be called inside function components"** → Move hooks to function component body

## Gotchas

⚠️ **JSX is not HTML** - Use `className` not `class`, `htmlFor` not `for`
⚠️ **Props are immutable** - Never modify props directly, create new objects/arrays
⚠️ **Keys in lists matter** - Don't use array index as key if list can change order
⚠️ **Case sensitivity** - Component names must start with capital letter

## Checklist

- [ ] Understand component-based architecture
- [ ] Know the difference between JSX and HTML
- [ ] Can create and use function components
- [ ] Understand props and how to pass data down
- [ ] Know what Virtual DOM is and why it's beneficial
- [ ] Can set up a new React project with Vite or CRA
- [ ] Comfortable with JSX syntax and expressions