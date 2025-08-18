# JavaScript 'this' Keyword `v 0.07.25.01`

## Essentials
```javascript
// Global Context
console.log(this); // Window (browser) or global (Node.js)

// Method Context
const obj = {
    name: 'John',
    greet() {
        console.log(this.name); // 'John' - 'this' is obj
    }
};

// Arrow Functions (inherit 'this')
const arrowObj = {
    name: 'Alice',
    arrowMethod: () => {
        console.log(this.name); // undefined - no own 'this'
    },
    regularMethod() {
        const arrow = () => console.log(this.name); // 'Alice' - inherits
        arrow();
    }
};

// Explicit Binding
function greet() { console.log(this.name); }
const person = { name: 'Bob' };
greet.call(person);  // 'Bob'
greet.apply(person); // 'Bob' 
greet.bind(person)(); // 'Bob'
```

**Key Concepts**: Context Binding | Method Invocation | Arrow Functions | Explicit Binding | Constructor Context
**When to use**: Understanding object methods, event handlers, class methods, and avoiding context loss

## Syntax Reference

### Context Rules
```javascript
// Rule 1: Global Context
function globalFunc() {
    console.log(this); // Window/global or undefined (strict mode)
}

// Rule 2: Method Context  
const obj = {
    method() {
        console.log(this); // obj
    }
};

// Rule 3: Constructor Context
function Person(name) {
    this.name = name; // 'this' is new instance
}

// Rule 4: Explicit Context (call/apply/bind)
const context = { value: 42 };
function useContext() { console.log(this.value); }
useContext.call(context); // 42

// Rule 5: Arrow Functions (lexical 'this')
const arrow = () => {
    console.log(this); // Inherits from enclosing scope
};
```

### Common Patterns
```javascript
// Pattern 1: Preserving Context in Callbacks
class Counter {
    constructor() {
        this.count = 0;
    }
    
    increment() {
        this.count++;
    }
    
    // Wrong - loses context
    startWrong() {
        setTimeout(this.increment, 1000); // 'this' is not Counter
    }
    
    // Right - bind preserves context
    startBind() {
        setTimeout(this.increment.bind(this), 1000);
    }
    
    // Right - arrow function preserves context
    startArrow() {
        setTimeout(() => this.increment(), 1000);
    }
}

// Pattern 2: Event Handlers
class ButtonHandler {
    constructor(element) {
        this.clickCount = 0;
        // Bind to preserve context
        element.addEventListener('click', this.handleClick.bind(this));
    }
    
    handleClick() {
        this.clickCount++;
        console.log(`Clicked ${this.clickCount} times`);
    }
}

// Pattern 3: Method Chaining
class Calculator {
    constructor(value = 0) {
        this.value = value;
    }
    
    add(n) {
        this.value += n;
        return this; // Return 'this' for chaining
    }
    
    multiply(n) {
        this.value *= n;
        return this;
    }
}

const calc = new Calculator(5).add(3).multiply(2); // 16
```

