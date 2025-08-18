# Arrays `v 0.07.25.01`

## Essentials
```javascript
// Array creation and access
const fruits = ['apple', 'banana', 'orange'];
fruits[0];                     // 'apple' (index access)
fruits.at(-1);                 // 'orange' (negative indexing)
fruits.length;                 // 3

// Common operations
fruits.push('grape');          // Add to end
fruits.pop();                  // Remove from end
fruits.unshift('mango');       // Add to beginning
fruits.shift();                // Remove from beginning
```

**Key Concepts**: Creation & Access | Mutating Methods | Higher-Order Functions | Destructuring | Spread/Rest
**When to use**: Data manipulation, iteration, transformation, filtering, and collection management

## Syntax Reference

### Array Creation
```javascript
// Array literal (most common)
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, 'hello', true, null];

// Array constructor
const empty = new Array();              // []
const withLength = new Array(5);        // Array with 5 empty slots
const withElements = new Array(1, 2, 3); // [1, 2, 3]

// Array.of() - creates from arguments
const arr1 = Array.of(1, 2, 3);        // [1, 2, 3]
const arr2 = Array.of(5);               // [5] (not empty array)

// Array.from() - creates from iterable
const arr3 = Array.from('hello');       // ['h', 'e', 'l', 'l', 'o']
const arr4 = Array.from([1, 2, 3], x => x * 2); // [2, 4, 6]
```

### Accessing Elements
```javascript
const colors = ['red', 'green', 'blue', 'yellow'];

// Index access (0-based)
colors[0];                      // 'red'
colors[2];                      // 'blue'
colors[-1];                     // undefined (no negative indexing)

// at() method - supports negative indexing
colors.at(0);                   // 'red'
colors.at(-1);                  // 'yellow' (last element)
colors.at(-2);                  // 'blue' (second to last)

// First and last elements
const first = colors[0];
const last = colors[colors.length - 1];
const lastWithAt = colors.at(-1);
```

### Mutating Methods
```javascript
const arr = [1, 2, 3];

// Add/remove at end
arr.push(4);                    // [1, 2, 3, 4] - returns new length
arr.push(5, 6);                 // [1, 2, 3, 4, 5, 6] - multiple elements
const removed = arr.pop();      // removes and returns last element

// Add/remove at beginning
arr.unshift(0);                 // [0, 1, 2, 3, 4, 5] - returns new length
const first = arr.shift();      // removes and returns first element

// splice() - add/remove/replace at any position
arr.splice(2, 1);               // Remove 1 element at index 2
arr.splice(2, 0, 'new');        // Insert 'new' at index 2
arr.splice(2, 1, 'replace');    // Replace 1 element at index 2
```

### Non-Mutating Methods
```javascript
const original = [1, 2, 3, 4, 5];

// slice() - extract portion without modifying original
original.slice(1, 3);           // [2, 3] (from index 1 to 2)
original.slice(2);              // [3, 4, 5] (from index 2 to end)
original.slice(-2);             // [4, 5] (last 2 elements)
original.slice();               // [1, 2, 3, 4, 5] (shallow copy)

// concat() - combine arrays
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = arr1.concat(arr2);     // [1, 2, 3, 4]
const multi = arr1.concat(arr2, [5, 6]); // [1, 2, 3, 4, 5, 6]
```

### Search Methods
```javascript
const fruits = ['apple', 'banana', 'orange', 'banana'];

// indexOf/lastIndexOf - find element position
fruits.indexOf('banana');       // 1 (first occurrence)
fruits.lastIndexOf('banana');   // 3 (last occurrence)
fruits.indexOf('grape');        // -1 (not found)

// includes() - check if element exists
fruits.includes('apple');       // true
fruits.includes('grape');       // false

// find/findIndex - search with condition
const numbers = [1, 2, 3, 4, 5];
const found = numbers.find(n => n > 3);      // 4 (first match)
const index = numbers.findIndex(n => n > 3); // 3 (index of first match)

const users = [{name: 'John', age: 30}, {name: 'Jane', age: 25}];
const user = users.find(u => u.name === 'Jane'); // {name: 'Jane', age: 25}
```

## Higher-Order Methods
```javascript
const numbers = [1, 2, 3, 4, 5];

// map() - transform each element
const doubled = numbers.map(n => n * 2);           // [2, 4, 6, 8, 10]
const squared = numbers.map(n => n ** 2);          // [1, 4, 9, 16, 25]

// filter() - select elements that pass test
const evens = numbers.filter(n => n % 2 === 0);   // [2, 4]
const greaterThan3 = numbers.filter(n => n > 3);  // [4, 5]

// reduce() - accumulate to single value
const sum = numbers.reduce((acc, n) => acc + n, 0);     // 15
const product = numbers.reduce((acc, n) => acc * n, 1); // 120
const max = numbers.reduce((acc, n) => Math.max(acc, n), -Infinity); // 5

// some() - test if any element passes
const hasEven = numbers.some(n => n % 2 === 0);   // true
const hasNegative = numbers.some(n => n < 0);     // false

// every() - test if all elements pass
const allPositive = numbers.every(n => n > 0);    // true
const allEven = numbers.every(n => n % 2 === 0);  // false

// forEach() - execute function for each element
numbers.forEach(n => console.log(n));             // Prints each number
numbers.forEach((n, i) => console.log(`${i}: ${n}`)); // With index
```

