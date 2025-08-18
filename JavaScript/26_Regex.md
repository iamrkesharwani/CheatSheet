# Regular Expressions (Regex) `v 0.07.25.01`

## Essentials
```javascript
// Creating patterns
const regex1 = /pattern/flags;
const regex2 = new RegExp('pattern', 'flags');

// Basic matching
const result = /hello/.test('hello world');     // true
const match = 'hello world'.match(/h(e)llo/);   // ["hello", "e", ...]
const replaced = 'hello world'.replace(/world/, 'universe');

// Common patterns
const email = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const phone = /^\(\d{3}\) \d{3}-\d{4}$/;
const digits = /\d+/g;
```

**Key Concepts**: Pattern Matching | Character Classes | Quantifiers | Groups & Capturing | Flags
**When to use**: Text validation, data extraction, string manipulation, pattern searching

## Syntax Reference

### Character Classes & Metacharacters
```javascript
// Basic classes
/./          // Any character except newline
/\d/         // Digit (0-9)
/\D/         // Non-digit
/\w/         // Word character (a-z, A-Z, 0-9, _)
/\W/         // Non-word character
/\s/         // Whitespace
/\S/         // Non-whitespace

// Custom character sets
/[abc]/      // Any of: a, b, c
/[^abc]/     // Not a, b, or c
/[a-z]/      // Lowercase letters
/[A-Z]/      // Uppercase letters
/[0-9]/      // Digits (same as \d)
/[a-zA-Z0-9]/ // Alphanumeric
```

### Quantifiers
```javascript
/a*/         // 0 or more 'a'
/a+/         // 1 or more 'a'
/a?/         // 0 or 1 'a' (optional)
/a{3}/       // Exactly 3 'a's
/a{3,}/      // 3 or more 'a's
/a{3,5}/     // 3 to 5 'a's

// Greedy vs non-greedy
/".+"/       // Greedy: matches entire "hello" and "world"
/".+?"/      // Non-greedy: matches "hello" and "world" separately
```

### Anchors & Boundaries
```javascript
/^hello/     // Start of string/line
/world$/     // End of string/line
/\bhello\b/  // Word boundary (whole word)
/\Bhello\B/  // Non-word boundary (inside word)
```

## Common Patterns

### Validation Patterns
```javascript
// Email (basic)
/^[^\s@]+@[^\s@]+\.[^\s@]+$/

// Phone (US format)
/^\(\d{3}\) \d{3}-\d{4}$/

// Password (8+ chars, upper, lower, digit)
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d@$!%*?&]{8,}$/

// URL
/https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)/

// IP Address
/^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/

// Date (MM/DD/YYYY)
/^(0[1-9]|1[0-2])\/(0[1-9]|[12][0-9]|3[01])\/\d{4}$/

// Hex Color
/^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/

// Credit Card
/^\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}$/
```

### Extraction Patterns
```javascript
// Extract emails from text
const extractEmails = (text) => {
  return text.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g) || [];
};

// Extract URLs
const extractUrls = (text) => {
  return text.match(/https?:\/\/[^\s]+/g) || [];
};

// Extract function names from JS code
const extractFunctions = (code) => {
  const matches = [];
  const regex = /function\s+([a-zA-Z_$][a-zA-Z0-9_$]*)\s*\(/g;
  let match;
  while ((match = regex.exec(code)) !== null) {
    matches.push(match[1]);
  }
  return matches;
};
```

## Groups & Capturing

### Basic Grouping
```javascript
// Capturing groups
const match = 'John Doe'.match(/(\w+) (\w+)/);
// match[0] = "John Doe", match[1] = "John", match[2] = "Doe"

// Non-capturing groups
/(?:hello) world/  // Groups but doesn't capture

// Named groups
const named = 'John Doe'.match(/(?<first>\w+) (?<last>\w+)/);
// named.groups.first = "John", named.groups.last = "Doe"

// Backreferences
/(\w+) \1/         // Matches repeated words: "hello hello"
/(?<word>\w+) \k<word>/  // Named backreference
```

### Lookahead & Lookbehind
```javascript
// Positive lookahead
/hello(?= world)/   // "hello" followed by " world"

// Negative lookahead  
/hello(?! world)/   // "hello" NOT followed by " world"

// Positive lookbehind
/(?<=hello )world/  // "world" preceded by "hello "

// Negative lookbehind
/(?<!hello )world/  // "world" NOT preceded by "hello "
```

## Flags & Methods

