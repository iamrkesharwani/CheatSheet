# JavaScript Design Patterns & Principles `v 0.07.25.01`

## Essentials
```javascript
// DRY, KISS, YAGNI principles
const utils = {
  calculate: (operation, ...args) => operations[operation](...args)
};

// Module Pattern
const MyModule = (function() {
  let privateVar = 0;
  return {
    publicMethod: () => ++privateVar,
    getCount: () => privateVar
  };
})();

// Observer Pattern
class Subject {
  constructor() { this.observers = []; }
  notify(data) { this.observers.forEach(obs => obs.update(data)); }
}

// Strategy Pattern
class Context {
  setStrategy(strategy) { this.strategy = strategy; }
  execute(data) { return this.strategy.process(data); }
}
```

**Key Concepts**: SOLID Principles | Creational Patterns | Behavioral Patterns | Structural Patterns | Code Organization
**When to use**: Building maintainable, scalable applications with clean, reusable code architecture

## Core Principles

### DRY, KISS, YAGNI
```javascript
// DRY (Don't Repeat Yourself) - Eliminate duplication
const ShapeCalculator = {
  PI: 3.14159,
  calculateArea(shape, ...dimensions) {
    const formulas = {
      square: (side) => side * side,
      rectangle: (length, width) => length * width,
      circle: (radius) => this.PI * radius * radius
    };
    return formulas[shape](...dimensions);
  }
};

// KISS (Keep It Simple, Stupid) - Simple solutions
const isEven = (num) => num % 2 === 0; // Not: num.toString(2).slice(-1) === '0'

// YAGNI (You Aren't Gonna Need It) - Build only what's needed
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
    // Don't add: avatar, preferences, friends[] until actually needed
  }
}
```

## Creational Patterns

### Module Pattern
```javascript
// Basic Module with IIFE
const CounterModule = (function() {
  let count = 0;
  let listeners = [];
  
  function notifyListeners() {
    listeners.forEach(fn => fn(count));
  }
  
  return {
    increment() {
      count++;
      notifyListeners();
      return count;
    },
    
    decrement() {
      count--;
      notifyListeners();
      return count;
    },
    
    getValue() {
      return count;
    },
    
    subscribe(callback) {
      listeners.push(callback);
    }
  };
})();

// Revealing Module Pattern
const ApiModule = (function() {
  const baseUrl = 'https://api.example.com';
  let authToken = null;
  
  function makeRequest(endpoint, options) {
    return fetch(`${baseUrl}${endpoint}`, {
      ...options,
      headers: {
        'Authorization': `Bearer ${authToken}`,
        ...options.headers
      }
    });
  }
  
  function setAuth(token) {
    authToken = token;
  }
  
  function get(endpoint) {
    return makeRequest(endpoint, { method: 'GET' });
  }
  
  function post(endpoint, data) {
    return makeRequest(endpoint, {
      method: 'POST',
      body: JSON.stringify(data),
      headers: { 'Content-Type': 'application/json' }
    });
  }
  
  // Reveal public methods
  return {
    authenticate: setAuth,
    get: get,
    post: post
  };
})();
```

### Singleton Pattern
```javascript
// Class-based Singleton
class DatabaseConnection {
  constructor() {
    if (DatabaseConnection.instance) {
      return DatabaseConnection.instance;
    }
    
    this.connection = 'Connected';
    this.queries = [];
    DatabaseConnection.instance = this;
  }
  
  query(sql) {
    this.queries.push(sql);
    return { success: true, data: [] };
  }
  
  getHistory() {
    return [...this.queries];
  }
}

// Module Singleton with lazy initialization
const ConfigManager = (function() {
  let instance;
  
  function createInstance(config) {
    return {
      settings: config || {},
      get(key) { return this.settings[key]; },
      set(key, value) { this.settings[key] = value; },
      getAll() { return {...this.settings}; }
    };
  }
  
  return {
    getInstance(config) {
      if (!instance) {
        instance = createInstance(config);
      }
      return instance;
    }
  };
})();
```

### Factory Pattern
```javascript
// Simple Factory
function createUser(type, userData) {
  const userTypes = {
    admin: () => ({ ...userData, role: 'admin', permissions: ['all'] }),
    user: () => ({ ...userData, role: 'user', permissions: ['read'] }),
    guest: () => ({ ...userData, role: 'guest', permissions: [] })
  };
  
  return userTypes[type] ? userTypes[type]() : null;
}

// Class Factory
class VehicleFactory {
  static create(type, specs) {
    const vehicles = {
      car: () => new Car(specs),
      truck: () => new Truck(specs),
      motorcycle: () => new Motorcycle(specs)
    };
    
    return vehicles[type]?.() || null;
  }
}

class Vehicle {
  constructor(specs) {
    this.make = specs.make;
    this.model = specs.model;
  }
  start() { console.log(`${this.make} ${this.model} starting...`); }
}

class Car extends Vehicle {
  constructor(specs) {
    super(specs);
    this.doors = specs.doors || 4;
  }
  openTrunk() { console.log('Trunk opened'); }
}
```

