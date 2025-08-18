# Type Conversion `v 0.07.25.01`

## Essentials
```javascript
// Implicit conversion (coercion)
"5" + 3;                   // "53" (number to string)
"10" - 5;                  // 5 (string to number)
"5" == 5;                  // true (type coercion)
"5" === 5;                 // false (no coercion)

// Explicit conversion
String(123);               // "123"
Number("456");             // 456
Boolean(0);                // false
parseInt("42px");          // 42
parseFloat("3.14abc");     // 3.14
```

**Key Concepts**: Implicit Coercion | Explicit Conversion | Truthy/Falsy | Comparison Operators | Type Guards
**When to use**: Data validation, user input processing, API responses, and ensuring type safety

## Syntax Reference

### Implicit Type Conversion (Coercion)
```javascript
// String concatenation with +
"Hello" + " World";        // "Hello World"
"5" + 3;                   // "53" (number becomes string)
"5" + 3 + 2;               // "532" (left to right)
5 + 3 + "2";               // "82" (addition first, then concat)

// Arithmetic operations (-, *, /, %)
"10" - 5;                  // 5 (string to number)
"10" * 2;                  // 20 (string to number)
"hello" - 5;               // NaN (invalid conversion)

// Comparison operations
0 == false;                // true (both become 0)
"0" == false;              // true (both become 0)
null == undefined;         // true (special case)

// Logical operations
true && "hello";           // "hello" (last truthy value)
false || "world";          // "world" (first truthy value)
"" || null || "default";   // "default" (first truthy)

// Unary plus operator
+"42";                     // 42 (string to number)
+true;                     // 1 (boolean to number)
+null;                     // 0 (null to number)
+undefined;                // NaN (undefined to number)
```

### Explicit Type Conversion
```javascript
// String() function
String(123);               // "123"
String(true);              // "true"
String(null);              // "null"
String([1, 2, 3]);         // "1,2,3"
String({a: 1});            // "[object Object]"

// Number() function
Number("123");             // 123
Number("");                // 0 (empty string)
Number("123abc");          // NaN (invalid format)
Number(true);              // 1
Number(false);             // 0
Number(null);              // 0
Number(undefined);         // NaN
Number([1]);               // 1 (single element)
Number([1, 2]);            // NaN (multiple elements)

// Boolean() function
Boolean(1);                // true
Boolean(0);                // false
Boolean("hello");          // true
Boolean("");               // false
Boolean([]);               // true (empty array is truthy)
Boolean({});               // true (empty object is truthy)
Boolean(null);             // false
Boolean(undefined);        // false
```

### parseInt() and parseFloat()
```javascript
// parseInt() - converts to integer
parseInt("123");           // 123
parseInt("123.45");        // 123 (ignores decimal)
parseInt("123abc");        // 123 (stops at non-digit)
parseInt("abc123");        // NaN (starts with non-digit)
parseInt("0x10");          // 16 (hexadecimal)
parseInt("1010", 2);       // 10 (binary with radix)
parseInt("FF", 16);        // 255 (hex with radix)

// parseFloat() - converts to floating point
parseFloat("123.45");      // 123.45
parseFloat("123.45abc");   // 123.45 (stops at non-digit)
parseFloat("1.23e-4");     // 0.000123 (scientific notation)
parseFloat("   3.14   ");  // 3.14 (ignores whitespace)
```

### Truthy and Falsy Values
```javascript
// Falsy values (only 8 in JavaScript)
Boolean(false);            // false
Boolean(0);                // false
Boolean(-0);               // false
Boolean(0n);               // false (BigInt zero)
Boolean("");               // false (empty string)
Boolean(null);             // false
Boolean(undefined);        // false
Boolean(NaN);              // false

// Everything else is truthy
Boolean("0");              // true (string with zero)
Boolean("false");          // true (string with false)
Boolean([]);               // true (empty array)
Boolean({});               // true (empty object)
Boolean(function(){});     // true (function)
Boolean(Infinity);         // true

// Double negation for boolean conversion
!!"hello";                 // true
!!0;                       // false
!![];                      // true
```

### toString() and valueOf() Methods
```javascript
// toString() method
(123).toString();          // "123"
(255).toString(16);        // "ff" (hexadecimal)
(8).toString(2);           // "1000" (binary)
[1, 2, 3].toString();      // "1,2,3"

// valueOf() method
(123).valueOf();           // 123
"hello".valueOf();         // "hello"
new Number(42).valueOf();  // 42

// Custom conversion methods
let obj = {
    value: 42,
    toString() { return `Value: ${this.value}`; },
    valueOf() { return this.value; }
};
String(obj);               // "Value: 42" (toString)
Number(obj);               // 42 (valueOf)
```

## Common Patterns
```javascript
// Pattern 1: Safe type conversion with defaults
function safeNumber(value, defaultValue = 0) {
    const num = Number(value);
    return isNaN(num) ? defaultValue : num;
}
safeNumber("42");          // 42
safeNumber("abc", 0);      // 0

// Pattern 2: Type guards for validation
function isValidNumber(value) {
    return typeof value === 'number' && !isNaN(value) && isFinite(value);
}
isValidNumber(42);         // true
isValidNumber(NaN);        // false

// Pattern 3: Normalize input to specific type
function normalizeToString(value) {
    return value == null ? '' : String(value);
}
normalizeToString(null);   // ""
normalizeToString(42);     // "42"

// Pattern 4: Boolean conversion for conditionals
function toBool(value) {
    return Boolean(value);
}
// Or use double negation
function toBoolAlt(value) {
    return !!value;
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use explicit conversion for clarity (`String()`, `Number()`) | Rely on implicit coercion (`+`, `""`) |
| Use strict equality (`===`) to avoid coercion | Use loose equality (`==`) unless necessary |
| Always specify radix with `parseInt("10", 10)` | Use `parseInt()` without radix parameter |
| Use `Number.isNaN()` for NaN checking | Use `isNaN()` which coerces values |
| Handle null/undefined before conversion | Convert without checking for null/undefined |

## Quick Fixes
- **`"5" + 3` gives `"53"` not `8`** → Use `Number("5") + 3` or `+"5" + 3`
- **`parseInt("08")` in old environments** → Always use `parseInt("08", 10)`
- **`NaN === NaN` is `false`** → Use `Number.isNaN(value)` to check for NaN
- **`[] == false` is `true`** → Use `arr.length === 0` to check empty arrays
- **`"" == 0` is `true`** → Use `=== 0` for strict number comparison

## Gotchas
⚠️ **String + Number**: `"5" + 3` becomes `"53"`, but `"5" - 3` becomes `2`
⚠️ **Array Conversion**: `[1] == 1` is true, but `[1] === 1` is false
⚠️ **Empty Values**: `""`, `0`, `false`, `null`, `undefined` are all falsy but behave differently
⚠️ **Object Truthiness**: Empty arrays `[]` and objects `{}` are truthy
⚠️ **NaN Comparison**: `NaN` never equals anything, including itself

## Checklist
- [ ] Use explicit conversion functions (`String()`, `Number()`, `Boolean()`)
- [ ] Always specify radix when using `parseInt()`
- [ ] Use strict equality (`===`) instead of loose equality (`==`)
- [ ] Validate input types before conversion operations
- [ ] Handle `null` and `undefined` values appropriately in conversions