### Sorting and Reversing
```javascript
const fruits = ['banana', 'apple', 'orange'];
const numbers = [10, 5, 40, 25, 1];

// sort() - sorts in place (lexicographically by default)
fruits.sort();                          // ['apple', 'banana', 'orange']
numbers.sort();                         // ['1', '10', '25', '40', '5'] (strings!)
numbers.sort((a, b) => a - b);          // [1, 5, 10, 25, 40] (numeric ascending)
numbers.sort((a, b) => b - a);          // [40, 25, 10, 5, 1] (numeric descending)

// Sort objects
const users = [{name: 'John', age: 30}, {name: 'Jane', age: 25}];
users.sort((a, b) => a.age - b.age);    // Sort by age
users.sort((a, b) => a.name.localeCompare(b.name)); // Sort by name

// reverse() - reverses in place
const arr = [1, 2, 3, 4, 5];
arr.reverse();                          // [5, 4, 3, 2, 1]

// Non-mutating reverse
const original = [1, 2, 3];
const reversed = [...original].reverse(); // [3, 2, 1]
```

## Destructuring and Spread
```javascript
// Array destructuring
const colors = ['red', 'green', 'blue'];
const [first, second, third] = colors;
const [r, , b] = colors;                // Skip middle element
const [x, y, z, w = 'yellow'] = colors; // Default value

// Rest operator in destructuring
const numbers = [1, 2, 3, 4, 5];
const [head, ...tail] = numbers;        // head: 1, tail: [2, 3, 4, 5]

// Spread operator
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];    // [1, 2, 3, 4, 5, 6]
const copy = [...arr1];                 // [1, 2, 3] (shallow copy)
const withExtra = [0, ...arr1, 99];     // [0, 1, 2, 3, 99]

// Function arguments
function sum(a, b, c) { return a + b + c; }
const nums = [1, 2, 3];
sum(...nums);                           // 6

// Math functions
const scores = [85, 92, 78, 96];
Math.max(...scores);                    // 96
Math.min(...scores);                    // 78
```

## Common Patterns
```javascript
// Pattern 1: Method chaining
const result = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  .filter(n => n % 2 === 0)             // [2, 4, 6, 8, 10]
  .map(n => n * 2)                      // [4, 8, 12, 16, 20]
  .reduce((sum, n) => sum + n, 0);      // 60

// Pattern 2: Remove duplicates
const unique = [...new Set([1, 2, 2, 3, 3, 4])]; // [1, 2, 3, 4]

// Pattern 3: Group by property
const users = [{dept: 'IT', name: 'John'}, {dept: 'HR', name: 'Jane'}];
const grouped = users.reduce((acc, user) => {
  const key = user.dept;
  if (!acc[key]) acc[key] = [];
  acc[key].push(user);
  return acc;
}, {}); // {IT: [...], HR: [...]}

// Pattern 4: Safe array operations
const safeGet = (arr, index, defaultValue = null) => 
  arr[index] ?? defaultValue;
const isEmpty = arr => arr.length === 0;
const last = arr => arr.at(-1);
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `Array.isArray()` to check if value is array | Use `typeof arr === 'object'` for array checking |
| Use `const` for arrays that won't be reassigned | Mutate arrays when immutability is needed |
| Use appropriate method for task (map, filter, reduce) | Use `forEach()` when you need to return something |
| Specify compare function with `sort()` for numbers | Rely on default lexicographic sorting for numbers |
| Use spread operator for copying arrays | Use `slice()` when spread is more readable |

## Quick Fixes
- **`[10, 5, 1].sort()` gives `["1", "10", "5"]`** → Use `arr.sort((a, b) => a - b)` for numeric sort
- **`forEach()` doesn't return anything** → Use `map()` when transforming, `filter()` when selecting
- **Can't break out of `forEach()`** → Use `for...of` loop when you need to break/continue
- **`indexOf()` doesn't work with objects** → Use `findIndex()` with comparison function
- **Mutating original array accidentally** → Use spread `[...arr]` or `slice()` to copy first

## Gotchas
⚠️ **Array Constructor**: `new Array(5)` creates empty array with 5 slots, not `[5]`
⚠️ **Sort Default**: `[10, 5, 1].sort()` sorts as strings, not numbers
⚠️ **Negative Indexing**: `arr[-1]` is `undefined`, use `arr.at(-1)` instead
⚠️ **Empty Slots**: Sparse arrays (`[1, , 3]`) behave differently in some methods
⚠️ **Reference vs Value**: Arrays are objects, so `[1, 2] === [1, 2]` is `false`

## Checklist
- [ ] Use appropriate creation method (`[]`, `Array.of()`, `Array.from()`)
- [ ] Choose correct method for the task (transform, filter, accumulate, test)
- [ ] Handle empty arrays and edge cases appropriately
- [ ] Use `const` for arrays that won't be reassigned
- [ ] Consider immutability - use non-mutating methods when needed