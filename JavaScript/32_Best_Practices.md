# JavaScript Best Practices `v 0.07.25.01`

## Essentials
```javascript
// Clean, readable code with proper separation
class UserService {
  async fetchUser(id) {
    try {
      const response = await fetch(`/api/users/${id}`);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      this.handleError('Failed to fetch user', error);
      throw error;
    }
  }
}

// Small, focused functions
const formatCurrency = (amount, currency = 'USD') => 
  new Intl.NumberFormat('en-US', { style: 'currency', currency }).format(amount);

// Avoid globals - use modules
export { UserService, formatCurrency };
```

**Key Concepts**: Readability | Maintainability | Performance | Architecture | Error Handling
**When to use**: Every JavaScript project - these practices improve code quality and team productivity

---

## Readable, Maintainable Code

### Basic Structure
```javascript
// ✅ Use descriptive names
const calculateTotalPrice = (items, taxRate, discountPercent) => {
  const subtotal = items.reduce((sum, item) => sum + item.price, 0);
  const discount = subtotal * (discountPercent / 100);
  const taxableAmount = subtotal - discount;
  return taxableAmount * (1 + taxRate);
};

// ✅ Use const/let appropriately
const CONFIG = { maxRetries: 3, timeout: 5000 }; // Never changes
let currentUser = null; // Will be reassigned

// ✅ Consistent formatting and style
const userPreferences = {
  theme: 'dark',
  language: 'en',
  notifications: {
    email: true,
    push: false,
    sms: true
  }
};
```

### Common Patterns
```javascript
// Pattern 1: Destructuring for cleaner code
const { name, email, preferences: { theme } } = user;
const [first, second, ...rest] = items;

// Pattern 2: Template literals over concatenation
const message = `Hello ${name}, you have ${count} unread messages`;

// Pattern 3: Optional chaining and nullish coalescing
const userTheme = user?.preferences?.theme ?? 'light';
const displayName = user.displayName || user.name || 'Anonymous';

// Pattern 4: Early returns to reduce nesting
function processOrder(order) {
  if (!order) return null;
  if (!order.items.length) return { error: 'No items' };
  if (order.total <= 0) return { error: 'Invalid total' };
  
  return validateAndProcess(order);
}
```

---

## Small, Focused Functions

### Basic Structure
```javascript
// ✅ Single responsibility principle
const validateEmail = (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);

const sanitizeInput = (input) => input.trim().replace(/[<>]/g, '');

const createUser = (userData) => ({
  id: generateId(),
  ...userData,
  createdAt: new Date().toISOString(),
  isActive: true
});

// ✅ Pure functions - same input, same output
const calculateDiscount = (price, percentage) => price * (percentage / 100);

// ✅ Composable functions
const pipe = (...fns) => (value) => fns.reduce((acc, fn) => fn(acc), value);

const processUserInput = pipe(
  sanitizeInput,
  (input) => input.toLowerCase(),
  (input) => input.replace(/\s+/g, '-')
);
```

### Common Patterns
```javascript
// Pattern 1: Higher-order functions for reusability
const createValidator = (rules) => (data) => {
  const errors = [];
  Object.entries(rules).forEach(([field, rule]) => {
    if (!rule.test(data[field])) {
      errors.push(`${field}: ${rule.message}`);
    }
  });
  return { isValid: errors.length === 0, errors };
};

// Pattern 2: Factory functions
const createApiClient = (baseUrl, defaultHeaders = {}) => ({
  get: (endpoint) => fetch(`${baseUrl}${endpoint}`, { headers: defaultHeaders }),
  post: (endpoint, data) => fetch(`${baseUrl}${endpoint}`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', ...defaultHeaders },
    body: JSON.stringify(data)
  })
});

// Pattern 3: Currying for partial application
const createLogger = (level) => (message, data = {}) => {
  console[level](`[${level.toUpperCase()}] ${message}`, data);
};

const logger = {
  info: createLogger('info'),
  warn: createLogger('warn'),
  error: createLogger('error')
};
```

---

## Strategic Comments