## Behavioral Patterns

### Observer Pattern
```javascript
// Basic Observer
class EventEmitter {
  constructor() {
    this.events = {};
  }
  
  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
  }
  
  off(event, callback) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(cb => cb !== callback);
    }
  }
  
  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(data));
    }
  }
  
  once(event, callback) {
    const onceCallback = (data) => {
      callback(data);
      this.off(event, onceCallback);
    };
    this.on(event, onceCallback);
  }
}

// News Publisher Example
class NewsPublisher extends EventEmitter {
  constructor() {
    super();
    this.articles = [];
  }
  
  publishArticle(article) {
    this.articles.push(article);
    this.emit('newArticle', article);
    this.emit('articleCount', this.articles.length);
  }
}

// Usage
const publisher = new NewsPublisher();
publisher.on('newArticle', (article) => {
  console.log(`New article: ${article.title}`);
});
```

### Strategy Pattern
```javascript
// Payment Processing Strategy
class PaymentProcessor {
  constructor() {
    this.strategy = null;
  }
  
  setStrategy(strategy) {
    this.strategy = strategy;
  }
  
  processPayment(amount) {
    if (!this.strategy) {
      throw new Error('No payment strategy set');
    }
    return this.strategy.pay(amount);
  }
}

// Payment Strategies
class CreditCardPayment {
  constructor(cardNumber) {
    this.cardNumber = cardNumber;
  }
  
  pay(amount) {
    return {
      success: true,
      method: 'Credit Card',
      amount: amount,
      transactionId: `CC_${Date.now()}`
    };
  }
}

class PayPalPayment {
  constructor(email) {
    this.email = email;
  }
  
  pay(amount) {
    return {
      success: true,
      method: 'PayPal',
      amount: amount,
      transactionId: `PP_${Date.now()}`
    };
  }
}

// Validation Strategy
class ValidationContext {
  constructor() {
    this.strategies = [];
  }
  
  addStrategy(strategy) {
    this.strategies.push(strategy);
  }
  
  validate(data) {
    const errors = [];
    
    for (const strategy of this.strategies) {
      const result = strategy.validate(data);
      if (!result.isValid) {
        errors.push(result.error);
      }
    }
    
    return {
      isValid: errors.length === 0,
      errors: errors
    };
  }
}

class EmailValidation {
  validate(email) {
    const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    return {
      isValid,
      error: isValid ? null : 'Invalid email format'
    };
  }
}
```

## Event Patterns

### Event Delegation
```javascript
// Basic Event Delegation
class EventDelegator {
  constructor(container) {
    this.container = container;
    this.setupDelegation();
  }
  
  setupDelegation() {
    this.container.addEventListener('click', (e) => {
      // Button clicks
      if (e.target.matches('.btn')) {
        this.handleButtonClick(e.target);
      }
      
      // Link clicks
      if (e.target.matches('a[data-action]')) {
        e.preventDefault();
        this.handleLinkAction(e.target.dataset.action);
      }
    });
    
    this.container.addEventListener('change', (e) => {
      // Checkbox changes
      if (e.target.matches('input[type="checkbox"]')) {
        this.handleCheckboxChange(e.target);
      }
    });
  }
  
  handleButtonClick(button) {
    const action = button.dataset.action;
    console.log(`Button action: ${action}`);
  }
  
  handleLinkAction(action) {
    console.log(`Link action: ${action}`);
  }
}

// Dynamic List with Event Delegation
class TodoList {
  constructor(selector) {
    this.container = document.querySelector(selector);
    this.todos = [];
    this.setupEvents();
  }
  
  setupEvents() {
    this.container.addEventListener('click', (e) => {
      const todoId = e.target.dataset.id;
      
      if (e.target.matches('.delete-btn')) {
        this.deleteTodo(todoId);
      } else if (e.target.matches('.toggle-btn')) {
        this.toggleTodo(todoId);
      }
    });
  }
  
  addTodo(text) {
    const todo = {
      id: Date.now(),
      text: text,
      completed: false
    };
    this.todos.push(todo);
    this.render();
  }
  
  render() {
    this.container.innerHTML = this.todos.map(todo => `
      <div class="todo-item">
        <span class="${todo.completed ? 'completed' : ''}">${todo.text}</span>
        <button class="toggle-btn" data-id="${todo.id}">Toggle</button>
        <button class="delete-btn" data-id="${todo.id}">Delete</button>
      </div>
    `).join('');
  }
}
```

