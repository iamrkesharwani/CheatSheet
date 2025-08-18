# Fundamentals `v 0.07.25.01`

## Essentials
```javascript
// Basic syntax
let message = "Hello World";
const PI = 3.14159;
var oldStyle = "avoid in modern code";

// Function declaration
function greet(name) {
    return `Hello, ${name}!`;
}

// Adding to HTML
<script src="script.js"></script>
```

**Key Concepts**: Variables | Functions | DOM Manipulation | Event Handling | Scope
**When to use**: Interactive web pages, SPAs, form validation, dynamic content

## Syntax Reference

### Adding JavaScript to HTML
```html
<!-- External (Recommended) -->
<script src="script.js"></script>
<script src="script.js" defer></script>  <!-- Execute after DOM ready -->
<script src="script.js" async></script>  <!-- Execute when ready -->

<!-- Internal -->
<script>
    console.log("JavaScript code here");
</script>

<!-- Inline (Avoid) -->
<button onclick="alert('Hello!')">Click</button>
```

### Variable Declarations
```javascript
// Modern declarations (ES6+)
let variable = "can be reassigned";
const constant = "cannot be reassigned";

// Legacy (avoid)
var oldVariable = "function scoped";

// Valid identifier rules
let validName = true;
let _validName = true;  
let $validName = true;
let valid123 = true;
// let 123invalid = false;  // Invalid start
// let class = false;       // Reserved keyword
```

### Comments
```javascript
// Single-line comment
let value = 42; // End-of-line comment

/* 
   Multi-line comment
   for longer explanations
*/

/**
 * JSDoc comment for documentation
 * @param {string} name - The name parameter
 * @returns {string} Greeting message
 */
function greet(name) {
    return `Hello ${name}`;
}
```

### Code Structure
```javascript
// Statements (end with semicolon)
let message = "Hello";
console.log(message);

// Blocks create scope for let/const
{
    let blockScoped = "only accessible here";
}

// Function blocks
function myFunction() {
    let localVar = "function scope";
    return localVar;
}

// Conditional blocks
if (condition) {
    let ifScoped = "only in if block";
}
```

### Strict Mode
```javascript
"use strict"; // Global strict mode

function strictFunction() {
    "use strict"; // Function-level strict mode
    // Stricter error checking applies here
}

// ES6 modules automatically use strict mode
import { module } from './module.js';
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `let`/`const` instead of `var` | Rely on automatic semicolon insertion |
| Place scripts before closing `</body>` | Use inline JavaScript for complex logic |
| Use external files for reusability | Ignore case sensitivity (myVar ≠ MyVar) |
| Enable strict mode in new projects | Use reserved keywords as identifiers |
| Use meaningful variable names | Mix tabs and spaces for indentation |
| Add semicolons explicitly | Forget script loading impacts performance |

## Quick Fixes
- **ReferenceError: variable is not defined** → Declare with `let`/`const` or check spelling
- **Script not loading** → Check file path and use relative/absolute paths correctly
- **Function not executing** → Ensure script runs after DOM elements exist
- **Strict mode errors** → Declare all variables and avoid deprecated syntax
- **Case sensitivity issues** → Check exact spelling and capitalization
- **Reserved keyword error** → Choose different variable name

## Gotchas
⚠️ **Hoisting**: `var` declarations are moved to top of scope, but not their values
⚠️ **Block Scope**: `let`/`const` are block-scoped, `var` is function-scoped
⚠️ **ASI (Automatic Semicolon Insertion)**: JavaScript adds semicolons automatically, sometimes incorrectly
⚠️ **Script Loading**: Scripts block HTML parsing unless using `async`/`defer`
⚠️ **Global Variables**: Variables without declaration become global (use strict mode to prevent)
⚠️ **Case Sensitivity**: `myVariable`, `MyVariable`, and `MYVARIABLE` are different variables

## Browser JavaScript Engine Process
1. **Parsing**: Source code → Abstract Syntax Tree (AST)
2. **Compilation**: AST → Bytecode  
3. **Execution**: Bytecode → Machine code
4. **Optimization**: Hot code gets JIT compiled for better performance

## Script Loading Best Practices
- **External files**: Better caching and organization
- **Placement**: Before `</body>` for better page load performance  
- **`defer`**: Download in parallel, execute after DOM ready (best for most scripts)
- **`async`**: Download in parallel, execute immediately when ready (for independent scripts)

## Checklist
- [ ] Use `let`/`const` instead of `var`
- [ ] Add "use strict" to new JavaScript files
- [ ] Place script tags before closing `</body>`
- [ ] Use external `.js` files instead of inline code
- [ ] Include semicolons explicitly 
- [ ] Follow consistent naming conventions (camelCase)
- [ ] Use meaningful variable and function names
- [ ] Add comments for complex logic, not obvious code
- [ ] Consider `defer` attribute for script loading
- [ ] Validate HTML and check console for JavaScript errors