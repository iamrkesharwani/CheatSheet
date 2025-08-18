# JavaScript Memory & Performance `v 0.07.25.01`

## Essentials
```javascript
// Memory leak prevention
element.removeEventListener('click', handler);
clearInterval(timerId);
largeObject = null;

// Performance optimization
const debounced = debounce(expensiveFunction, 300);
const throttled = throttle(scrollHandler, 16);
```

**Key Concepts**: Memory Management | Event Cleanup | Function Throttling | Lazy Loading | Web Workers
**When to use**: Preventing memory leaks, optimizing performance, handling expensive operations

## Syntax Reference

### Memory Leak Prevention
```javascript
// Event listener cleanup
function createButton() {
  const button = document.createElement('button');
  const handler = () => console.log('clicked');
  
  button.addEventListener('click', handler);
  
  // Cleanup function
  return () => {
    button.removeEventListener('click', handler);
    button.remove();
  };
}

// Timer cleanup
function createTimer() {
  const timerId = setInterval(() => console.log('tick'), 1000);
  return () => clearInterval(timerId);
}

// Nullify references
let largeData = new Array(1000000).fill('data');
// Use largeData...
largeData = null; // Help garbage collection
```

### Throttling & Debouncing
```javascript
// Throttling - limit execution frequency
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// Debouncing - delay execution until idle
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// Usage
const throttledScroll = throttle(() => updateScrollPosition(), 16);
const debouncedSearch = debounce(query => searchAPI(query), 300);
```

### Web Workers
```javascript
// Create worker pool
class WorkerPool {
  constructor(script, size = 4) {
    this.workers = Array(size).fill().map(() => new Worker(script));
    this.available = [...this.workers];
    this.queue = [];
  }
  
  async execute(data) {
    return new Promise((resolve, reject) => {
      if (this.available.length > 0) {
        this.runTask({ data, resolve, reject });
      } else {
        this.queue.push({ data, resolve, reject });
      }
    });
  }
  
  runTask(task) {
    const worker = this.available.pop();
    worker.onmessage = (e) => {
      this.available.push(worker);
      task.resolve(e.data);
      if (this.queue.length > 0) {
        this.runTask(this.queue.shift());
      }
    };
    worker.postMessage(task.data);
  }
}

// Simple worker execution
async function runInWorker(fn, data) {
  const workerScript = `
    self.onmessage = function(e) {
      const { fn, data } = e.data;
      const func = new Function('return ' + fn)();
      const result = func(data);
      self.postMessage(result);
    };
  `;
  
  const blob = new Blob([workerScript], { type: 'application/javascript' });
  const worker = new Worker(URL.createObjectURL(blob));
  
  return new Promise((resolve) => {
    worker.onmessage = (e) => {
      resolve(e.data);
      worker.terminate();
    };
    worker.postMessage({ fn: fn.toString(), data });
  });
}
```

### Lazy Loading
```javascript
// Intersection Observer for lazy loading
class LazyLoader {
  constructor(options = {}) {
    this.observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.loadElement(entry.target);
          this.observer.unobserve(entry.target);
        }
      });
    }, { rootMargin: '50px', ...options });
  }
  
  loadElement(element) {
    if (element.tagName === 'IMG') {
      element.src = element.dataset.src;
    } else if (element.dataset.component) {
      import(`./components/${element.dataset.component}.js`)
        .then(module => {
          const component = new module.default();
          element.appendChild(component.render());
        });
    }
  }
  
  observe(element) {
    this.observer.observe(element);
  }
}

// Dynamic imports for code splitting
async function loadFeature(featureName) {
  const featureModules = {
    'charts': () => import('./features/charts.js'),
    'editor': () => import('./features/editor.js'),
    'calendar': () => import('./features/calendar.js')
  };
  
  return await featureModules[featureName]();
}
```

### Memory Monitoring
```javascript
// Memory usage tracking
class MemoryMonitor {
  static getUsage() {
    if (performance.memory) {
      return {
        used: performance.memory.usedJSHeapSize,
        total: performance.memory.totalJSHeapSize,
        limit: performance.memory.jsHeapSizeLimit,
        percentage: (performance.memory.usedJSHeapSize / performance.memory.jsHeapSizeLimit) * 100
      };
    }
    return null;
  }
  
  static startMonitoring(interval = 5000) {
    return setInterval(() => {
      const memory = MemoryMonitor.getUsage();
      if (memory && memory.percentage > 80) {
        console.warn('High memory usage:', memory.percentage.toFixed(2) + '%');
      }
    }, interval);
  }
}

// Object pooling for frequent allocations
class ObjectPool {
  constructor(createFn, resetFn, maxSize = 100) {
    this.createFn = createFn;
    this.resetFn = resetFn;
    this.pool = [];
    this.maxSize = maxSize;
  }
  
  acquire() {
    return this.pool.pop() || this.createFn();
  }
  
  release(obj) {
    if (this.pool.length < this.maxSize) {
      this.resetFn(obj);
      this.pool.push(obj);
    }
  }
}
```

