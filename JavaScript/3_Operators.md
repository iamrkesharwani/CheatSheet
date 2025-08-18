# Operators `v 0.07.25.01`

## Essentials
```javascript
// Arithmetic
let sum = 5 + 3;              // 8
let product = 4 * 2;          // 8
let power = 2 ** 3;           // 8
let remainder = 10 % 3;       // 1

// Assignment
let x = 10;
x += 5;                       // x = 15
x *= 2;                       // x = 30

// Comparison
5 === "5"                     // false (strict equality)
5 == "5"                      // true (loose equality)
10 > 5                        // true

// Logical
true && false                 // false
true || false                 // true
!true                         // false

// Modern operators
user?.profile?.name           // Optional chaining
config.port ?? 3000          // Nullish coalescing
age >= 18 ? "adult" : "minor" // Ternary
```

**Key Concepts**: Type Coercion | Short-Circuit Evaluation | Precedence | Strict vs Loose Equality
**When to use**: Math operations, comparisons, conditionals, safe property access, default values

## Syntax Reference

### Arithmetic Operators
```javascript
// Basic arithmetic
5 + 3        // 8 (addition)
10 - 4       // 6 (subtraction)
6 * 7        // 42 (multiplication)
15 / 3       // 5 (division)
17 % 5       // 2 (modulus/remainder)
2 ** 8       // 256 (exponentiation)

// String concatenation with +
"Hello" + " World"           // "Hello World"
"Age: " + 25                 // "Age: 25"
5 + "3"                      // "53" (number coerced to string)

// Type coercion in arithmetic
"10" - 3     // 7 (string coerced to number)
"5" * 2      // 10 (string coerced to number)
true + 1     // 2 (true = 1, false = 0)
5 + null     // 5 (null = 0)
5 + undefined // NaN (undefined = NaN)

// Unary operators
+x           // Convert to number
-x           // Negate
++x          // Pre-increment (increment then use)
x++          // Post-increment (use then increment)
--x          // Pre-decrement
x--          // Post-decrement
```

### Assignment Operators
```javascript
// Basic assignment
let x = 10;

// Compound assignment
x += 5;      // x = x + 5
x -= 3;      // x = x - 3
x *= 2;      // x = x * 2
x /= 4;      // x = x / 4
x %= 3;      // x = x % 3
x **= 2;     // x = x ** 2

// Logical assignment (ES2021)
x &&= 5;     // x = x && 5 (if x is truthy)
x ||= 10;    // x = x || 10 (if x is falsy)
x ??= 20;    // x = x ?? 20 (if x is null/undefined)

// Bitwise assignment
x &= 3;      // Bitwise AND
x |= 2;      // Bitwise OR
x ^= 1;      // Bitwise XOR
x <<= 1;     // Left shift
x >>= 1;     // Right shift
```

### Comparison Operators
```javascript
// Equality
5 === 5      // true (strict: same type and value)
5 === "5"    // false (different types)
5 == "5"     // true (loose: with type coercion)
null == undefined // true (special case)

// Inequality
5 !== "5"    // true (strict inequality)
5 != "6"     // true (loose inequality)

// Relational
10 > 5       // true
10 >= 10     // true
5 < 10       // true
5 <= 5       // true

// String comparison (lexicographic)
"apple" < "banana"    // true
"10" < "2"           // true (string comparison!)
"Apple" < "apple"    // true (uppercase < lowercase)
```

### Logical Operators
```javascript
// AND (&&) - returns first falsy or last value
true && true         // true
true && false        // false
5 && "hello"         // "hello" (both truthy)
0 && "hello"         // 0 (first falsy)

// OR (||) - returns first truthy or last value
false || "default"   // "default"
5 || "default"       // 5 (first truthy)
"" || null          // null (both falsy)

// NOT (!) - converts to boolean and negates
!true               // false
!0                  // true
!""                 // true
!!value             // convert to boolean

// Short-circuit evaluation
user.name && console.log("Has name");  // Execute if truthy
name || setDefaultName();              // Execute if falsy
```

### Modern Operators (ES2020+)
```javascript
// Optional chaining (?.) - safe property access
user?.profile?.name                    // undefined if any part is null/undefined
api?.getData?.()                       // safe method call
users?.[0]?.email                      // safe array access
config?.server?.["api-url"]            // safe bracket notation

// Nullish coalescing (??) - default for null/undefined only
let port = config.port ?? 3000;       // 3000 only if port is null/undefined
let timeout = config.timeout ?? 5000;  // preserves 0, false, ""

// Ternary operator (? :) - conditional assignment
let status = age >= 18 ? "adult" : "minor";
let message = user.isLoggedIn 
  ? "Welcome back!" 
  : "Please log in";

// Nested ternary (use sparingly)
let grade = score >= 90 ? "A" : 
            score >= 80 ? "B" : 
            score >= 70 ? "C" : "F";
```

