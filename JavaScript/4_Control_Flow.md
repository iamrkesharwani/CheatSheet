# Control Flow `v 0.07.25.01`

## Essentials
```javascript
// Conditionals
if (condition) { /* do something */ }
if (condition) { /* do */ } else { /* alternative */ }

// Switch for multiple values
switch (value) {
  case 'option1': return result1;
  case 'option2': return result2;
  default: return defaultResult;
}

// Loops
for (let i = 0; i < array.length; i++) { /* iterate with index */ }
for (let item of array) { /* iterate over values */ }
while (condition) { /* repeat until false */ }

// Control flow
break;    // exit loop/switch
continue; // skip to next iteration
```

**Key Concepts**: Conditionals | Loops | Flow Control | Guard Clauses
**When to use**: Decision making, repetition, and controlling program execution flow

## Syntax Reference

### Conditional Statements
```javascript
// Basic if statement
if (age >= 18) {
    console.log("Adult");
}

// if-else chain
if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else {
    grade = "F";
}

// Guard clauses (early returns)
function processUser(user) {
    if (!user) return "User not found";
    if (!user.isActive) return "User inactive";
    // Main logic here
    return "User processed";
}

// Switch statement
switch (status) {
    case 200:
        return "Success";
    case 404:
        return "Not Found";
    case 500:
        return "Server Error";
    default:
        return "Unknown Status";
}
```

### Loop Types
```javascript
// Traditional for loop - best for arrays with index needed
for (let i = 0; i < items.length; i++) {
    console.log(`${i}: ${items[i]}`);
}

// for...of - best for iterating values
for (let item of items) {
    console.log(item);
}

// while loop - condition-based repetition
let count = 0;
while (count < 5) {
    console.log(count);
    count++;
}

// do...while - execute at least once
do {
    userInput = prompt("Enter command:");
} while (userInput !== "quit");
```

## Common Patterns

### Array Processing
```javascript
// Find first match and exit
for (let item of array) {
    if (item.id === targetId) {
        console.log("Found:", item);
        break;
    }
}

// Skip invalid items
for (let item of array) {
    if (!item.isValid) continue;
    processItem(item);
}

// Process with index
for (let [index, item] of array.entries()) {
    console.log(`${index}: ${item}`);
}
```

### Validation Patterns
```javascript
// Multiple validation checks
function validateUser(user) {
    if (!user) return { valid: false, error: "No user" };
    if (!user.email) return { valid: false, error: "Email required" };
    if (user.age < 18) return { valid: false, error: "Must be 18+" };
    return { valid: true };
}

// Switch for categorization
function getFileType(extension) {
    switch (extension.toLowerCase()) {
        case 'jpg':
        case 'png':
        case 'gif':
            return 'image';
        case 'pdf':
            return 'document';
        case 'mp4':
        case 'avi':
            return 'video';
        default:
            return 'unknown';
    }
}
```

### Nested Loop Control
```javascript
// Break out of nested loops with labels
outer: for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
        if (matrix[i][j] === target) {
            console.log(`Found at [${i}, ${j}]`);
            break outer; // Exit both loops
        }
    }
}

// Continue outer loop from inner loop
outer: for (let item of items) {
    for (let validator of item.validators) {
        if (!validator.isValid) {
            continue outer; // Skip to next item
        }
    }
    processValidItem(item);
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `for...of` for arrays when you need values | Use `for...in` with arrays (includes prototype properties) |
| Use guard clauses for early returns | Create deeply nested if-else chains |
| Always include `break` in switch cases | Forget `break` statements (causes fall-through) |
| Use descriptive label names like `searchLoop:` | Use generic labels like `loop1:` |
| Use `while` for unknown iteration counts | Use `for` loops with complex conditions |
| Check array bounds in traditional for loops | Assume arrays are never empty |

## Quick Fixes
- **ReferenceError in if block** → Use `let`/`const` instead of `var` for block scope
- **Switch fall-through behavior** → Add `break;` after each case
- **Infinite loop** → Ensure loop condition eventually becomes false
- **Can't break outer loop** → Use labeled break: `outerLoop: for...` then `break outerLoop;`
- **for...in includes inherited properties** → Use `obj.hasOwnProperty(key)` or `Object.keys(obj)`
- **Loop variable leaked to global scope** → Use `let` instead of `var` in for loops

## Gotchas
⚠️ **for...in with arrays**: Iterates over indices as strings and includes prototype properties  
⚠️ **Switch fall-through**: Missing `break` causes execution to continue to next case  
⚠️ **Truthy/falsy confusion**: `0`, `""`, `null`, `undefined`, `false`, `NaN` are all falsy  
⚠️ **Modifying array while iterating**: Can skip elements or cause infinite loops  
⚠️ **Variable hoisting with var**: Loop variables declared with `var` leak outside the loop  
⚠️ **Label scope**: Labels only work with `break` and `continue`, not with functions  

## Performance Notes
- `for` loops are fastest for arrays when index is needed
- `for...of` is nearly as fast and more readable
- `for...in` is slowest, avoid for arrays
- `switch` is optimized for many conditions vs long `if-else` chains
- Early returns/breaks improve performance by avoiding unnecessary checks

## Checklist
- [ ] Use appropriate loop type for the task
- [ ] Include `break` statements in switch cases
- [ ] Handle edge cases (empty arrays, null values)
- [ ] Use meaningful variable names in loops
- [ ] Avoid deeply nested conditions (max 3 levels)
- [ ] Test all code paths including error conditions
- [ ] Consider modern alternatives (array methods, optional chaining)