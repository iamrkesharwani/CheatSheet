# Objects `v 0.07.25.01`

## Essentials
```javascript
// Object creation
const person = { name: 'John', age: 30 };
const user = new Object();
const empty = Object.create(null);

// Property access
person.name;           // Dot notation
person['age'];         // Bracket notation
person?.address?.city; // Optional chaining

// Manipulation
person.email = 'john@example.com';  // Add
delete person.age;                   // Remove
Object.freeze(person);              // Make immutable
```

**Key Concepts**: Creation | Access | Manipulation | Iteration | Copying
**When to use**: Storing related data, creating data structures, modeling real-world entities

## Syntax Reference

### Object Creation
```javascript
// Object literal (most common)
const user = {
  name: 'John',
  age: 30,
  greet() { return `Hello, I'm ${this.name}`; }
};

// Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}
const person = new Person('John', 30);

// Class constructor (ES6)
class User {
  constructor(name) { this.name = name; }
}

// Factory function
function createUser(name) {
  return { name, greet: () => `Hi, ${name}` };
}

// Object.create()
const prototype = { speak() { return 'Hello'; } };
const obj = Object.create(prototype);
```

### Property Access & Modification
```javascript
const obj = { name: 'John', 'full-name': 'John Doe' };

// Reading
obj.name;              // Dot notation
obj['full-name'];      // Bracket notation (required for special chars)
obj[dynamicKey];       // Dynamic access

// Writing
obj.age = 30;          // Add/modify
obj['email'] = 'john@example.com';

// Deletion
delete obj.age;        // Remove property

// Safe access
obj?.address?.city;    // Optional chaining (ES2020)
obj.city ?? 'Unknown'; // Nullish coalescing
```

### Destructuring
```javascript
const user = { name: 'John', age: 30, city: 'NYC' };

// Basic destructuring
const { name, age } = user;

// Rename variables
const { name: fullName, age: years } = user;

// Default values
const { name, country = 'USA' } = user;

// Nested destructuring
const { address: { street, city } } = user;

// Rest operator
const { name, ...otherProps } = user;

// Function parameters
function greet({ name, age = 25 }) {
  return `Hello ${name}, age ${age}`;
}
```

## Common Patterns

### Object Methods & Utilities
```javascript
// Static methods
Object.keys(obj);        // Property names array
Object.values(obj);      // Property values array
Object.entries(obj);     // [key, value] pairs
Object.fromEntries(arr); // Create object from entries

// Property checking
Object.hasOwn(obj, 'prop');     // Modern way (ES2022)
obj.hasOwnProperty('prop');     // Legacy way
'prop' in obj;                  // Includes inherited

// Copying & merging
const copy = { ...original };            // Shallow copy
const merged = { ...obj1, ...obj2 };     // Merge objects
const assigned = Object.assign({}, obj); // Alternative merge

// Iteration
for (const key in obj) {
  if (Object.hasOwn(obj, key)) {
    console.log(key, obj[key]);
  }
}

Object.entries(obj).forEach(([key, value]) => {
  console.log(key, value);
});
```

### Immutability Control
```javascript
// Prevent extensions (no new properties)
Object.preventExtensions(obj);

// Seal (no add/delete, can modify)
Object.seal(obj);

// Freeze (completely immutable - shallow)
Object.freeze(obj);

// Deep freeze
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach(prop => {
    if (obj[prop] && typeof obj[prop] === 'object') {
      deepFreeze(obj[prop]);
    }
  });
  return Object.freeze(obj);
}
```

### Computed Properties & Getters/Setters
```javascript
// Computed property names
const key = 'dynamic';
const obj = {
  [key]: 'value',
  [`${key}Name`]: 'computed'
};

// Getters and setters
const temperature = {
  _celsius: 0,
  get celsius() { return this._celsius; },
  set celsius(value) { this._celsius = value; },
  get fahrenheit() { return (this._celsius * 9/5) + 32; }
};

// Property descriptors
Object.defineProperty(obj, 'readOnly', {
  value: 'constant',
  writable: false,
  enumerable: true,
  configurable: false
});
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `Object.hasOwn()` for property checks | Use `hasOwnProperty()` directly (can be overridden) |
| Use optional chaining for safe access | Access nested properties without checking |
| Use `const` for objects you won't reassign | Use `var` for object declarations |
| Use spread operator for shallow copying | Mutate objects when immutability is needed |
| Use meaningful property names | Use reserved words as property names |
| Validate data before setting properties | Assume all input data is valid |

