# Variables & Data Types `v 0.07.25.01`

## Essentials
```javascript
// Modern variable declarations
const PI = 3.14159;           // Cannot reassign
let counter = 0;              // Can reassign
// var oldStyle;              // Avoid - use let/const

// Primitive types
let text = "Hello World";     // string
let number = 42;              // number
let isTrue = true;            // boolean
let empty = null;             // null
let notSet = undefined;       // undefined
let unique = Symbol("id");    // symbol
let bigNum = 123n;            // bigint

// Reference types
let obj = { name: "John" };   // object
let arr = [1, 2, 3];          // array
let fn = () => "Hello";       // function
```

**Key Concepts**: Block Scope | Hoisting | Type Coercion | Strict Equality | Mutability
**When to use**: Variable declarations, data storage, type checking, comparisons

## Syntax Reference

### Variable Declarations
```javascript
// const: Block-scoped, cannot reassign, must initialize
const user = { name: "John" };
user.age = 25;              // OK: modify object contents
// user = {};               // Error: cannot reassign

// let: Block-scoped, can reassign
let count = 0;
count = 1;                  // OK: reassignment allowed
// let count = 2;           // Error: cannot redeclare in same scope

// var: Function-scoped, avoid in modern code
var legacy = "old style";   // Hoisted, function-scoped
```

### Scope Behavior
```javascript
function scopeDemo() {
    // Function scope
    var functionScoped = "accessible anywhere in function";
    
    if (true) {
        // Block scope
        var stillFunction = "var ignores blocks";
        let blockOnly = "only in this block";
        const alsoBlock = "also only in this block";
    }
    
    console.log(stillFunction);  // Works
    // console.log(blockOnly);   // Error: not accessible
}

// Loop variable gotcha
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // Prints: 3, 3, 3
}

for (let j = 0; j < 3; j++) {
    setTimeout(() => console.log(j), 100); // Prints: 0, 1, 2
}
```

### Primitive Data Types
```javascript
// String: Immutable text
let name = "John";
let template = `Hello ${name}`;     // Template literals
let escaped = "He said \"Hello\"";  // Escape characters

// Number: 64-bit floating point
let int = 42;
let float = 3.14;
let hex = 0xFF;                     // 255
let binary = 0b1010;                // 10
let scientific = 2.5e3;             // 2500

// Boolean: true/false
let isActive = true;
let isComplete = false;

// null: Intentional absence
let empty = null;
console.log(typeof null);           // "object" (JavaScript bug!)

// undefined: Uninitialized
let notSet;
console.log(notSet);                // undefined

// Symbol: Unique identifier
let id = Symbol("user");
let globalSym = Symbol.for("global");

// BigInt: Large integers
let big = 123456789012345678901234567890n;
let bigInt = BigInt(999999999999999999);
```

### Reference Data Types
```javascript
// Object: Key-value pairs
let person = {
    name: "John",
    age: 25,
    greet() { return `Hello, I'm ${this.name}`; }
};

// Array: Ordered collection
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "hello", true, { id: 1 }];

// Function: Reusable code blocks
function declaration() { return "declared"; }
let expression = function() { return "expressed"; };
let arrow = () => "arrow function";
```

### Type Checking & Conversion
```javascript
// typeof operator
console.log(typeof 42);             // "number"
console.log(typeof "hello");        // "string"
console.log(typeof true);           // "boolean"
console.log(typeof undefined);      // "undefined"
console.log(typeof null);           // "object" (bug!)
console.log(typeof []);             // "object"
console.log(typeof {});             // "object"
console.log(typeof function(){});   // "function"

// Explicit conversion
let num = Number("123");            // 123
let str = String(123);              // "123"
let bool = Boolean("hello");        // true

// Check for arrays specifically
Array.isArray([1, 2, 3]);          // true
Array.isArray("hello");             // false
```

### Equality Comparison
```javascript
// Strict equality (===) - No type coercion
5 === 5;                    // true
5 === "5";                  // false
null === null;              // true
null === undefined;         // false

// Loose equality (==) - With type coercion
5 == "5";                   // true (string coerced to number)
true == 1;                  // true (boolean to number)
null == undefined;          // true (special case)
[] == false;                // true (array to primitive)
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `const` by default | Use `var` in modern code |
| Use `let` when reassignment needed | Rely on loose equality (`==`) |
| Use strict equality (`===`) | Compare different types without conversion |
| Initialize const variables | Assume `typeof null === "null"` |
| Check `Array.isArray()` for arrays | Trust automatic type coercion |
| Use template literals for strings | Modify const primitive values |

## Quick Fixes
- **TypeError: Assignment to constant variable** → Use `let` instead of `const` for reassignment
- **ReferenceError: Cannot access before initialization** → Move declaration before usage (TDZ)
- **Unexpected type coercion** → Use strict equality (`===`) instead of loose (`==`)
- **Variable not accessible** → Check block scope - use `let`/`const` in correct scope
- **Loop variable closure issues** → Use `let` instead of `var` in loop declarations
- **`typeof null` returns "object"** → Check `value === null` explicitly

## Gotchas
⚠️ **Hoisting**: `var` declarations are moved to top but values stay in place
⚠️ **Temporal Dead Zone**: `let`/`const` exist but can't be accessed before declaration  
⚠️ **`typeof null`**: Returns `"object"` due to JavaScript bug
⚠️ **Array Type**: Arrays return `"object"` with `typeof` - use `Array.isArray()`
⚠️ **Const Objects**: `const` prevents reassignment, not object mutation
⚠️ **Type Coercion**: `==` performs unexpected conversions (`[] == false` is `true`)
⚠️ **Loop Variables**: `var` in loops creates function-scoped variable causing closure issues

## Falsy Values (Convert to `false`)
```javascript
false, 0, -0, 0n, "", null, undefined, NaN
```

## Truthy Values (Convert to `true`)
```javascript
// Everything else, including:
true, 1, -1, "hello", "0", [], {}, function(){}
```

## Type Coercion Examples
```javascript
// String concatenation
"5" + 3        // "53"
"Hello" + true // "Hellotrue"

// Arithmetic operations
"5" - 3        // 2
"5" * 3        // 15
true + 1       // 2
false + 1      // 1

// Common gotchas
[] + []        // ""
[] + {}        // "[object Object]"
{} + []        // 0 (in some contexts)
![] == false   // true
```

## Checklist
- [ ] Use `const` for values that won't be reassigned
- [ ] Use `let` when you need to reassign the variable
- [ ] Avoid `var` completely in new code
- [ ] Use strict equality (`===`) for comparisons
- [ ] Check `Array.isArray()` instead of `typeof` for arrays
- [ ] Handle `null` checks explicitly (`value === null`)
- [ ] Use template literals for string interpolation
- [ ] Initialize `const` variables at declaration
- [ ] Be aware of block scope with `let`/`const`
- [ ] Understand falsy/truthy values for conditional logic