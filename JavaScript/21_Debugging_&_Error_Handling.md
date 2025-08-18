# JavaScript Debugging & Error Handling `v 0.07.25.01`

## Essentials
```javascript
try {
  riskyOperation();
} catch (error) {
  console.error('Error:', error.message);
} finally {
  cleanup();
}

// Breakpoint
debugger;

// Quick logging
console.log('Debug:', variable);
console.table(arrayOfObjects);
```

**Key Concepts**: try/catch/finally | console methods | DevTools | custom errors | performance profiling
**When to use**: Error handling, debugging code execution, performance analysis, and troubleshooting

## Syntax Reference

### Basic Error Handling
```javascript
// Simple try-catch
try {
  let result = JSON.parse(jsonString);
} catch (error) {
  console.error('Parse failed:', error.message);
}

// With finally block
try {
  performOperation();
} catch (error) {
  handleError(error);
} finally {
  cleanup(); // Always executes
}

// Specific error types
try {
  riskyCode();
} catch (error) {
  if (error instanceof TypeError) {
    // Handle type error
  } else if (error instanceof ReferenceError) {
    // Handle reference error
  }
}
```

### Console Methods
```javascript
// Basic logging
console.log('Message', variable, object);
console.error('Error message');      // Red in console
console.warn('Warning message');     // Yellow in console
console.info('Info message');        // Blue in console

// Structured display
console.table(arrayOfObjects);
console.table(data, ['name', 'age']); // Specific columns

// Grouping
console.group('Group Name');
console.log('Item 1');
console.log('Item 2');
console.groupEnd();

// Timing
console.time('operation');
// ... code to measure
console.timeEnd('operation');

// Counting & tracing
console.count('function calls');
console.trace('Stack trace here');
```

### Custom Errors
```javascript
// Custom error class
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = 'ValidationError';
    this.field = field;
  }
}

// Throwing errors
throw new Error('General error');
throw new ValidationError('Required field', 'email');
throw new TypeError('Expected string');

// Using custom errors
function validateUser(user) {
  if (!user.email) {
    throw new ValidationError('Email required', 'email');
  }
}
```

## Common Patterns

### Async Error Handling
```javascript
// Async/await with try-catch
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error;
  }
}

// Promise error handling
fetchData()
  .then(data => console.log(data))
  .catch(error => console.error('Promise rejected:', error));
```

### Performance Profiling
```javascript
// Basic timing
const start = performance.now();
// ... operation
const end = performance.now();
console.log(`Took ${end - start}ms`);

// Performance marks
performance.mark('start-operation');
// ... operation
performance.mark('end-operation');
performance.measure('duration', 'start-operation', 'end-operation');

// Function profiling
function profileFunction(fn, iterations = 1000) {
  const start = performance.now();
  for (let i = 0; i < iterations; i++) {
    fn();
  }
  console.log(`${iterations} calls: ${performance.now() - start}ms`);
}
```

### Global Error Handling
```javascript
// Catch all errors
window.addEventListener('error', (event) => {
  console.error('Global error:', event.error);
});

// Unhandled promise rejections
window.addEventListener('unhandledrejection', (event) => {
  console.error('Unhandled rejection:', event.reason);
  event.preventDefault(); // Prevent default browser behavior
});
```

## DevTools Navigation

### Essential Shortcuts
- **F12** / **Ctrl+Shift+I** - Open DevTools
- **Ctrl+Shift+C** - Element inspector
- **Ctrl+Shift+J** - Console tab
- **Ctrl+P** - Go to file

### Breakpoint Controls
- **F8** - Continue execution
- **F10** - Step over
- **F11** - Step into
- **Shift+F11** - Step out

### Key Tabs
- **Console** - Execute JS, view logs
- **Sources** - Debug code, set breakpoints
- **Network** - Monitor requests
- **Performance** - Profile execution
- **Application** - Storage, cookies, cache

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use specific error types for clarity | Catch errors without handling them |
| Log meaningful context with errors | Use bare `console.log` in production |
| Set conditional breakpoints for efficiency | Leave `debugger` statements in production |
| Use performance timing for optimization | Ignore unhandled promise rejections |
| Clean up resources in `finally` blocks | Throw generic errors without context |

## Quick Fixes
- **"Uncaught TypeError"** → Check if variable exists before using
- **"Cannot read property of undefined"** → Use optional chaining `obj?.prop`
- **"Promise rejection unhandled"** → Add `.catch()` or try-catch
- **"Script error"** → Check CORS, add error event listener
- **Infinite console logs** → Check for recursive function calls
- **Memory leaks** → Clear intervals, remove event listeners

## Gotchas
⚠️ **Finally blocks always execute** - Even if return/throw in try/catch
⚠️ **Async errors need await** - Regular try-catch won't catch Promise rejections
⚠️ **DevTools changes code behavior** - Breakpoints can affect timing-sensitive code
⚠️ **Console.log shows current state** - Objects logged may change before viewing
⚠️ **Stack traces in minified code** - Use source maps for readable traces

## Checklist
- [ ] Add try-catch around risky operations
- [ ] Handle async errors with proper async/await or .catch()
- [ ] Set up global error handlers for production
- [ ] Remove console.log and debugger statements before deployment
- [ ] Use appropriate console methods (error, warn, info)
- [ ] Add performance monitoring for critical functions
- [ ] Test error scenarios and edge cases
- [ ] Implement proper error logging/reporting system