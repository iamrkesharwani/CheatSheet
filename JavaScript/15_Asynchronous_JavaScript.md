# Asynchronous JavaScript `v ES2022`

## Essentials

```javascript
// Promises - handle async operations
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Success!'), 1000);
});

// async/await - cleaner syntax
async function fetchData() {
  try {
    const result = await promise;
    return result;
  } catch (error) {
    console.error(error);
  }
}

// Promise.all - run operations in parallel
const results = await Promise.all([promise1, promise2, promise3]);
```

**Key Concepts**: Event Loop | Callbacks | Promises | async/await | Error Handling

**When to use**: API calls, file operations, timers, any operation that shouldn't block the main thread

## Syntax Reference

### Basic Promise Structure

```javascript
// Creating a promise
const myPromise = new Promise((resolve, reject) => {
  // Async operation
  if (success) {
    resolve(data);
  } else {
    reject(error);
  }
});

// Consuming promises
myPromise
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log('Always runs'));
```

### async/await Patterns

```javascript
// Basic async function
async function getData() {
  const response = await fetch('/api/data');
  const data = await response.json();
  return data;
}

// Error handling with try/catch
async function safeGetData() {
  try {
    const data = await getData();
    return data;
  } catch (error) {
    console.error('Failed to fetch:', error);
    return null;
  }
}

// Multiple await operations
async function processMultiple() {
  const user = await fetchUser();
  const posts = await fetchPosts(user.id);
  const comments = await fetchComments(posts[0].id);
  return { user, posts, comments };
}
```

### Promise Utilities

```javascript
// Promise.all - all must succeed
const allResults = await Promise.all([promise1, promise2, promise3]);

// Promise.allSettled - wait for all to complete
const settled = await Promise.allSettled([promise1, promise2, promise3]);
settled.forEach(result => {
  if (result.status === 'fulfilled') {
    console.log('Success:', result.value);
  } else {
    console.log('Error:', result.reason);
  }
});

// Promise.race - first to complete wins
const fastest = await Promise.race([promise1, promise2, promise3]);

// Promise.any - first successful result
const firstSuccess = await Promise.any([promise1, promise2, promise3]);
```

## Common Patterns

```javascript
// Pattern 1: Timeout wrapper
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Timeout')), ms)
  );
  return Promise.race([promise, timeout]);
}

// Pattern 2: Retry mechanism
async function retry(operation, maxAttempts = 3) {
  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await operation();
    } catch (error) {
      if (attempt === maxAttempts) throw error;
      await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
    }
  }
}

// Pattern 3: Promise queue for rate limiting
class PromiseQueue {
  constructor(concurrency = 1) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }
  
  async add(promiseFunction) {
    return new Promise((resolve, reject) => {
      this.queue.push({ promiseFunction, resolve, reject });
      this.process();
    });
  }
  
  async process() {
    if (this.running >= this.concurrency || this.queue.length === 0) return;
    
    this.running++;
    const { promiseFunction, resolve, reject } = this.queue.shift();
    
    try {
      const result = await promiseFunction();
      resolve(result);
    } catch (error) {
      reject(error);
    } finally {
      this.running--;
      this.process();
    }
  }
}

// Pattern 4: Promisify callback functions
function promisify(callbackFn) {
  return function(...args) {
    return new Promise((resolve, reject) => {
      callbackFn(...args, (error, result) => {
        if (error) reject(error);
        else resolve(result);
      });
    });
  };
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use async/await for cleaner code | Mix promises and callbacks unnecessarily |
| Handle errors with try/catch in async functions | Forget to await async operations |
| Use Promise.all for parallel operations | Use sequential awaits when parallel is possible |
| Create custom error types for better handling | Ignore promise rejections |
| Clean up resources in finally blocks | Forget to handle edge cases in async operations |

## Quick Fixes

- **UnhandledPromiseRejectionWarning** → Add .catch() or try/catch blocks
- **Cannot use await outside async function** → Wrap in async function or use .then()
- **Promise never resolves** → Check if resolve/reject is called in all code paths
- **Sequential when should be parallel** → Use Promise.all instead of multiple awaits
- **Callback hell** → Convert to promises or async/await
- **Memory leaks with timers** → Clear timeouts/intervals and cancel promises

## Gotchas

⚠️ **await in loops**: `for...of` works with await, but `forEach` doesn't. Use `for...of` or `Promise.all`

⚠️ **Promise.all fails fast**: If one promise rejects, all fail. Use `Promise.allSettled` if you need all results

⚠️ **Floating promises**: Unhandled promises can cause memory leaks. Always handle with await, .then(), or .catch()

⚠️ **Async constructors**: Constructors can't be async. Use factory functions or initialize after construction

⚠️ **Event loop blocking**: Large synchronous operations still block. Use `setTimeout` or Web Workers for heavy tasks

## Checklist

- [ ] All async operations have error handling
- [ ] Using Promise.all for parallel operations where possible
- [ ] No unhandled promise rejections in console
- [ ] Async functions properly await their dependencies
- [ ] Resources are cleaned up in finally blocks or with proper cancellation
- [ ] Race conditions are considered and handled
- [ ] Timeouts are implemented for network operations