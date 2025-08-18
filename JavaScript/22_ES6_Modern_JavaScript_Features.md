# ES6+ Modern JavaScript Features `v 0.07.25.01`

## Essentials
```javascript
// Variable declarations
const API_URL = 'https://api.example.com';
let counter = 0;

// Arrow functions
const add = (a, b) => a + b;
const users = data.map(user => ({ ...user, active: true }));

// Template literals
const message = `Hello ${name}, you have ${count} messages`;

// Destructuring
const { name, age } = user;
const [first, ...rest] = array;

// Optional chaining & nullish coalescing
const street = user?.address?.street ?? 'No address';
```

**Key Concepts**: Block scope | Arrow functions | Template literals | Destructuring | Spread/Rest | Modules | Async iteration
**When to use**: Modern JavaScript development, cleaner syntax, better scoping, and enhanced functionality

## Syntax Reference

### Variable Declarations
```javascript
// Block-scoped, reassignable
let mutableValue = 'can change';
mutableValue = 'changed';

// Block-scoped, immutable reference
const immutableRef = { name: 'John' };
immutableRef.name = 'Jane'; // ✓ Object mutation allowed
// immutableRef = {}; // ✗ Reference reassignment forbidden

// Temporal Dead Zone
console.log(x); // ReferenceError
let x = 5;

// Loop behavior fix
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // Prints: 0, 1, 2
}
```

### Arrow Functions
```javascript
// Basic syntax variations
const greet = () => 'Hello!';
const double = x => x * 2;                    // Single param
const multiply = (a, b) => a * b;             // Multiple params
const process = data => {                     // Block body
  const result = data * 2;
  return result;
};

// Returning objects (wrap in parentheses)
const createUser = (name, age) => ({ name, age });

// 'this' binding inheritance
const obj = {
  name: 'John',
  regularMethod() {
    return this.name; // 'John'
  },
  arrowMethod: () => {
    return this.name; // undefined (inherits from outer scope)
  }
};
```

### Template Literals
```javascript
// Basic interpolation
const user = 'John';
const greeting = `Hello, ${user}! Today is ${new Date().toDateString()}`;

// Multiline strings
const html = `
  <div class="card">
    <h2>${title}</h2>
    <p>${description}</p>
  </div>
`;

// Tagged templates
function highlight(strings, ...values) {
  return strings.reduce((result, string, i) => {
    const value = values[i] ? `<mark>${values[i]}</mark>` : '';
    return result + string + value;
  }, '');
}

const text = highlight`Search for ${term} in ${category}`;
```

### Destructuring
```javascript
// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
const [a, , c] = [1, 2, 3]; // Skip elements
const [x = 10, y = 20] = [5]; // Default values

// Object destructuring
const { name, age, city = 'Unknown' } = user;
const { name: fullName, age: years } = user; // Rename
const { address: { street, city } } = user; // Nested

// Function parameters
const greet = ({ name, age = 0 }) => `Hello ${name}, age ${age}`;
const sum = ([a, b, c]) => a + b + c;

// Swapping variables
[a, b] = [b, a];
```

### Spread & Rest Operators
```javascript
// Array spread
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]
const combined = [...arr1, ...arr2];

// Object spread
const user = { name: 'John', age: 30 };
const updated = { ...user, age: 31, city: 'NYC' };

// Function arguments
const max = Math.max(...numbers);
const greet = (greeting, ...names) => `${greeting} ${names.join(', ')}`;

// Rest in destructuring
const [head, ...tail] = array;
const { name, ...otherProps } = user;
```

## Common Patterns

### Optional Chaining & Nullish Coalescing
```javascript
// Safe property access
const street = user?.address?.street;
const result = obj?.method?.();
const item = array?.[index];

// Nullish coalescing (only null/undefined)
const timeout = config.timeout ?? 5000; // Keeps 0, false, ''
const name = user.name ?? 'Anonymous';

// Combined usage
const theme = user?.preferences?.theme ?? 'light';
```