### Basic Structure
```javascript
// ✅ Explain WHY, not WHAT
// Use exponential backoff to handle API rate limiting
const delay = Math.min(1000 * Math.pow(2, attempt), 30000);

// ✅ Document complex business rules
/**
 * Calculate shipping cost based on weight, distance, and service level.
 * Premium service adds 50% surcharge for orders under $100.
 * Free shipping applies to orders over $75 within 50 miles.
 */
function calculateShipping(weight, distance, total, isPremium) {
  if (total > 75 && distance <= 50) return 0;
  
  const baseRate = weight * 0.5 + distance * 0.1;
  return isPremium && total < 100 ? baseRate * 1.5 : baseRate;
}

// ✅ Warn about gotchas and edge cases
// NOTE: setTimeout IDs can be reused after clearTimeout
// Store reference to prevent accidental conflicts
let timeoutId = null;

// TODO: Replace with Web Workers for CPU-intensive operations
function processLargeDataset(data) {
  // Current implementation blocks UI thread
  return data.map(heavyComputation);
}
```

### Common Patterns
```javascript
// Pattern 1: API documentation comments
/**
 * Fetches user data with caching and retry logic
 * @param {string} userId - Unique user identifier
 * @param {Object} options - Configuration options
 * @param {boolean} options.useCache - Use cached data if available
 * @param {number} options.maxRetries - Maximum retry attempts
 * @returns {Promise<User>} User data object
 * @throws {NetworkError} When all retry attempts fail
 */
async function fetchUser(userId, { useCache = true, maxRetries = 3 } = {}) {
  // Implementation...
}

// Pattern 2: Inline comments for complex logic
function calculateTax(income, filingStatus) {
  // 2024 tax brackets for single filers
  const brackets = [
    { min: 0, max: 11000, rate: 0.10 },
    { min: 11000, max: 44725, rate: 0.12 },
    { min: 44725, max: 95375, rate: 0.22 }
  ];
  
  let tax = 0;
  let remainingIncome = income;
  
  for (const bracket of brackets) {
    if (remainingIncome <= 0) break;
    
    // Calculate tax for this bracket
    const taxableInBracket = Math.min(remainingIncome, bracket.max - bracket.min);
    tax += taxableInBracket * bracket.rate;
    remainingIncome -= taxableInBracket;
  }
  
  return tax;
}
```

---

## Avoid Global Variables

### Basic Structure
```javascript
// ❌ Avoid globals
// window.userData = {}; // Pollutes global scope

// ✅ Use modules and namespacing
const AppState = {
  user: null,
  theme: 'light',
  
  setUser(userData) {
    this.user = userData;
    this.notifySubscribers('user', userData);
  },
  
  subscribers: new Set(),
  
  subscribe(callback) {
    this.subscribers.add(callback);
    return () => this.subscribers.delete(callback);
  },
  
  notifySubscribers(key, value) {
    this.subscribers.forEach(callback => callback(key, value));
  }
};

// ✅ Use closure for private variables
const createCounter = () => {
  let count = 0; // Private variable
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getValue: () => count
  };
};
```

### Common Patterns
```javascript
// Pattern 1: Module pattern with IIFE
const UserModule = (() => {
  let users = []; // Private data
  
  return {
    addUser(user) {
      users.push({ ...user, id: Date.now() });
    },
    
    getUser(id) {
      return users.find(user => user.id === id);
    },
    
    getAllUsers() {
      return [...users]; // Return copy to prevent mutation
    }
  };
})();

// Pattern 2: ES6 modules
// userService.js
class UserService {
  #apiKey = process.env.API_KEY; // Private field
  #baseURL = 'https://api.example.com';
  
  async fetchUser(id) {
    const response = await fetch(`${this.#baseURL}/users/${id}`, {
      headers: { 'Authorization': `Bearer ${this.#apiKey}` }
    });
    return response.json();
  }
}

export default new UserService();

// Pattern 3: Configuration object
const CONFIG = Object.freeze({
  API: {
    BASE_URL: process.env.REACT_APP_API_URL || 'http://localhost:3000',
    TIMEOUT: 5000,
    RETRY_ATTEMPTS: 3
  },
  UI: {
    DEBOUNCE_DELAY: 300,
    ANIMATION_DURATION: 200
  }
});
```

---

## Separate Logic from UI

### Basic Structure
```javascript
// ✅ Business logic layer
class ShoppingCart {
  constructor() {
    this.items = [];
    this.observers = [];
  }
  
