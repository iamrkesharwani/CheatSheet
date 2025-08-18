# JavaScript Functions `v 0.07.25.01`

## Essentials
```javascript
// Function Declaration (hoisted)
function add(a, b) {
    return a + b;
}

// Function Expression (not hoisted)
const subtract = function(a, b) {
    return a - b;
};

// Arrow Function (implicit return)
const multiply = (a, b) => a * b;

// Arrow Function (explicit return)
const divide = (a, b) => {
    return a / b;
};
```

**Key Concepts**: Hoisting | Arrow Functions | Callbacks | Higher-Order Functions | Closures
**When to use**: Creating reusable code blocks, event handling, data processing, and functional programming

## Syntax Reference

### Function Types
```javascript
// 1. Function Declaration
function greet(name) {
    return `Hello, ${name}!`;
}

// 2. Function Expression  
const greet = function(name) {
    return `Hello, ${name}!`;
};

// 3. Arrow Function
const greet = name => `Hello, ${name}!`;

// 4. Method (object function)
const obj = {
    greet(name) {
        return `Hello, ${name}!`;
    }
};
```

### Parameters & Arguments
```javascript
// Default Parameters
function greet(name = 'Guest') {
    return `Hello, ${name}!`;
}

// Rest Parameters
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

// Destructuring Parameters
function processUser({name, age, email = 'not provided'}) {
    return `${name} (${age}) - ${email}`;
}

// Arguments Object (traditional functions only)
function showArgs() {
    console.log(Array.from(arguments));
}
```

### Common Patterns
```javascript
// Pattern 1: Callback Functions
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);

// Pattern 2: Higher-Order Functions
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}
const double = createMultiplier(2);

// Pattern 3: IIFE (Immediately Invoked Function Expression)
(function() {
    const privateVar = 'hidden';
    console.log('IIFE executed');
})();

// Pattern 4: Function Composition
const add1 = x => x + 1;
const multiply2 = x => x * 2;
const composed = x => multiply2(add1(x));

// Pattern 5: Currying
const curriedAdd = a => b => c => a + b + c;
console.log(curriedAdd(1)(2)(3)); // 6
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use arrow functions for callbacks and short functions | Use arrow functions for methods needing `this` |
| Use function declarations for main functions (hoisted) | Rely on hoisting for function expressions |
| Use descriptive function names | Create functions with too many parameters |
| Return early with guard clauses | Nest functions too deeply |
| Use rest parameters instead of `arguments` | Modify `arguments` object directly |

## Quick Fixes
- **"Cannot access before initialization"** → Use function declaration instead of expression
- **"`this` is undefined in arrow function"** → Use regular function or bind context
- **"Too many parameters"** → Use object destructuring or options object
- **"Function not found"** → Check if function expression is defined before use
- **"Stack overflow in recursion"** → Add base case and ensure it's reachable

## Gotchas
⚠️ **Arrow functions don't have their own `this`** - They inherit from enclosing scope
⚠️ **Function declarations are hoisted** - Can be called before definition, expressions cannot
⚠️ **Default parameters don't count toward function.length** - Only parameters before first default
⚠️ **Rest parameters must be last** - Can't have parameters after `...rest`
⚠️ **Arguments object doesn't exist in arrow functions** - Use rest parameters instead

## Function Properties & Methods
```javascript
function example(a, b, c = 'default') {}

console.log(example.name);     // 'example'
console.log(example.length);   // 2 (excludes defaults & rest)

// Call, Apply, Bind
const obj = { name: 'John' };
function greet(greeting) {
    return `${greeting}, ${this.name}!`;
}

greet.call(obj, 'Hello');      // 'Hello, John!'
greet.apply(obj, ['Hello']);   // 'Hello, John!'
const boundGreet = greet.bind(obj, 'Hello');
```

## Advanced Patterns
```javascript
// Memoization
function memoize(fn) {
    const cache = {};
    return function(...args) {
        const key = JSON.stringify(args);
        if (key in cache) return cache[key];
        const result = fn.apply(this, args);
        cache[key] = result;
        return result;
    };
}

// Partial Application
function partial(fn, ...presetArgs) {
    return function(...laterArgs) {
        return fn.apply(this, [...presetArgs, ...laterArgs]);
    };
}

// Pipe Function (left to right composition)
const pipe = (...functions) => input => 
    functions.reduce((acc, fn) => fn(acc), input);

// Async Function Composition
const asyncPipe = (...functions) => async input => {
    let result = input;
    for (const fn of functions) {
        result = await fn(result);
    }
    return result;
};
```

## Checklist
- [ ] Choose appropriate function type (declaration vs expression vs arrow)
- [ ] Consider if function needs its own `this` context
- [ ] Use meaningful parameter names and default values
- [ ] Handle edge cases and invalid inputs
- [ ] Return consistent data types
- [ ] Avoid mutating input parameters
- [ ] Consider using pure functions when possible
- [ ] Add proper error handling for risky operations