### Enhanced Objects
```javascript
const name = 'John';
const age = 30;

// Property shorthand & computed keys
const user = {
  name,          // Same as name: name
  age,           // Same as age: age
  [`${prefix}_id`]: 123,

  // Method shorthand
  greet() {
    return `Hello, ${this.name}!`;
  },

  // Async method
  async fetchData() {
    return await fetch('/api/data');
  }
};
```

### Module System
```javascript
// Named exports
export const API_URL = 'https://api.example.com';
export function helper() { /* ... */ }
export { existing as renamed };

// Default export
export default class Calculator {
  add(a, b) { return a + b; }
}

// Mixed exports
export default request;
export { API_URL, timeout };

// Imports
import Calculator from './calculator.js';
import { API_URL, timeout } from './config.js';
import * as utils from './utils.js';

// Dynamic imports
const module = await import('./feature.js');
```

### Generators & Iterators
```javascript
// Generator function
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

// Infinite generator
function* fibonacci() {
  let [a, b] = [0, 1];
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

// Custom iterable
const range = {
  *[Symbol.iterator]() {
    for (let i = this.start; i <= this.end; i++) {
      yield i;
    }
  },
  start: 1,
  end: 5
};

// Usage
for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `const` by default, `let` when reassigning | Use `var` in modern JavaScript |
| Use arrow functions for callbacks and short functions | Use arrow functions as object methods |
| Use template literals for string interpolation | Concatenate strings with `+` operator |
| Use destructuring for multiple values | Access object properties individually |
| Use spread for copying arrays/objects | Use `Object.assign()` or manual copying |
| Use optional chaining for nested access | Write long `&&` chains for safety |

## Quick Fixes
- **"Cannot access before initialization"** → Check for Temporal Dead Zone with `let`/`const`
- **"Arrow function `this` is undefined"** → Use regular function for methods
- **"Unexpected token"** in template → Escape backticks with `\``
- **"Cannot destructure undefined"** → Provide default values `{} = {}`
- **"Spread syntax unexpected"** → Check if target supports spread
- **"Cannot use import outside module"** → Add `type: "module"` to package.json

## Gotchas
⚠️ **`const` objects are mutable** - Only the reference is immutable, not contents
⚠️ **Arrow functions inherit `this`** - Don't use for object methods needing `this`
⚠️ **Template literals execute immediately** - Expressions are evaluated when created
⚠️ **Destructuring creates new variables** - Original object/array remains unchanged
⚠️ **Spread is shallow** - Nested objects/arrays share references
⚠️ **Optional chaining short-circuits** - Entire chain returns `undefined` if any part fails

## Advanced Features

### Symbols & Privacy
```javascript
const _private = Symbol('private');

class MyClass {
  constructor() {
    this[_private] = 'hidden';
  }
  
  getPrivate() {
    return this[_private];
  }
}

// Well-known symbols
class CustomIterable {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
  }
}
```

### Async Iteration
```javascript
// Async generator
async function* fetchPages() {
  let page = 1;
  while (page <= 3) {
    const data = await fetch(`/api/page/${page}`);
    yield await data.json();
    page++;
  }
}

// Usage
for await (const page of fetchPages()) {
  console.log(page);
}
```

### Tagged Templates
```javascript
function sql(strings, ...values) {
  return {
    query: strings.reduce((result, string, i) => {
      return result + string + (values[i] || '');
    }, ''),
    values
  };
}

const query = sql`SELECT * FROM users WHERE id = ${userId}`;
```

## Checklist
- [ ] Replace `var` with `const`/`let` for proper scoping
- [ ] Use arrow functions for callbacks and short functions
- [ ] Implement template literals for string interpolation
- [ ] Apply destructuring for cleaner variable assignments
- [ ] Use spread operator for array/object operations
- [ ] Add optional chaining for safe property access
- [ ] Convert to ES6 modules with import/export
- [ ] Consider generators for complex iteration scenarios
- [ ] Use symbols for truly private properties
- [ ] Implement async iteration for streaming data