  addItem(product, quantity = 1) {
    const existingItem = this.items.find(item => item.id === product.id);
    
    if (existingItem) {
      existingItem.quantity += quantity;
    } else {
      this.items.push({ ...product, quantity });
    }
    
    this.notifyObservers();
  }
  
  getTotal() {
    return this.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  }
  
  subscribe(callback) {
    this.observers.push(callback);
  }
  
  notifyObservers() {
    this.observers.forEach(callback => callback(this.items));
  }
}

// ✅ UI layer - separate concerns
class CartUI {
  constructor(cart, container) {
    this.cart = cart;
    this.container = container;
    this.cart.subscribe(() => this.render());
  }
  
  render() {
    const items = this.cart.items;
    const total = this.cart.getTotal();
    
    this.container.innerHTML = `
      <div class="cart">
        ${items.map(item => this.renderItem(item)).join('')}
        <div class="total">Total: $${total.toFixed(2)}</div>
      </div>
    `;
  }
  
  renderItem(item) {
    return `
      <div class="cart-item">
        <span>${item.name} x ${item.quantity}</span>
        <span>$${(item.price * item.quantity).toFixed(2)}</span>
      </div>
    `;
  }
}
```

### Common Patterns
```javascript
// Pattern 1: MVC-like architecture
const Model = {
  data: {},
  
  set(key, value) {
    this.data[key] = value;
    this.notifyViews(key, value);
  },
  
  get(key) {
    return this.data[key];
  },
  
  views: [],
  
  addView(view) {
    this.views.push(view);
  },
  
  notifyViews(key, value) {
    this.views.forEach(view => view.update(key, value));
  }
};

const View = {
  update(key, value) {
    const element = document.querySelector(`[data-bind="${key}"]`);
    if (element) element.textContent = value;
  }
};

const Controller = {
  init() {
    Model.addView(View);
    this.bindEvents();
  },
  
  bindEvents() {
    document.addEventListener('click', (e) => {
      if (e.target.matches('[data-action]')) {
        const action = e.target.dataset.action;
        this[action] && this[action](e);
      }
    });
  },
  
  updateCounter() {
    const current = Model.get('counter') || 0;
    Model.set('counter', current + 1);
  }
};

// Pattern 2: Event-driven architecture
class EventBus {
  constructor() {
    this.events = {};
  }
  
  on(event, callback) {
    if (!this.events[event]) this.events[event] = [];
    this.events[event].push(callback);
  }
  
  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(data));
    }
  }
  
  off(event, callback) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(cb => cb !== callback);
    }
  }
}
```

---

## Linters Configuration

### ESLint Setup
```javascript
// .eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'prettier'
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 12,
    sourceType: 'module'
  },
  rules: {
    // Code quality
    'no-console': 'warn',
    'no-debugger': 'error',
    'no-unused-vars': 'error',
    
    // Best practices  
    'eqeqeq': 'error',
    'no-eval': 'error',
    'no-implied-eval': 'error',
    'no-new-func': 'error',
    
    // Style (handled by Prettier mostly)
    'prefer-const': 'error',
    'no-var': 'error',
    'prefer-arrow-callback': 'error',
    
    // Complexity
    'complexity': ['warn', 10],
    'max-depth': ['warn', 4],
    'max-lines-per-function': ['warn', { max: 50 }]
  }
};
```

### Prettier Configuration
```javascript
// .prettierrc.js
module.exports = {
  semi: true,
  trailingComma: 'es5',
  singleQuote: true,
  printWidth: 80,
  tabWidth: 2,
  useTabs: false,
  bracketSpacing: true,
  arrowParens: 'avoid',
  endOfLine: 'lf'
};

