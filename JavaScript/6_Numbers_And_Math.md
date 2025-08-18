# Numbers & Math `v 0.07.25.01`

## Essentials
```javascript
// Basic operations
let sum = 5 + 3;           // 8
let product = 4 * 7;       // 28
let quotient = 15 / 3;     // 5
let remainder = 17 % 5;    // 2

// Math object methods
Math.round(4.7);           // 5
Math.floor(4.9);           // 4
Math.ceil(4.1);            // 5
Math.random();             // 0 to 1 (exclusive)
Math.max(1, 5, 3);         // 5
Math.min(1, 5, 3);         // 1
```

**Key Concepts**: Arithmetic Operations | Math Object | Number Conversion | Type Checking | BigInt
**When to use**: Mathematical calculations, random generation, number validation, and precision handling

## Syntax Reference

### Basic Math Operations
```javascript
// Arithmetic operators
let a = 10, b = 3;
console.log(a + b);        // 13 (addition)
console.log(a - b);        // 7 (subtraction)
console.log(a * b);        // 30 (multiplication)
console.log(a / b);        // 3.333... (division)
console.log(a % b);        // 1 (modulo/remainder)
console.log(a ** b);       // 1000 (exponentiation)

// Increment/decrement
let counter = 5;
counter++;                 // 6 (post-increment)
++counter;                 // 7 (pre-increment)
counter--;                 // 6 (post-decrement)
--counter;                 // 5 (pre-decrement)
```

### Math Object Methods
```javascript
// Rounding functions
Math.floor(4.9);           // 4 (round down)
Math.ceil(4.1);            // 5 (round up)
Math.round(4.5);           // 5 (round to nearest)
Math.trunc(4.9);           // 4 (remove decimal part)

// Power and roots
Math.pow(2, 3);            // 8 (2^3)
Math.sqrt(16);             // 4 (square root)
Math.abs(-5);              // 5 (absolute value)

// Min/max and random
Math.min(1, 5, 3, 9);      // 1
Math.max(1, 5, 3, 9);      // 9
Math.random();             // Random float 0 to 1
Math.floor(Math.random() * 10); // Random int 0-9

// Sign function
Math.sign(-5);             // -1 (negative)
Math.sign(0);              // 0 (zero)
Math.sign(5);              // 1 (positive)
```

### Number Methods & Conversion
```javascript
// Number formatting
let num = 3.14159;
num.toFixed(2);            // "3.14" (string)
num.toPrecision(3);        // "3.14" (string)

// String to number conversion
parseInt("42");            // 42
parseInt("42.7");          // 42 (ignores decimal)
parseInt("42px");          // 42 (stops at non-digit)
parseFloat("42.7");        // 42.7
parseFloat("42.7px");      // 42.7 (stops at non-digit)

// Number constructor
Number("42");              // 42
Number("42.7");            // 42.7
Number("42px");            // NaN (invalid)
```

### Type Checking & Validation
```javascript
// Check for NaN (Not a Number)
isNaN("hello");            // true
Number.isNaN("hello");     // false (stricter)
Number.isNaN(NaN);         // true

// Check if finite
isFinite(42);              // true
isFinite(Infinity);        // false
Number.isFinite(42);       // true (stricter)

// Check if integer
Number.isInteger(42);      // true
Number.isInteger(42.0);    // true
Number.isInteger(42.7);    // false
```

### BigInt for Large Numbers
```javascript
// Creating BigInt
let big1 = BigInt(123);           // 123n
let big2 = 123n;                  // 123n (literal syntax)

// BigInt operations
let sum = 123n + 456n;            // 579n
let product = 123n * 456n;        // 56088n

// Cannot mix BigInt with regular numbers
// 123n + 123;                    // TypeError!
let mixed = 123n + BigInt(123);   // 246n (correct way)
```

### Special Number Constants
```javascript
// Useful constants
Number.EPSILON;                   // Smallest representable positive number
Number.MAX_SAFE_INTEGER;          // 9007199254740991
Number.MIN_SAFE_INTEGER;          // -9007199254740991
Number.MAX_VALUE;                 // Largest representable number
Number.MIN_VALUE;                 // Smallest positive representable number

// Special values
Number.POSITIVE_INFINITY;         // Infinity
Number.NEGATIVE_INFINITY;         // -Infinity
Number.NaN;                       // NaN
```

## Common Patterns
```javascript
// Pattern 1: Random integer in range
function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
randomInt(1, 6);                  // Dice roll (1-6)

// Pattern 2: Safe number comparison with floating point
function isEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3);         // true (handles floating point precision)

// Pattern 3: Clamp number to range
function clamp(num, min, max) {
  return Math.min(Math.max(num, min), max);
}
clamp(15, 0, 10);                // 10

// Pattern 4: Convert to currency
function toCurrency(amount) {
  return (Math.round(amount * 100) / 100).toFixed(2);
}
toCurrency(19.999);              // "20.00"
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `Number.isNaN()` for strict NaN checking | Use `isNaN()` which coerces values |
| Use `Number.isInteger()` to check integers | Assume `num % 1 === 0` always works |
| Use `Math.trunc()` to remove decimals | Use `parseInt()` for number truncation |
| Handle floating point precision with epsilon | Directly compare floating point results |
| Use BigInt for numbers > MAX_SAFE_INTEGER | Mix BigInt with regular number operations |

## Quick Fixes
- **`0.1 + 0.2 !== 0.3`** → Use `Math.abs(a - b) < Number.EPSILON` for comparison
- **`parseInt("08")` returns 8 not 0** → Always use `parseInt(str, 10)` for base 10
- **`Math.random()` includes 0 but not 1** → Use `Math.floor(Math.random() * n)` for 0 to n-1
- **`NaN === NaN` is false** → Use `Number.isNaN()` to check for NaN
- **Large integers lose precision** → Use BigInt for numbers > `Number.MAX_SAFE_INTEGER`

## Gotchas
⚠️ **Floating Point Precision**: `0.1 + 0.2` equals `0.30000000000000004`, not `0.3`
⚠️ **NaN Comparisons**: `NaN` is not equal to anything, including itself
⚠️ **parseInt Behavior**: `parseInt("123abc")` returns `123`, stopping at first non-digit
⚠️ **Math.random() Range**: Returns 0 to 1 (exclusive of 1), not 1 to 10
⚠️ **BigInt Mixing**: Cannot use arithmetic operators between BigInt and regular numbers

## Checklist
- [ ] Use appropriate rounding method (`Math.floor`, `Math.ceil`, `Math.round`, `Math.trunc`)
- [ ] Handle floating point precision issues in comparisons
- [ ] Validate numbers with `Number.isNaN()` and `Number.isFinite()`
- [ ] Use BigInt for integers larger than `Number.MAX_SAFE_INTEGER`
- [ ] Specify radix when using `parseInt()` to avoid unexpected behavior