### Flags
```javascript
/pattern/g    // Global - find all matches
/pattern/i    // Case-insensitive
/pattern/m    // Multiline - ^ and $ match line breaks
/pattern/s    // Dotall - . matches newlines
/pattern/u    // Unicode support
/pattern/y    // Sticky - match from lastIndex
```

### String Methods
```javascript
// Testing
'hello'.match(/l+/g);           // ["ll"]
'hello'.search(/l/);            // 2 (index of first match)
/hello/.test('hello world');    // true
/h(e)llo/.exec('hello');        // ["hello", "e", index: 0, ...]

// Replacing
'hello world'.replace(/l/g, 'x');     // "hexxo worxd"
'hello world'.replaceAll(/l/g, 'x');  // Same as above

// Splitting
'a,b;c:d'.split(/[,:;]/);       // ["a", "b", "c", "d"]
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Escape special characters (\\., \\?, \\+) | Forget to escape dots and other metacharacters |
| Use non-capturing groups (?:) when possible | Create unnecessary capturing groups |
| Use specific character classes over . | Use . when you mean specific characters |
| Test regex with various inputs | Assume regex works without testing |
| Use global flag for replace all | Use replace() without /g for multiple matches |
| Compile regex once and reuse | Create new regex in loops |

## Quick Fixes
- **"Invalid regular expression"** → Check for unescaped special characters
- **Matches too much text** → Use non-greedy quantifiers (+?, *?, {n,m}?)
- **Replace only replaces first match** → Add global flag /g
- **Case sensitivity issues** → Add case-insensitive flag /i
- **Not matching across lines** → Add multiline /m or dotall /s flag
- **Catastrophic backtracking** → Avoid nested quantifiers like (a+)+

## Gotchas
⚠️ **Greedy matching**: Quantifiers are greedy by default - use ? for non-greedy
⚠️ **Dot doesn't match newlines**: Use /s flag or [\s\S] to include newlines
⚠️ **Global flag persistence**: RegExp object remembers lastIndex with /g flag
⚠️ **Special characters in strings**: Use \\\\ for literal backslash in strings
⚠️ **Character class ranges**: [A-z] includes punctuation, use [A-Za-z] instead
⚠️ **Empty matches**: Some patterns can match empty strings infinitely

## Real-World Examples

### Data Cleaning
```javascript
// Remove HTML tags
const stripHtml = (html) => html.replace(/<[^>]*>/g, '');

// Format phone numbers
const formatPhone = (phone) => {
  const cleaned = phone.replace(/\D/g, '');
  const match = cleaned.match(/^(\d{3})(\d{3})(\d{4})$/);
  return match ? `(${match[1]}) ${match[2]}-${match[3]}` : null;
};

// Camel to snake case
const camelToSnake = (str) => 
  str.replace(/[A-Z]/g, letter => `_${letter.toLowerCase()}`);

// Snake to camel case
const snakeToCamel = (str) => 
  str.replace(/_([a-z])/g, (match, letter) => letter.toUpperCase());
```

### Form Validation
```javascript
const validators = {
  email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  phone: /^\(\d{3}\) \d{3}-\d{4}$/,
  zipCode: /^\d{5}(-\d{4})?$/,
  strongPassword: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/,
  
  validate(type, value) {
    return this[type] ? this[type].test(value) : false;
  }
};

// Usage
validators.validate('email', 'user@example.com'); // true
```

### Text Processing
```javascript
// Extract mentions and hashtags
const extractSocial = (text) => {
  const mentions = text.match(/@[a-zA-Z0-9_]+/g) || [];
  const hashtags = text.match(/#[a-zA-Z0-9_]+/g) || [];
  return { mentions, hashtags };
};

// Parse simple CSV
const parseCSV = (csvString) => {
  return csvString.split('\n').map(row => 
    row.split(',').map(cell => cell.trim().replace(/^"|"$/g, ''))
  );
};

// Highlight search terms
const highlightSearch = (text, searchTerm) => {
  const regex = new RegExp(`(${searchTerm})`, 'gi');
  return text.replace(regex, '<mark>$1</mark>');
};
```

## Checklist
- [ ] Escaped all special characters in literal patterns
- [ ] Used appropriate flags (g, i, m, s, u, y)
- [ ] Tested with edge cases and various inputs
- [ ] Avoided catastrophic backtracking patterns
- [ ] Used non-capturing groups where possible
- [ ] Considered performance for large text processing
- [ ] Validated regex works across different browsers/environments