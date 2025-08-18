# JavaScript Scope, Hoisting & Closures `v 0.07.25.01`

## Essentials
```javascript
// Function Scope (var) - accessible throughout function
function example() {
    if (true) {
        var functionScoped = "accessible everywhere in function";
    }
    console.log(functionScoped); // Works!
}

// Block Scope (let/const) - accessible only in block
function example() {
    if (true) {
        let blockScoped = "only accessible in this block";
        const alsoBlockScoped = "me too";
    }
    // console.log(blockScoped); // ReferenceError
}

// Closure - function accessing outer scope variables
function createCounter() {
    let count = 0; // Private variable
    return function() {
        return ++count;
    };
}
const counter = createCounter();
console.log(counter()); // 1
```

**Key Concepts**: Function Scope | Block Scope | Lexical Scope | Hoisting | Temporal Dead Zone | Closures
**When to use**: Understanding variable accessibility, avoiding scope bugs, creating private variables, and managing memory

## Syntax Reference

### Scope Types
```javascript
// Global Scope
var globalVar = "accessible everywhere";

// Function Scope (var)
function functionScope() {
    var funcVar = "accessible throughout function";
    if (true) {
        var stillFuncVar = "also function scoped";
    }
    console.log(stillFuncVar); // Works
}

// Block Scope (let/const)
function blockScope() {
    let funcLet = "function scoped";
    if (true) {
        let blockLet = "block scoped";
        const blockConst = "also block scoped";
    }
    // console.log(blockLet); // ReferenceError
}

// Lexical Scope Chain
function outer() {
    let outerVar = "outer";
    function inner() {
        let innerVar = "inner";
        console.log(outerVar); // Can access outer
    }
    // console.log(innerVar); // Can't access inner
}
```

### Hoisting Behavior
```javascript
// Variable Hoisting
console.log(varHoisted); // undefined (not error)
var varHoisted = "value";

// console.log(letHoisted); // ReferenceError (TDZ)
let letHoisted = "value";

// Function Declaration Hoisting
console.log(hoistedFunc()); // Works! "I'm hoisted"
function hoistedFunc() {
    return "I'm hoisted";
}

// Function Expression Hoisting
// console.log(notHoisted()); // TypeError
var notHoisted = function() {
    return "Not hoisted";
};
```

### Common Patterns
```javascript
// Pattern 1: Private Variables with Closures
function createPrivateCounter() {
    let count = 0; // Private
    return {
        increment: () => ++count,
        decrement: () => --count,
        getCount: () => count
    };
}

// Pattern 2: Module Pattern
const myModule = (function() {
    let privateVar = "private";
    return {
        publicMethod() {
            return privateVar;
        }
    };
})();

// Pattern 3: Loop Variable Capture
// Wrong (var)
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 3, 3, 3
}

// Right (let)
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 0, 1, 2
}

// Right (IIFE)
for (var i = 0; i < 3; i++) {
    (function(index) {
        setTimeout(() => console.log(index), 100); // 0, 1, 2
    })(i);
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `let`/`const` for block scoping | Use `var` unless legacy support needed |
| Declare variables before using them | Rely on hoisting for readability |
| Use closures for data privacy | Create unnecessary closures (memory) |
| Use `let` in loops with async operations | Use `var` in loops with callbacks |
| Nullify closure references when done | Ignore memory implications of closures |

## Quick Fixes
- **"Cannot access before initialization"** → Variable in Temporal Dead Zone, declare before use
- **"Variable is not defined"** → Check if variable is in scope or properly declared
- **"Loop variable has wrong value in callback"** → Use `let` instead of `var` or IIFE
- **"Memory leak with closures"** → Nullify references when closures no longer needed
- **"Unexpected undefined"** → Check for hoisting issues with `var`

## Gotchas
⚠️ **Temporal Dead Zone** - `let`/`const` exist but can't be accessed before declaration
⚠️ **Function vs Variable Hoisting** - Function declarations hoist completely, variables don't
⚠️ **var in loops** - Creates single variable shared across all iterations
⚠️ **Closure Memory** - Closures keep entire outer scope in memory, not just used variables
⚠️ **Block scope shadowing** - Inner `let`/`const` can shadow outer variables completely

## Hoisting Rules
```javascript
// What gets hoisted:
// ✅ var declarations (not assignments)
// ✅ function declarations (completely)
// ✅ let/const declarations (but in TDZ)

// Hoisting order:
// 1. Function declarations
// 2. Variable declarations
// 3. Variable assignments (not hoisted)

function hoistingExample() {
    // This is what actually happens:
    // var a; // hoisted
    // function myFunc() { return "hoisted"; } // hoisted
    
    console.log(a); // undefined
    console.log(myFunc()); // "hoisted"
    
    var a = "assigned";
    function myFunc() {
        return "hoisted";
    }
}
```

## Closure Patterns
```javascript
// Factory Functions
function createAdder(x) {
    return function(y) {
        return x + y;
    };
}
const add5 = createAdder(5);

// Memoization
function memoize(fn) {
    const cache = {};
    return function(...args) {
        const key = JSON.stringify(args);
        if (key in cache) return cache[key];
        return cache[key] = fn.apply(this, args);
    };
}

// Event Handlers with State
function createClickCounter() {
    let clicks = 0;
    return function(event) {
        console.log(`Clicked ${++clicks} times`);
    };
}

// Partial Application
function partial(fn, ...args1) {
    return function(...args2) {
        return fn(...args1, ...args2);
    };
}
```

## Execution Context
```javascript
// Execution Context phases:
// 1. Creation Phase:
//    - Hoisting occurs
//    - 'this' binding
//    - Outer environment reference
// 2. Execution Phase:
//    - Code runs line by line
//    - Variable assignments

// Scope chain resolution:
function outer() {
    let x = "outer";
    function inner() {
        // Looks for x in:
        // 1. inner() scope
        // 2. outer() scope ← found here
        // 3. global scope
        console.log(x);
    }
}
```

## Checklist
- [ ] Use `let`/`const` instead of `var` for block scoping
- [ ] Declare variables before using them to avoid TDZ errors
- [ ] Understand hoisting behavior for debugging
- [ ] Use closures for data privacy and state management
- [ ] Be aware of memory implications with closures
- [ ] Use `let` in loops with asynchronous operations
- [ ] Nullify closure references to prevent memory leaks
- [ ] Consider scope chain when accessing variables