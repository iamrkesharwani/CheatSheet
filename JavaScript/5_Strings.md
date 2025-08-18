# Strings `v 0.07.25.01`

## Essentials
```javascript
// Creation
let str1 = "double quotes";
let str2 = 'single quotes';  
let str3 = `template literal with ${variable}`;

// Common methods
str.length                    // Get length
str.slice(start, end)        // Extract substring
str.includes("text")         // Check if contains
str.replace("old", "new")    // Replace text
str.split(",")              // Split into array
str.trim()                  // Remove whitespace
str.toUpperCase()           // Convert case
```

**Key Concepts**: Immutable | Methods Return New Strings | Template Literals | Escape Characters
**When to use**: Text manipulation, data processing, user input handling, and string formatting

## Syntax Reference

### String Creation
```javascript
// Literal notation (preferred)
let str1 = "Hello World";
let str2 = 'Single quotes';
let str3 = `Template literal`;

// Constructor (avoid unless converting)
let str4 = String(123);        // "123"
let str5 = String(true);       // "true"

// Template literals with expressions
let name = "John";
let greeting = `Hello ${name}!`;
let calc = `Result: ${2 + 3}`;

// Multi-line strings
let multiLine = `Line 1
Line 2
Line 3`;
```

### Escape Characters
```javascript
// Common escapes
let quotes = "He said \"Hello\"";
let path = "C:\\Users\\John";
let newline = "Line 1\nLine 2";
let tab = "Col1\tCol2";

// Unicode
let emoji = "\u{1F600}";       // 😀
let copyright = "\u00A9";      // ©

// Raw strings (no escape processing)
let raw = String.raw`Path: C:\Users\file.txt`;
```

## Common Patterns

### String Extraction
```javascript
// slice() - most flexible
str.slice(0, 5);              // First 5 characters
str.slice(-3);                // Last 3 characters
str.slice(2, -1);             // From index 2 to second-to-last

// Getting file extension
function getExtension(filename) {
    let lastDot = filename.lastIndexOf('.');
    return lastDot !== -1 ? filename.slice(lastDot + 1) : '';
}
```

### Search and Check
```javascript
// Modern methods (ES6+)
str.includes("text");         // Contains text
str.startsWith("Hello");      // Begins with
str.endsWith(".pdf");         // Ends with

// Index-based
str.indexOf("text");          // First occurrence (-1 if not found)
str.lastIndexOf("text");      // Last occurrence

// Case-insensitive search
str.toLowerCase().includes("hello");
```

### String Replacement
```javascript
// replace() - first occurrence only
str.replace("old", "new");

// replaceAll() - all occurrences (ES2021)
str.replaceAll("old", "new");

// Regex replacement
str.replace(/old/g, "new");           // Global
str.replace(/old/gi, "new");          // Global + case-insensitive

// Function replacement
str.replace(/\d+/g, (match) => parseInt(match) * 2);
```

### Array Conversion
```javascript
// Split string to array
"a,b,c".split(",");           // ["a", "b", "c"]
"hello".split("");            // ["h", "e", "l", "l", "o"]

// Join arrays to string
["a", "b", "c"].join(",");    // "a,b,c"
["a", "b", "c"].join(" ");    // "a b c"

// Parse CSV
function parseCSV(line) {
    return line.split(',').map(item => item.trim());
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use template literals for interpolation | Concatenate with + when templating |
| Use `slice()` for substring extraction | Use deprecated `substr()` method |
| Use `includes()` for simple contains checks | Use `indexOf() !== -1` for modern code |
| Trim user input with `trim()` | Forget to handle whitespace in forms |
| Use `replaceAll()` for multiple replacements | Chain multiple `replace()` calls |
| Use appropriate quotes to avoid escaping | Mix quote types unnecessarily |

## Quick Fixes
- **String won't update** → Strings are immutable; assign result: `str = str.toUpperCase()`
- **Template literal not working** → Use backticks `` ` ``, not quotes `"` or `'`
- **Case-sensitive search issues** → Use `.toLowerCase()` before comparison
- **Unicode emoji counting wrong** → Some emojis count as 2 characters in `.length`
- **Escape characters showing literally** → Use proper escape sequences or raw strings
- **replaceAll() not working** → Check browser support (ES2021) or use regex with `/g` flag

## Gotchas
⚠️ **String immutability**: Methods return new strings, original unchanged  
⚠️ **Template literal scope**: Variables must be in scope when template executes  
⚠️ **Unicode length**: Emojis and special characters may count as multiple units  
⚠️ **Replace vs replaceAll**: `replace()` only replaces first occurrence without regex  
⚠️ **Case sensitivity**: All string methods are case-sensitive by default  
⚠️ **Empty string truthy**: `""` is falsy, but `" "` (space) is truthy  

## Validation & Formatting
```javascript
// Email validation
function isValidEmail(email) {
    return email.includes('@') && 
           email.includes('.') && 
           email.indexOf('@') < email.lastIndexOf('.');
}

// Phone number cleaning
function cleanPhone(phone) {
    return phone.replace(/[\s\-\(\)]/g, '');
}

// Title case
function toTitleCase(str) {
    return str.toLowerCase()
              .split(' ')
              .map(word => word.charAt(0).toUpperCase() + word.slice(1))
              .join(' ');
}

// URL slug creation
function createSlug(title) {
    return title.toLowerCase()
                .trim()
                .replace(/[^a-z0-9]+/g, '-')
                .replace(/^-+|-+$/g, '');
}
```

## Padding & Formatting
```javascript
// Pad with zeros
"5".padStart(3, "0");         // "005"
"5".padEnd(3, "0");           // "500"

// Create aligned columns
function formatTable(data) {
    return data.map(row => 
        row.name.padEnd(20) + row.value.toString().padStart(10)
    ).join('\n');
}

// Progress bar
function progressBar(percent, width = 20) {
    let filled = Math.round(percent * width / 100);
    let bar = '█'.repeat(filled) + '░'.repeat(width - filled);
    return `[${bar}] ${percent}%`;
}
```

## Performance Notes
- Template literals are fast and readable for string building
- `includes()` is faster than `indexOf() !== -1`
- `slice()` is more predictable than `substring()`
- For many replacements, `replaceAll()` is cleaner than regex
- String concatenation with `+` is optimized in modern browsers

## Checklist
- [ ] Use appropriate string creation method (literal vs template)
- [ ] Handle user input with trim() and validation
- [ ] Consider case sensitivity in comparisons
- [ ] Remember strings are immutable - assign results
- [ ] Use modern methods (includes, startsWith, endsWith)
- [ ] Validate and sanitize data before processing
- [ ] Test with edge cases (empty strings, unicode characters)