### Bitwise Operators
```javascript
// Basic bitwise operations
5 & 3        // 1 (AND: 101 & 011 = 001)
5 | 3        // 7 (OR: 101 | 011 = 111)
5 ^ 3        // 6 (XOR: 101 ^ 011 = 110)
~5           // -6 (NOT: invert all bits)

// Shift operators
5 << 1       // 10 (left shift: multiply by 2)
10 >> 1      // 5 (right shift: divide by 2)
-10 >>> 1    // 2147483643 (unsigned right shift)

// Practical bitwise uses
num % 2 === 0    // Check even
(num & 1) === 0  // Check even (faster)

num * 8          // Multiply by 8
num << 3         // Multiply by 8 (faster)

// Flags/permissions
const PERMISSIONS = {
  READ: 1,     // 001
  WRITE: 2,    // 010
  EXECUTE: 4   // 100
};

let userPerms = PERMISSIONS.READ | PERMISSIONS.WRITE; // 3
let hasRead = (userPerms & PERMISSIONS.READ) !== 0;   // true
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `===` for equality comparisons | Use `==` unless you need coercion |
| Use `??` for null/undefined defaults | Use `\|\|` when 0 or false are valid |
| Use optional chaining for deep objects | Chain more than 3-4 levels deep |
| Use ternary for simple conditions | Nest ternary operators deeply |
| Use parentheses for complex expressions | Rely on operator precedence memory |
| Use meaningful variable names | Use bitwise ops without comments |

## Quick Fixes
- **`5 + "3"` gives `"53"`** → Use `Number()` or `+` to convert: `5 + Number("3")`
- **`"10" < "2"` is `true`** → Convert to numbers: `Number("10") < Number("2")`
- **`null == undefined` is `true`** → Use strict equality: `null === undefined`
- **Optional chaining not working** → Check browser support or use Babel
- **Unexpected type coercion** → Use explicit conversion functions
- **Bitwise operations on large numbers** → JavaScript uses 32-bit signed integers

## Gotchas
⚠️ **String + Number**: Addition with strings always concatenates (`5 + "3" = "53"`)
⚠️ **Loose Equality**: `==` performs unexpected coercion (`[] == false` is `true`)
⚠️ **String Comparison**: `"10" < "2"` is `true` (lexicographic, not numeric)
⚠️ **NaN Comparison**: `NaN === NaN` is `false` (use `Number.isNaN()`)
⚠️ **Operator Precedence**: `2 + 3 * 4` is `14`, not `20` (multiplication first)
⚠️ **Bitwise 32-bit Limit**: Bitwise ops convert to 32-bit signed integers
⚠️ **Nullish vs Logical OR**: `0 ?? 5` is `0`, but `0 || 5` is `5`

## Operator Precedence (High to Low)
```javascript
// 1. Grouping
(2 + 3) * 4          // 20

// 2. Member access & function calls
obj.prop
obj[prop]
func()
obj?.prop

// 3. Unary operators
!value
++value
-value
typeof value

// 4. Exponentiation (right-to-left)
2 ** 3 ** 2          // 512 (2 ** (3 ** 2))

// 5. Multiplication, Division, Modulus
2 + 3 * 4            // 14 (* before +)

// 6. Addition, Subtraction
10 - 5 + 2           // 7 (left to right)

// 7. Shift operators
5 << 1 + 1           // 10 (5 << (1 + 1))

// 8. Relational
5 > 3 == true        // true ((5 > 3) == true)

// 9. Equality
5 == 5 && true       // true ((5 == 5) && true)

// 10. Bitwise AND, XOR, OR
5 | 3 & 1            // 5 (5 | (3 & 1))

// 11. Logical AND
true || false && false // true (|| after &&)

// 12. Logical OR

// 13. Nullish coalescing
null ?? false || true  // false ((null ?? false) || true)

// 14. Ternary (right-to-left)
true ? false ? 1 : 2 : 3  // 2

// 15. Assignment (right-to-left)
a = b = c = 5        // All equal 5
```

## Common Patterns
```javascript
// Default values
let name = user.name || "Guest";           // Falsy fallback
let port = config.port ?? 3000;           // Null/undefined only

// Guard clauses
user.isActive && processUser(user);       // Execute if truthy
!user.isValid && return;                  // Exit if falsy

// Safe property access
let email = user?.contact?.email?.toLowerCase();

// Validation chains
let isValid = data.name && 
              data.email && 
              data.email.includes('@') && 
              data.age >= 18;

// Type checking
typeof value === "string" && value.length > 0
Array.isArray(data) && data.length > 0

// Feature flags with bitwise
const FEATURES = { DARK_MODE: 1, NOTIFICATIONS: 2, ANALYTICS: 4 };
let userFeatures = FEATURES.DARK_MODE | FEATURES.NOTIFICATIONS;
let hasDarkMode = (userFeatures & FEATURES.DARK_MODE) !== 0;

// Conditional assignment
user.role = user.isAdmin ? "admin" : "user";
counter += isIncrement ? 1 : -1;

// Null-safe chaining with defaults
let theme = user?.preferences?.theme ?? 
            localStorage.getItem('theme') ?? 
            'light';
```

## Type Coercion Rules
```javascript
// To String
5 + ""           // "5"
String(5)        // "5"
(5).toString()   // "5"

// To Number
+"5"             // 5
Number("5")      // 5
parseInt("5px")  // 5
parseFloat("5.5") // 5.5

// To Boolean
!!"hello"        // true
Boolean("hello") // true

// Falsy values: false, 0, -0, 0n, "", null, undefined, NaN
// Everything else is truthy
```

## Checklist
- [ ] Use strict equality (`===`) instead of loose (`==`)
- [ ] Use nullish coalescing (`??`) for null/undefined defaults
- [ ] Use optional chaining (`?.`) for safe property access
- [ ] Add parentheses to complex expressions for clarity
- [ ] Be aware of string concatenation with `+` operator
- [ ] Use ternary operators for simple conditional assignments
- [ ] Consider readability over operator precedence cleverness
- [ ] Test edge cases with null/undefined values
- [ ] Use appropriate type conversion functions
- [ ] Comment complex bitwise operations