# JavaScript Execution Context & Call Stack `v 0.07.25.01`

## Essentials
```javascript
// Execution Context Creation
console.log(hoistedVar); // undefined (creation phase)
console.log(hoistedFunc()); // "I'm hoisted" (creation phase)

var hoistedVar = "assigned"; // execution phase
function hoistedFunc() { return "I'm hoisted"; }

// Call Stack (LIFO - Last In, First Out)
function first() {
    console.log('First start');
    second(); // Push second() to stack
    console.log('First end');
}

function second() {
    console.log('Second start');
    third(); // Push third() to stack
    console.log('Second end');
}

function third() {
    console.log('Third'); // Pop third() from stack
}

first(); // Stack: [third, second, first, global]
```

**Key Concepts**: Execution Context | Call Stack | Hoisting | Temporal Dead Zone | Memory Management | Lexical Environment
**When to use**: Understanding code execution flow, debugging scope issues, preventing stack overflow, memory optimization

## Syntax Reference

### Execution Context Types
```javascript
// 1. Global Execution Context (GEC)
var globalVar = 'global';
function globalFunc() { return 'global function'; }

// 2. Function Execution Context (FEC)
function createContext() {
    var localVar = 'local';
    let blockVar = 'block';
    const constVar = 'constant';
    
    // New execution context created here
    function nested() {
        console.log(localVar); // Access parent context
    }
    
    return nested;
}

// 3. Eval Execution Context (avoid)
// eval('var evalVar = "eval";'); // Creates new context
```

### Context Creation Phases
```javascript
// Phase 1: Creation/Memory Phase
function creationPhase() {
    // Memory allocated:
    // varVar: undefined
    // funcDecl: function object
    // letVar: <uninitialized> (TDZ)
    // constVar: <uninitialized> (TDZ)
    
    console.log(varVar); // undefined
    console.log(funcDecl); // function
    // console.log(letVar); // ReferenceError (TDZ)
    
    var varVar = 'value';
    let letVar = 'let value';
    const constVar = 'const value';
    
    function funcDecl() {
        return 'declared';
    }
}

// Phase 2: Execution Phase
function executionPhase() {
    console.log('Code executes line by line');
    var assigned = 'now assigned'; // Assignment happens
    console.log(assigned); // 'now assigned'
}
```

### Common Patterns
```javascript
// Pattern 1: Stack Visualization
function stackTrace() {
    console.log('Level 1');
    function level2() {
        console.log('Level 2');
        function level3() {
            console.log('Level 3');
            console.trace(); // Shows current stack
        }
        level3();
    }
    level2();
}

// Pattern 2: Preventing Stack Overflow
// Recursive (dangerous)
function recursiveFactorial(n) {
    if (n <= 1) return 1;
    return n * recursiveFactorial(n - 1); // Can overflow
}

// Iterative (safe)
function iterativeFactorial(n) {
    let result = 1;
    for (let i = 1; i <= n; i++) {
        result *= i;
    }
    return result;
}

// Pattern 3: Context Preservation with Closures
function createCounter() {
    let count = 0; // Preserved in lexical environment
    return function() {
        return ++count; // Accesses preserved context
    };
}

const counter = createCounter();
console.log(counter()); // 1 - context still accessible
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `let`/`const` for block scoping | Rely on `var` hoisting for logic |
| Declare functions before using them | Create deep recursion without base cases |
| Use iterative solutions for large datasets | Ignore stack overflow risks |
| Understand temporal dead zone behavior | Access variables before declaration |
| Monitor memory usage with closures | Create unnecessary execution contexts |

## Quick Fixes
- **"Cannot access before initialization"** → Variable in TDZ, declare before use
- **"Maximum call stack size exceeded"** → Add base case to recursion or use iteration
- **"Unexpected undefined"** → Check hoisting behavior, use `let`/`const`
- **"Memory leak with closures"** → Nullify references to heavy objects in closures
- **"Variable not defined"** → Check scope chain and variable declaration

## Gotchas
⚠️ **Hoisting creates `undefined`, not errors** - `var` declarations are hoisted but not assignments
⚠️ **TDZ exists for `let`/`const`** - Variables exist but can't be accessed before declaration
⚠️ **Each function call creates new context** - Even recursive calls create separate contexts
⚠️ **Closures preserve entire lexical environment** - Can cause memory issues with large objects
⚠️ **Stack is limited in size** - Deep recursion will cause overflow errors

## Memory Management
```javascript
// Stack Memory (automatic cleanup)
function stackMemory() {
    let num = 42;           // Stack: primitive
    let str = 'hello';      // Stack: primitive
    let obj = { a: 1 };     // Stack: reference, Heap: object
    
    // When function exits, stack memory is freed
}

