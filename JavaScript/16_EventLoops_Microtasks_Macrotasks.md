# JavaScript Event Loop & Async Execution `v ES2022`

## Essentials

```javascript
// Event Loop phases - execution priority
console.log('1. Synchronous code');           // Call Stack (highest priority)
Promise.resolve().then(() => 
  console.log('2. Microtask'));               // Microtask Queue
setTimeout(() => 
  console.log('3. Macrotask'), 0);            // Callback Queue (lowest priority)

// Event loop execution order
console.log('Start');
setTimeout(() => console.log('Timeout'), 0);      // Macrotask
Promise.resolve().then(() => console.log('Promise')); // Microtask
console.log('End');
// Output: Start → End → Promise → Timeout
```

**Key Concepts**: Call Stack | Microtask Queue | Callback Queue | Event Loop | Task Scheduling

**When to use**: Understanding async execution order, debugging timing issues, optimizing performance

## Syntax Reference

### Event Loop Structure

```javascript
// Call Stack - LIFO execution
function first() {
  console.log('First');
  second(); // Pushed to stack
}

function second() {
  console.log('Second');
} // Popped from stack

// Microtask Queue - highest priority after call stack
Promise.resolve().then(() => console.log('Microtask'));
queueMicrotask(() => console.log('Another microtask'));

// Callback Queue (Macrotask) - lowest priority
setTimeout(() => console.log('Macrotask'), 0);
```

### Execution Priority Order

```javascript
// Complex execution order example
console.log('1. Sync start');

setTimeout(() => console.log('6. Macrotask'), 0);

Promise.resolve().then(() => {
  console.log('3. Microtask 1');
  return Promise.resolve();
}).then(() => {
  console.log('4. Microtask 2');
});

queueMicrotask(() => console.log('5. queueMicrotask'));

console.log('2. Sync end');

// Order: 1 → 2 → 3 → 4 → 5 → 6
```

### async/await in Event Loop

```javascript
async function asyncExample() {
  console.log('1. Sync part of async function');
  
  // Everything after first await becomes microtask
  await Promise.resolve();
  console.log('2. First microtask');
  
  await Promise.resolve();
  console.log('3. Second microtask');
  
  return 'Complete';
}

// Function call itself is synchronous until first await
asyncExample().then(result => console.log('4. Result:', result));
```

## Common Patterns

```javascript
// Pattern 1: Event loop monitoring
function monitorEventLoop() {
  let start = Date.now();
  
  const checkLoop = () => {
    const now = Date.now();
    const delay = now - start;
    
    if (delay > 100) {
      console.warn(`Event loop blocked for ${delay}ms`);
    }
    
    start = now;
    setTimeout(checkLoop, 0); // Schedule next check
  };
  
  checkLoop();
}

// Pattern 2: Cooperative multitasking
async function cooperativeTask(data) {
  for (let i = 0; i < data.length; i++) {
    processItem(data[i]);
    
    // Yield control every 10 items
    if (i % 10 === 0) {
      await new Promise(resolve => setTimeout(resolve, 0));
    }
  }
}

// Pattern 3: Priority task scheduler
class TaskScheduler {
  schedule(task, priority = 'normal') {
    if (priority === 'high') {
      queueMicrotask(() => task()); // High priority
    } else {
      setTimeout(() => task(), 0);   // Normal priority
    }
  }
}

// Pattern 4: Preventing stack overflow with event loop
function avoidStackOverflow(count) {
  if (count > 0) {
    console.log(count);
    // Use setTimeout to prevent stack overflow
    setTimeout(() => avoidStackOverflow(count - 1), 0);
  }
}

// Pattern 5: Microtask queue exhaustion prevention
function safeMicrotaskChain(count, maxCount = 100) {
  if (count < maxCount) {
    Promise.resolve().then(() => {
      console.log(`Safe microtask ${count}`);
      safeMicrotaskChain(count + 1, maxCount);
    });
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Understand microtasks execute before macrotasks | Mix up execution order expectations |
| Use queueMicrotask for high-priority async tasks | Create infinite microtask loops |
| Yield control in long-running tasks with setTimeout | Block the event loop with synchronous code |
| Monitor event loop performance in production | Assume setTimeout(fn, 0) executes immediately |
| Use async/await understanding it creates microtasks | Nest too many microtasks without yielding |

## Quick Fixes

- **Event loop blocked** → Add setTimeout yields in long operations
- **Unexpected execution order** → Check microtask vs macrotask queue priority
- **setTimeout not executing** → Microtask queue might be saturated
- **Memory leaks in async code** → Clear timeouts and avoid infinite microtask chains
- **DOM not updating** → Use requestAnimationFrame or setTimeout to yield
- **async/await confusion** → Remember first await makes rest of function microtask

## Gotchas

⚠️ **setTimeout(fn, 0) is not immediate**: Goes to macrotask queue, executes after all microtasks

⚠️ **Microtasks can starve macrotasks**: Infinite microtask creation prevents macrotask execution

⚠️ **async function synchronous start**: Code before first await executes synchronously

⚠️ **DOM events are macrotasks**: Click handlers execute after current microtasks finish

⚠️ **Promise.resolve().then() vs await**: Both create microtasks but await pauses function execution

## Checklist

- [ ] Understand call stack → microtask → macrotask execution order
- [ ] Know when to use queueMicrotask vs setTimeout
- [ ] Avoid blocking the event loop with heavy synchronous operations
- [ ] Use cooperative multitasking patterns for long-running tasks
- [ ] Monitor event loop performance in critical applications
- [ ] Understand async/await creates microtasks after first await
- [ ] Prevent infinite microtask loops that starve macrotasks