### Performance Measurement
```javascript
// Performance timing
class PerformanceProfiler {
  static measure(name, fn) {
    performance.mark(`${name}-start`);
    const result = fn();
    performance.mark(`${name}-end`);
    performance.measure(name, `${name}-start`, `${name}-end`);
    
    const measure = performance.getEntriesByName(name)[0];
    console.log(`${name}: ${measure.duration.toFixed(2)}ms`);
    return result;
  }
  
  static async measureAsync(name, asyncFn) {
    const start = performance.now();
    const result = await asyncFn();
    const end = performance.now();
    console.log(`${name}: ${(end - start).toFixed(2)}ms`);
    return result;
  }
}

// Frame rate monitoring
class FPSMonitor {
  constructor() {
    this.fps = 0;
    this.frameCount = 0;
    this.lastTime = performance.now();
  }
  
  update() {
    this.frameCount++;
    const currentTime = performance.now();
    
    if (currentTime - this.lastTime >= 1000) {
      this.fps = Math.round((this.frameCount * 1000) / (currentTime - this.lastTime));
      this.frameCount = 0;
      this.lastTime = currentTime;
    }
    
    requestAnimationFrame(() => this.update());
  }
}
```

## Common Patterns

### Event Listener Management
```javascript
// Memory leak detector for event listeners
class EventManager {
  constructor() {
    this.listeners = new WeakMap();
  }
  
  addListener(element, event, handler) {
    element.addEventListener(event, handler);
    
    if (!this.listeners.has(element)) {
      this.listeners.set(element, []);
    }
    this.listeners.get(element).push({ event, handler });
  }
  
  removeListener(element, event, handler) {
    element.removeEventListener(event, handler);
    
    if (this.listeners.has(element)) {
      const listeners = this.listeners.get(element);
      const index = listeners.findIndex(l => l.event === event && l.handler === handler);
      if (index > -1) listeners.splice(index, 1);
    }
  }
  
  cleanup(element) {
    const listeners = this.listeners.get(element);
    if (listeners) {
      listeners.forEach(({ event, handler }) => {
        element.removeEventListener(event, handler);
      });
      this.listeners.delete(element);
    }
  }
}
```

### Rate Limiting
```javascript
// API rate limiter
class RateLimiter {
  constructor(maxCalls, timeWindow) {
    this.maxCalls = maxCalls;
    this.timeWindow = timeWindow;
    this.calls = [];
  }
  
  canMakeCall() {
    const now = Date.now();
    this.calls = this.calls.filter(time => now - time < this.timeWindow);
    return this.calls.length < this.maxCalls;
  }
  
  async makeCall(fn) {
    if (!this.canMakeCall()) {
      const waitTime = this.timeWindow - (Date.now() - Math.min(...this.calls));
      await new Promise(resolve => setTimeout(resolve, waitTime));
    }
    
    this.calls.push(Date.now());
    return fn();
  }
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Remove event listeners when elements are destroyed | Leave event listeners attached to removed elements |
| Clear timers and intervals when done | Let timers run indefinitely |
| Use WeakMap/WeakSet for temporary references | Store DOM references in regular objects |
| Throttle scroll/resize events | Handle every scroll/resize event |
| Use web workers for heavy computations | Block main thread with expensive operations |
| Implement lazy loading for large datasets | Load everything upfront |
| Profile performance in production-like conditions | Only test in development |

## Quick Fixes
- **Memory growing indefinitely** → Check for uncleaned event listeners and timers
- **Slow scrolling/resizing** → Implement throttling with 16ms delay (60fps)
- **Search input lag** → Add debouncing with 300ms delay
- **UI freezing during computation** → Move work to web worker
- **Slow initial load** → Implement code splitting and lazy loading
- **High memory usage** → Use object pooling and nullify references
- **Performance bottlenecks** → Use Performance API to measure and identify issues

## Gotchas
⚠️ **Closures retain all variables in scope** - Extract only needed values to avoid memory leaks
⚠️ **WeakMap keys must be objects** - Cannot use primitives as keys
⚠️ **Web workers have limited access** - No DOM manipulation, limited APIs available
⚠️ **Throttling vs debouncing confusion** - Throttle limits frequency, debounce delays execution
⚠️ **Performance.memory not available in all browsers** - Always check for availability
⚠️ **Transferable objects are moved, not copied** - Original becomes unusable after transfer

## Checklist
- [ ] Remove all event listeners when components unmount
- [ ] Clear timers and intervals in cleanup functions
- [ ] Nullify large object references when done
- [ ] Use throttling for frequent events (scroll, resize, mousemove)
- [ ] Use debouncing for user input (search, form validation)
- [ ] Implement lazy loading for images and components
- [ ] Move heavy computations to web workers
- [ ] Monitor memory usage in production
- [ ] Profile performance regularly with realistic data
- [ ] Use object pooling for frequently created objects