// Heap Memory (garbage collected)
function heapMemory() {
    let user = {            // Heap: object
        name: 'John',
        data: new Array(1000).fill('data')
    };
    
    let reference = user;   // Stack: another reference
    user = null;           // Object still in heap (reference exists)
    reference = null;      // Now eligible for garbage collection
}

// Memory Leak Prevention
function preventLeaks() {
    let heavyObject = { /* large data */ };
    
    return function closure() {
        // Closure keeps heavyObject in memory
        return 'result';
    };
}

// Fix: Only capture what you need
function fixedClosure() {
    let heavyObject = { data: 'needed', other: 'not needed' };
    let needed = heavyObject.data; // Extract only needed data
    
    return function closure() {
        return needed; // Only keeps 'needed', not entire object
    };
}
```

## Call Stack Management
```javascript
// Stack Overflow Example
function dangerous(n) {
    if (n > 0) {
        return dangerous(n - 1); // No tail call optimization in JS
    }
    return n;
}

// Safe Alternatives
// 1. Iterative
function safeIterative(n) {
    while (n > 0) {
        n--;
    }
    return n;
}

// 2. Trampoline (for complex recursion)
function trampoline(fn) {
    while (typeof fn === 'function') {
        fn = fn();
    }
    return fn;
}

function safeTrampolined(n) {
    return function bounce() {
        if (n > 0) {
            n--;
            return bounce; // Return function, don't call
        }
        return n;
    };
}

// Usage: trampoline(safeTrampolined(10000));
```

## Debugging Execution Context
```javascript
// Context Inspector
function inspectContext() {
    console.log('=== Context Inspector ===');
    
    // Check what's hoisted
    console.log('hoistedVar:', typeof hoistedVar); // undefined
    console.log('hoistedFunc:', typeof hoistedFunc); // function
    
    // Check TDZ
    try {
        console.log(letVar);
    } catch (e) {
        console.log('letVar in TDZ:', e.message);
    }
    
    var hoistedVar = 'assigned';
    let letVar = 'accessible now';
    
    function hoistedFunc() {
        return 'hoisted function';
    }
    
    console.log('After declarations:');
    console.log('hoistedVar:', hoistedVar);
    console.log('letVar:', letVar);
    console.log('hoistedFunc():', hoistedFunc());
}

// Stack Trace Helper
function getStackTrace() {
    const stack = new Error().stack;
    console.log('Current stack:', stack);
}

// Performance Monitoring
function monitorExecutionTime(fn, name) {
    return function(...args) {
        const start = performance.now();
        const result = fn.apply(this, args);
        const end = performance.now();
        console.log(`${name} took ${end - start} milliseconds`);
        return result;
    };
}
```

## Advanced Concepts
```javascript
// Lexical Environment Structure
function lexicalEnvironmentDemo() {
    let outer = 'outer value';
    
    function inner() {
        let inner = 'inner value';
        
        // Lexical Environment:
        // {
        //   environmentRecord: { inner: 'inner value' },
        //   outerEnvironmentReference: {
        //     environmentRecord: { outer: 'outer value' },
        //     outerEnvironmentReference: globalEnvironment
        //   }
        // }
        
        console.log(outer, inner); // Scope chain lookup
    }
    
    return inner;
}

// Variable Environment vs Lexical Environment
function environmentTypes() {
    // Variable Environment (var, function declarations)
    var varVariable = 'in variable environment';
    function funcDeclaration() { return 'in variable environment'; }
    
    // Lexical Environment (let, const)
    let letVariable = 'in lexical environment';
    const constVariable = 'in lexical environment';
    
    // Both environments work together for scope resolution
}
```

## Checklist
- [ ] Understand the two phases of execution context creation
- [ ] Know the difference between `var`, `let`, and `const` hoisting
- [ ] Recognize temporal dead zone errors
- [ ] Monitor call stack depth in recursive functions
- [ ] Use iterative solutions for large datasets
- [ ] Understand memory allocation (stack vs heap)
- [ ] Prevent memory leaks with closures
- [ ] Use proper error handling to catch stack overflows