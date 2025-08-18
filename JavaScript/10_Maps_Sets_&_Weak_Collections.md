# Maps, Sets & Weak Collections `v 0.07.25.01`

## Essentials
```javascript
// Map - key-value pairs with any key type
const map = new Map([['key', 'value'], [1, 'number']]);
map.set('name', 'John').get('name'); // 'John'

// Set - unique values collection
const set = new Set([1, 2, 2, 3]); // {1, 2, 3}
set.add(4).has(1); // true

// WeakMap - object keys only, garbage collected
const weakMap = new WeakMap();
weakMap.set(obj, 'data');

// WeakSet - unique objects, garbage collected
const weakSet = new WeakSet();
weakSet.add(obj);
```

**Key Concepts**: Map (any keys) | Set (unique values) | WeakMap/WeakSet (garbage collection)
**When to use**: Maps for flexible keys, Sets for uniqueness, Weak collections for memory management

## Syntax Reference

### Map Operations
```javascript
// Creation and basic operations
const map = new Map();
const mapInit = new Map([['key1', 'val1'], ['key2', 'val2']]);

// Core methods
map.set(key, value);    // Add/update (chainable)
map.get(key);           // Retrieve value
map.has(key);           // Check existence
map.delete(key);        // Remove entry
map.clear();            // Remove all
map.size;               // Count (property)

// Iteration
for (let [key, value] of map) { /* entries */ }
for (let key of map.keys()) { /* keys only */ }
for (let value of map.values()) { /* values only */ }
map.forEach((value, key) => { /* callback */ });
```

### Set Operations
```javascript
// Creation and basic operations
const set = new Set();
const setInit = new Set([1, 2, 3, 4]);

// Core methods
set.add(value);         // Add value (chainable)
set.has(value);         // Check existence
set.delete(value);      // Remove value
set.clear();            // Remove all
set.size;               // Count (property)

// Iteration
for (let value of set) { /* values */ }
set.forEach(value => { /* callback */ });
```

### WeakMap & WeakSet
```javascript
// WeakMap - object keys only
const weakMap = new WeakMap();
weakMap.set(objKey, value);  // Objects only as keys
weakMap.get(objKey);         // Retrieve
weakMap.has(objKey);         // Check
weakMap.delete(objKey);      // Remove

// WeakSet - objects only
const weakSet = new WeakSet();
weakSet.add(obj);            // Objects only
weakSet.has(obj);            // Check
weakSet.delete(obj);         // Remove
```

## Common Patterns

### Map Use Cases
```javascript
// Pattern 1: Flexible key types
const userMap = new Map();
userMap.set(1, 'numeric ID');
userMap.set('1', 'string ID');
userMap.set(userObj, 'object key');

// Pattern 2: Memoization
const cache = new Map();
function fibonacci(n) {
  if (cache.has(n)) return cache.get(n);
  const result = n <= 1 ? n : fibonacci(n-1) + fibonacci(n-2);
  cache.set(n, result);
  return result;
}

// Pattern 3: Grouping data
const groupBy = (array, keyFn) => {
  const groups = new Map();
  array.forEach(item => {
    const key = keyFn(item);
    if (!groups.has(key)) groups.set(key, []);
    groups.get(key).push(item);
  });
  return groups;
};
```

### Set Use Cases
```javascript
// Pattern 1: Remove duplicates
const unique = [...new Set([1, 2, 2, 3, 3])]; // [1, 2, 3]

// Pattern 2: Set operations
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);
const union = new Set([...setA, ...setB]);
const intersection = new Set([...setA].filter(x => setB.has(x)));
const difference = new Set([...setA].filter(x => !setB.has(x)));

// Pattern 3: Fast lookups
const allowedUsers = new Set(['admin', 'user1', 'user2']);
if (allowedUsers.has(currentUser)) { /* authorized */ }
```

### WeakMap Use Cases
```javascript
// Pattern 1: Private data
const privateData = new WeakMap();
class User {
  constructor(name) {
    privateData.set(this, { name, secret: 'hidden' });
  }
  getName() { return privateData.get(this).name; }
}

// Pattern 2: Metadata storage
const metadata = new WeakMap();
function attachData(element, data) {
  metadata.set(element, data);
}

// Pattern 3: Object caching
const computedValues = new WeakMap();
function getExpensiveValue(obj) {
  if (computedValues.has(obj)) return computedValues.get(obj);
  const result = expensiveComputation(obj);
  computedValues.set(obj, result);
  return result;
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use Map for non-string keys | Use Object for complex key types |
| Use Set for uniqueness checks | Use Array.includes() for membership |
| Use WeakMap for private data | Store primitives in WeakMap keys |
| Chain Map.set() operations | Forget Map preserves insertion order |
| Convert to Array: `[...set]` | Modify Set while iterating |

## Quick Fixes
- **TypeError: Invalid value used as weak map key** → Use objects only for WeakMap/WeakSet keys
- **Map not JSON serializable** → Convert with `Object.fromEntries(map)` first
- **Set contains duplicates** → Check for object references vs values
- **Cannot iterate WeakMap/WeakSet** → Use regular Map/Set for iteration needs
- **Memory leaks with event listeners** → Use WeakMap to associate data with DOM elements

## Gotchas
⚠️ **Map keys use SameValueZero equality**: `NaN === NaN` in Maps, but `{} !== {}` always
⚠️ **WeakCollections not enumerable**: No `.size`, `.clear()`, or iteration methods
⚠️ **Set with objects**: Each object reference is unique, even with same properties
⚠️ **Map vs Object performance**: Maps better for frequent additions/deletions, Objects better for records

## When to Choose What

### Map vs Object
```javascript
// Use Map when:
// - Keys are not strings/symbols
// - Frequent additions/deletions
// - Need to know size often
// - Need guaranteed iteration order

// Use Object when:
// - Keys are strings (records)
// - Need JSON serialization
// - Need methods/prototypes
// - Working with existing APIs
```

### Set vs Array
```javascript
// Use Set when:
// - Need uniqueness guarantee
// - Fast membership testing O(1)
// - Set operations (union, intersection)

// Use Array when:
// - Need indexed access
// - Allow duplicates
// - Need array methods (map, filter, etc.)
```

### Weak Collections
```javascript
// Use WeakMap when:
// - Associate data with objects
// - Avoid memory leaks
// - Private object properties
// - DOM element metadata

// Use WeakSet when:
// - Track object states
// - Mark objects as processed
// - Prevent circular references
```

## Checklist
- [ ] Choose Map for flexible keys, Object for string-keyed records
- [ ] Use Set for uniqueness, Array for ordered collections with duplicates
- [ ] Consider WeakMap/WeakSet for memory-sensitive object associations
- [ ] Remember: WeakCollections only accept objects as keys/values
- [ ] Convert collections to Arrays when you need array methods