// package.json scripts
{
  "scripts": {
    "lint": "eslint src --ext .js,.ts,.jsx,.tsx",
    "lint:fix": "eslint src --ext .js,.ts,.jsx,.tsx --fix",
    "format": "prettier --write \"src/**/*.{js,ts,jsx,tsx,json,css,md}\"",
    "format:check": "prettier --check \"src/**/*.{js,ts,jsx,tsx,json,css,md}\""
  }
}
```

---

## Code Organization & Architecture

### Basic Structure
```javascript
// Project structure example
src/
├── components/          // UI components
│   ├── common/         // Reusable components
│   └── pages/          // Page-specific components
├── services/           // Business logic & API calls
├── utils/              // Pure utility functions
├── hooks/              // Custom React hooks (if applicable)
├── store/              // State management
├── types/              // TypeScript definitions
└── constants/          // App constants

// services/userService.js
class UserService {
  constructor(apiClient) {
    this.api = apiClient;
    this.cache = new Map();
  }
  
  async getUser(id) {
    if (this.cache.has(id)) {
      return this.cache.get(id);
    }
    
    const user = await this.api.get(`/users/${id}`);
    this.cache.set(id, user);
    return user;
  }
  
  async updateUser(id, updates) {
    const user = await this.api.put(`/users/${id}`, updates);
    this.cache.set(id, user); // Update cache
    return user;
  }
}

// utils/validation.js
export const validators = {
  email: (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
  phone: (value) => /^\+?[\d\s-()]+$/.test(value),
  required: (value) => value != null && value !== '',
  minLength: (min) => (value) => value && value.length >= min
};

export const validate = (data, rules) => {
  const errors = {};
  
  Object.entries(rules).forEach(([field, fieldRules]) => {
    const value = data[field];
    const fieldErrors = [];
    
    fieldRules.forEach(rule => {
      if (typeof rule === 'function' && !rule(value)) {
        fieldErrors.push(`Invalid ${field}`);
      }
    });
    
    if (fieldErrors.length > 0) {
      errors[field] = fieldErrors;
    }
  });
  
  return {
    isValid: Object.keys(errors).length === 0,
    errors
  };
};
```

### Common Patterns
```javascript
// Pattern 1: Dependency injection
class Application {
  constructor(dependencies) {
    this.userService = dependencies.userService;
    this.logger = dependencies.logger;
    this.eventBus = dependencies.eventBus;
  }
  
  async start() {
    this.logger.info('Application starting...');
    await this.userService.initialize();
    this.eventBus.emit('app:started');
  }
}

// Pattern 2: Plugin architecture
class PluginManager {
  constructor() {
    this.plugins = new Map();
  }
  
  register(name, plugin) {
    if (typeof plugin.init === 'function') {
      this.plugins.set(name, plugin);
      plugin.init();
    }
  }
  
  get(name) {
    return this.plugins.get(name);
  }
  
  executeHook(hookName, ...args) {
    this.plugins.forEach(plugin => {
      if (typeof plugin[hookName] === 'function') {
        plugin[hookName](...args);
      }
    });
  }
}
```

---

## Error Handling Best Practices

### Basic Structure
```javascript
// ✅ Custom error classes
class ValidationError extends Error {
  constructor(field, message) {
    super(`Validation failed for ${field}: ${message}`);
    this.name = 'ValidationError';
    this.field = field;
  }
}

class NetworkError extends Error {
  constructor(status, message) {
    super(`Network error ${status}: ${message}`);
    this.name = 'NetworkError';
    this.status = status;
  }
}

// ✅ Centralized error handling
class ErrorHandler {
  static handle(error, context = {}) {
    const errorInfo = {
      message: error.message,
      stack: error.stack,
      timestamp: new Date().toISOString(),
      context
    };
    
    // Log error
    console.error('Error occurred:', errorInfo);
    
    // Report to monitoring service
    if (process.env.NODE_ENV === 'production') {
      this.reportError(errorInfo);
    }
    
    // Show user-friendly message
    this.showUserError(error);
  }
  
  static showUserError(error) {
    let message = 'An unexpected error occurred';
    
    if (error instanceof ValidationError) {
      message = `Please check your ${error.field}`;
    } else if (error instanceof NetworkError) {
      message = 'Network connection problem. Please try again.';
    }
    
    // Show to user (toast, modal, etc.)
    this.notifyUser(message);
  }
}

// ✅ Graceful error handling in async functions
async function fetchUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    
    if (!response.ok) {
      throw new NetworkError(response.status, response.statusText);
    }
    
    const userData = await response.json();
    
    if (!userData.id) {
      throw new ValidationError('user', 'Invalid user data received');
    }
    
    return userData;
  } catch (error) {
    ErrorHandler.handle(error, { userId, operation: 'fetchUserData' });
    throw error; // Re-throw for caller to handle
  }
}
```

### Common Patterns
```javascript
// Pattern 1: Result pattern (instead of throwing)
const Result = {
  success: (data) => ({ success: true, data, error: null }),
  failure: (error) => ({ success: false, data: null, error })
};