## Quick Fixes
- **TypeError: Cannot read property of undefined** → Use optional chaining `obj?.prop?.subprop`
- **Object properties not showing in for...in** → Check if properties are enumerable
- **hasOwnProperty not working** → Use `Object.hasOwn(obj, prop)` instead
- **Can't modify object properties** → Check if object is frozen/sealed
- **Nested object changes affect original** → Use deep copy instead of shallow copy
- **Symbol properties not appearing** → Use `Object.getOwnPropertySymbols()`

## Gotchas
⚠️ **Shallow copy trap**: `{...obj}` only copies top-level properties, nested objects are still referenced

⚠️ **Property order**: Object property order is mostly guaranteed, but don't rely on it for logic

⚠️ **this binding**: Arrow functions don't bind `this`, use regular functions for methods

⚠️ **JSON limitations**: `JSON.stringify()` loses functions, symbols, and undefined values

⚠️ **Prototype pollution**: Be careful when merging objects from untrusted sources

## Advanced Patterns

### Object Transformation
```javascript
// Map object values
const mapValues = (obj, fn) => 
  Object.fromEntries(Object.entries(obj).map(([k, v]) => [k, fn(v)]));

// Filter object entries
const filterObject = (obj, predicate) =>
  Object.fromEntries(Object.entries(obj).filter(([k, v]) => predicate(v, k)));

// Pick specific properties
const pick = (obj, keys) =>
  Object.fromEntries(keys.filter(k => k in obj).map(k => [k, obj[k]]));

// Omit specific properties
const omit = (obj, keys) =>
  Object.fromEntries(Object.entries(obj).filter(([k]) => !keys.includes(k)));
```

### Deep Operations
```javascript
// Deep merge
function deepMerge(target, source) {
  const result = { ...target };
  for (const key in source) {
    if (source[key] && typeof source[key] === 'object' && !Array.isArray(source[key])) {
      result[key] = deepMerge(result[key] || {}, source[key]);
    } else {
      result[key] = source[key];
    }
  }
  return result;
}

// Deep equality check
function deepEqual(obj1, obj2) {
  if (obj1 === obj2) return true;
  if (!obj1 || !obj2 || typeof obj1 !== 'object' || typeof obj2 !== 'object') {
    return obj1 === obj2;
  }
  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);
  if (keys1.length !== keys2.length) return false;
  return keys1.every(key => keys2.includes(key) && deepEqual(obj1[key], obj2[key]));
}

// Flatten nested object
function flatten(obj, prefix = '') {
  return Object.entries(obj).reduce((acc, [key, value]) => {
    const newKey = prefix ? `${prefix}.${key}` : key;
    if (value && typeof value === 'object' && !Array.isArray(value)) {
      Object.assign(acc, flatten(value, newKey));
    } else {
      acc[newKey] = value;
    }
    return acc;
  }, {});
}
```

### Proxy Patterns
```javascript
// Validation proxy
const createValidatedObject = (validators = {}) => {
  return new Proxy({}, {
    set(target, prop, value) {
      if (validators[prop] && !validators[prop](value)) {
        throw new Error(`Invalid value for ${prop}: ${value}`);
      }
      target[prop] = value;
      return true;
    }
  });
};

// Observable object
const createObservable = (target) => {
  const listeners = {};
  return new Proxy(target, {
    set(obj, prop, value) {
      const oldValue = obj[prop];
      obj[prop] = value;
      if (listeners[prop]) {
        listeners[prop].forEach(callback => callback(value, oldValue));
      }
      return true;
    },
    get(obj, prop) {
      if (prop === 'on') {
        return (property, callback) => {
          listeners[property] = listeners[property] || [];
          listeners[property].push(callback);
        };
      }
      return obj[prop];
    }
  });
};
```

## Checklist
- [ ] Choose appropriate object creation method for your use case
- [ ] Use `Object.hasOwn()` for reliable property checking
- [ ] Consider immutability requirements (freeze, seal, preventExtensions)
- [ ] Handle nested object access safely with optional chaining
- [ ] Use proper copying method (shallow vs deep) based on needs
- [ ] Validate object structure when working with external data
- [ ] Be aware of prototype chain implications
- [ ] Consider using Proxy for advanced object behavior
- [ ] Test object operations with edge cases (null, undefined, empty objects)