### Callback Pattern
```javascript
// Higher-Order Functions with Callbacks
function processData(data, onSuccess, onError) {
  setTimeout(() => {
    try {
      const processed = data.map(item => item * 2);
      onSuccess(processed);
    } catch (error) {
      onError(error);
    }
  }, 1000);
}

// Callback with configuration
function createProcessor(config) {
  return function(data, callback) {
    const processed = data
      .filter(config.filter || (() => true))
      .map(config.transform || (x => x))
      .slice(0, config.limit || data.length);
    
    callback(null, processed);
  };
}

// Usage
const doubleProcessor = createProcessor({
  transform: x => x * 2,
  filter: x => x > 0,
  limit: 10
});

doubleProcessor([1, -2, 3, 4], (err, result) => {
  console.log(result); // [2, 6, 8]
});
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use modules to encapsulate functionality | Create global variables unnecessarily |
| Implement single responsibility principle | Mix multiple concerns in one function |
| Prefer composition over inheritance | Create deep inheritance chains |
| Use meaningful names for patterns | Use patterns just to use them |
| Handle errors in callbacks properly | Ignore error handling in async operations |
| Keep strategies focused and simple | Create overly complex strategy implementations |

## Quick Fixes
- **"Cannot read property of undefined"** → Add null checks before accessing object properties
- **Memory leaks in observers** → Always remove event listeners and unsubscribe observers
- **Callback hell** → Use named functions or switch to Promises/async-await
- **Singleton not working** → Ensure instance check happens before creating new object
- **Factory returning wrong type** → Validate input parameters and provide default cases
- **Strategy pattern errors** → Always check if strategy is set before execution

## Gotchas
⚠️ **Module pattern closure**: Variables persist between calls, can cause memory leaks
⚠️ **Singleton timing**: Multiple async instantiations can create multiple instances
⚠️ **Observer memory leaks**: Always clean up event listeners to prevent memory leaks
⚠️ **Strategy pattern nulls**: Always validate strategy exists before calling methods
⚠️ **Factory validation**: Invalid types can return null/undefined, handle gracefully
⚠️ **Event delegation specificity**: CSS selectors must be specific enough to avoid conflicts

## Real-World Applications

### Shopping Cart with Observer
```javascript
class ShoppingCart extends EventEmitter {
  constructor() {
    super();
    this.items = [];
    this.total = 0;
  }
  
  addItem(item) {
    this.items.push(item);
    this.updateTotal();
    this.emit('itemAdded', item);
    this.emit('cartUpdated', this.getState());
  }
  
  removeItem(itemId) {
    this.items = this.items.filter(item => item.id !== itemId);
    this.updateTotal();
    this.emit('itemRemoved', itemId);
    this.emit('cartUpdated', this.getState());
  }
  
  updateTotal() {
    this.total = this.items.reduce((sum, item) => sum + item.price, 0);
  }
  
  getState() {
    return {
      items: [...this.items],
      total: this.total,
      count: this.items.length
    };
  }
}

// Usage
const cart = new ShoppingCart();
cart.on('itemAdded', (item) => console.log(`Added: ${item.name}`));
cart.on('cartUpdated', (state) => updateCartUI(state));
```

### Form Validation with Strategy
```javascript
class FormValidator {
  constructor() {
    this.strategies = new Map();
  }
  
  addRule(field, strategy) {
    if (!this.strategies.has(field)) {
      this.strategies.set(field, []);
    }
    this.strategies.get(field).push(strategy);
  }
  
  validate(formData) {
    const errors = {};
    
    for (const [field, strategies] of this.strategies) {
      const fieldErrors = [];
      
      for (const strategy of strategies) {
        const result = strategy.validate(formData[field]);
        if (!result.isValid) {
          fieldErrors.push(result.message);
        }
      }
      
      if (fieldErrors.length > 0) {
        errors[field] = fieldErrors;
      }
    }
    
    return {
      isValid: Object.keys(errors).length === 0,
      errors: errors
    };
  }
}

// Validation strategies
class RequiredValidator {
  validate(value) {
    return {
      isValid: value != null && value !== '',
      message: 'This field is required'
    };
  }
}

class EmailValidator {
  validate(value) {
    const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
    return {
      isValid,
      message: 'Please enter a valid email address'
    };
  }
}
```

## Checklist
- [ ] Applied DRY principle to eliminate code duplication
- [ ] Used appropriate design patterns for the problem context
- [ ] Implemented proper error handling in all patterns
- [ ] Set up event cleanup to prevent memory leaks
- [ ] Validated inputs in factory and strategy patterns
- [ ] Used meaningful names for pattern implementations
- [ ] Tested patterns with various scenarios and edge cases