async function safeApiCall(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      return Result.failure(`HTTP ${response.status}`);
    }
    const data = await response.json();
    return Result.success(data);
  } catch (error) {
    return Result.failure(error.message);
  }
}

// Usage
const result = await safeApiCall('/api/data');
if (result.success) {
  console.log(result.data);
} else {
  console.error(result.error);
}

// Pattern 2: Circuit breaker pattern
class CircuitBreaker {
  constructor(threshold = 5, timeout = 60000) {
    this.threshold = threshold;
    this.timeout = timeout;
    this.failureCount = 0;
    this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    this.nextAttempt = Date.now();
  }
  
  async execute(operation) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      }
      this.state = 'HALF_OPEN';
    }
    
    try {
      const result = await operation();
      this.onSuccess();
      return result;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }
  
  onSuccess() {
    this.failureCount = 0;
    this.state = 'CLOSED';
  }
  
  onFailure() {
    this.failureCount++;
    if (this.failureCount >= this.threshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.timeout;
    }
  }
}
```

---

## Performance Optimization

### Basic Structure
```javascript
// ✅ Debouncing and throttling
const debounce = (func, delay) => {
  let timeoutId;
  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
};

const throttle = (func, limit) => {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
};

// Usage
const handleSearch = debounce((query) => {
  // Expensive search operation
  searchAPI(query);
}, 300);

const handleScroll = throttle(() => {
  // Update UI based on scroll position
  updateScrollPosition();
}, 100);

// ✅ Lazy loading and code splitting
const LazyComponent = React.lazy(() => import('./HeavyComponent'));

// ✅ Memoization
const memoize = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};

const expensiveCalculation = memoize((n) => {
  console.log('Computing...');
  return n * n * n;
});

// ✅ Virtual scrolling for large lists
class VirtualList {
  constructor(container, items, itemHeight, visibleCount) {
    this.container = container;
    this.items = items;
    this.itemHeight = itemHeight;
    this.visibleCount = visibleCount;
    this.scrollTop = 0;
    
    this.render();
    this.bindEvents();
  }
  
  render() {
    const startIndex = Math.floor(this.scrollTop / this.itemHeight);
    const endIndex = Math.min(startIndex + this.visibleCount, this.items.length);
    
    const visibleItems = this.items.slice(startIndex, endIndex);
    
    this.container.innerHTML = `
      <div style="height: ${this.items.length * this.itemHeight}px; position: relative;">
        ${visibleItems.map((item, index) => `
          <div style="position: absolute; top: ${(startIndex + index) * this.itemHeight}px; height: ${this.itemHeight}px;">
            ${item}
          </div>
        `).join('')}
      </div>
    `;
  }
  
  bindEvents() {
    this.container.addEventListener('scroll', throttle(() => {
      this.scrollTop = this.container.scrollTop;
      this.render();
    }, 16)); // ~60fps
  }
}
```

### Common Patterns
```javascript
// Pattern 1: Object pooling
class ObjectPool {
  constructor(createFn, resetFn, initialSize = 10) {
    this.createFn = createFn;
    this.resetFn = resetFn;
    this.pool = [];
    
    // Pre-populate pool
    for (let i = 0; i < initialSize; i++) {
      this.pool.push(this.createFn());
    }
  }
  
  acquire() {
    return this.pool.length > 0 ? this.pool.pop() : this.createFn();
  }
  