### Context Binding Methods
```javascript
const person = { name: 'John', age: 30 };

function introduce(greeting, punctuation) {
    return `${greeting}, I'm ${this.name} and I'm ${this.age}${punctuation}`;
}

// call() - immediate invocation with args
const result1 = introduce.call(person, 'Hello', '!');

// apply() - immediate invocation with array
const result2 = introduce.apply(person, ['Hi', '.']);

// bind() - returns new function with bound context
const boundIntroduce = introduce.bind(person);
const result3 = boundIntroduce('Hey', '?');

// Partial binding
const sayHello = introduce.bind(person, 'Hello');
const result4 = sayHello('!');
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use arrow functions for callbacks in methods | Use arrow functions as object methods |
| Bind context when passing methods as callbacks | Rely on implicit 'this' in detached methods |
| Store 'this' reference if needed (`const self = this`) | Forget 'this' context changes in nested functions |
| Use explicit binding (call/apply/bind) when needed | Assume 'this' is the same in all contexts |
| Check 'this' context in strict vs non-strict mode | Use 'this' in global functions unnecessarily |

## Quick Fixes
- **"Cannot read property of undefined"** → 'this' is undefined, use bind() or arrow function
- **"Method doesn't work when passed as callback"** → Bind the method or wrap in arrow function
- **"'this' is window/global in method"** → Check if method is called with dot notation
- **"Arrow function 'this' is wrong"** → Arrow functions inherit 'this', use regular function
- **"Event handler 'this' is wrong element"** → Bind method or use arrow function wrapper

## Gotchas
⚠️ **Arrow functions have no 'this'** - They inherit from lexical scope, can't be bound
⚠️ **Method assignment loses context** - `const fn = obj.method; fn()` loses 'this'
⚠️ **Strict mode changes global 'this'** - Functions have `undefined` instead of global object
⚠️ **Nested functions lose context** - Inner functions don't inherit 'this' from outer
⚠️ **Event handlers bind to element** - `this` refers to the element, not your object

## Context Determination Rules
```javascript
// 1. new keyword - 'this' is new instance
function Constructor() {
    this.prop = 'value'; // 'this' is new object
}

// 2. call/apply/bind - explicit 'this'
func.call(context);   // 'this' is context
func.apply(context);  // 'this' is context
func.bind(context)(); // 'this' is context

// 3. Method call - 'this' is the object
obj.method(); // 'this' is obj

// 4. Regular function call
func(); // 'this' is global/undefined (strict mode)

// 5. Arrow function - inherits 'this'
const arrow = () => {}; // 'this' from enclosing scope
```

## Advanced Patterns
```javascript
// Dynamic Method Binding
const api = {
    baseUrl: 'https://api.example.com',
    
    request(endpoint) {
        return fetch(`${this.baseUrl}${endpoint}`);
    },
    
    // Create bound methods dynamically
    createEndpoint(path) {
        return this.request.bind(this, path);
    }
};

const getUsers = api.createEndpoint('/users');

// Mixin Pattern with Controlled 'this'
const EventMixin = {
    on(event, callback) {
        this._events = this._events || {};
        this._events[event] = this._events[event] || [];
        this._events[event].push(callback);
    },
    
    emit(event, ...args) {
        if (this._events && this._events[event]) {
            this._events[event].forEach(callback => {
                callback.apply(this, args); // Preserve 'this' context
            });
        }
    }
};

// Apply mixin to any object
Object.assign(MyClass.prototype, EventMixin);

// Factory Pattern with Bound Methods
function createCounter(initial = 0) {
    const state = { count: initial };
    
    return {
        increment: function() {
            return ++state.count;
        }.bind(state),
        
        decrement: function() {
            return --state.count;
        }.bind(state),
        
        get current() {
            return state.count;
        }
    };
}
```

## Debugging 'this'
```javascript
// Add debugging to any function
function debugThis(fn, name) {
    return function(...args) {
        console.log(`${name} - 'this' is:`, this);
        console.log(`${name} - 'this' type:`, typeof this);
        console.log(`${name} - 'this' constructor:`, this?.constructor?.name);
        return fn.apply(this, args);
    };
}

// Usage
obj.method = debugThis(obj.method, 'obj.method');

// Test different contexts
function testContext() {
    console.log('Context test:', this);
}

testContext(); // Global context
obj.testMethod = testContext;
obj.testMethod(); // Object context
testContext.call({ custom: 'context' }); // Custom context
```

## Checklist
- [ ] Understand the four 'this' binding rules (new, explicit, implicit, default)
- [ ] Use arrow functions for preserving lexical 'this' in callbacks
- [ ] Bind methods when passing them as callbacks
- [ ] Avoid arrow functions as object methods
- [ ] Check 'this' context in event handlers
- [ ] Use explicit binding (call/apply/bind) when context is ambiguous
- [ ] Store 'this' reference in variables when needed (`const self = this`)
- [ ] Test 'this' behavior in both strict and non-strict modes