  release(obj) {
    this.resetFn(obj);
    this.pool.push(obj);
  }
}

// Pattern 2: Batch processing
class BatchProcessor {
  constructor(processFn, batchSize = 100, delay = 10) {
    this.processFn = processFn;
    this.batchSize = batchSize;
    this.delay = delay;
    this.queue = [];
    this.processing = false;
  }
  
  add(item) {
    this.queue.push(item);
    if (!this.processing) {
      this.process();
    }
  }
  
  async process() {
    this.processing = true;
    
    while (this.queue.length > 0) {
      const batch = this.queue.splice(0, this.batchSize);
      await this.processFn(batch);
      
      // Yield control to prevent blocking
      if (this.queue.length > 0) {
        await new Promise(resolve => setTimeout(resolve, this.delay));
      }
    }
    
    this.processing = false;
  }
}

// Pattern 3: Lazy evaluation
class LazySequence {
  constructor(iterable) {
    this.iterable = iterable;
    this.operations = [];
  }
  
  map(fn) {
    this.operations.push({ type: 'map', fn });
    return this;
  }
  
  filter(fn) {
    this.operations.push({ type: 'filter', fn });
    return this;
  }
  
  take(count) {
    this.operations.push({ type: 'take', count });
    return this;
  }
  
  *[Symbol.iterator]() {
    let taken = 0;
    
    for (const item of this.iterable) {
      let current = item;
      let skip = false;
      
      for (const op of this.operations) {
        if (op.type === 'map') {
          current = op.fn(current);
        } else if (op.type === 'filter') {
          if (!op.fn(current)) {
            skip = true;
            break;
          }
        } else if (op.type === 'take') {
          if (taken >= op.count) return;
        }
      }
      
      if (!skip) {
        yield current;
        taken++;
      }
    }
  }
  
  toArray() {
    return [...this];
  }
}
```

---

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use meaningful variable and function names | Use abbreviations or single letters (except loops) |
| Keep functions under 20-30 lines | Create massive functions that do everything |
| Handle errors explicitly with try/catch | Let errors bubble up without handling |
| Use const by default, let when reassigning | Use var or create unnecessary globals |
| Separate business logic from UI rendering | Mix data manipulation with DOM updates |
| Write self-documenting code with good names | Over-comment obvious code |
| Use ESLint and Prettier consistently | Ignore linting rules or format inconsistently |
| Optimize only after measuring performance | Prematurely optimize without profiling |

---

## Quick Fixes

- **Function too complex** → Extract smaller functions, use early returns, reduce nesting
- **Global variable pollution** → Use modules, closures, or namespacing patterns
- **Performance issues** → Profile first, then optimize bottlenecks with memoization/caching
- **Hard to test code** → Separate pure functions from side effects, use dependency injection
- **Error swallowing** → Add proper error handling, logging, and user feedback
- **Memory leaks** → Clean up event listeners, clear intervals/timeouts, avoid circular references

---

## Gotchas

⚠️ **Hoisting**: Function declarations are hoisted, but not function expressions or arrow functions

⚠️ **Closures**: Inner functions capture variables by reference, not value. Use IIFE for loops

⚠️ This binding: Arrow functions inherit `this` from enclosing scope, regular functions create their own

⚠️ **Event listeners**: Always remove event listeners to prevent memory leaks, especially in SPAs

⚠️ **Async/await vs Promises**: Mixing them can cause unexpected behavior. Choose one approach per function

⚠️ **Equality operators**: `==` performs type coercion, `===` doesn't. Always prefer `===` unless you specifically need coercion

---

## Checklist

- [ ] Functions are under 30 lines and have single responsibility
- [ ] Variables use descriptive names (no abbreviations)
- [ ] Error handling covers all async operations
- [ ] No global variables polluting window object
- [ ] Business logic separated from UI rendering
- [ ] ESLint and Prettier configured and running
- [ ] Comments explain "why" not "what"
- [ ] Code is organized into logical modules/folders
- [ ] Performance bottlenecks identified and optimized
- [ ] Memory leaks prevented (event listeners, intervals cleared)
- [ ] All user inputs validated and sanitized
- [ ] API calls include proper error handling and loading states