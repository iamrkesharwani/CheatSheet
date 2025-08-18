# Object-Oriented JavaScript `v 0.07.25.01`

## Essentials
```javascript
// ES6 Classes
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
  
  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

// Inheritance
class Admin extends User {
  constructor(name, email, permissions) {
    super(name, email);
    this.permissions = permissions;
  }
}

// Private fields (ES2022)
class BankAccount {
  #balance = 0;
  
  deposit(amount) {
    this.#balance += amount;
  }
  
  getBalance() {
    return this.#balance;
  }
}
```

**Key Concepts**: Classes | Inheritance | Encapsulation | Polymorphism | Static methods | Mixins
**When to use**: Complex applications, code organization, data modeling, and reusable components

## Syntax Reference

### ES6 Classes
```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  // Instance method
  getArea() {
    return this.width * this.height;
  }
  
  // Static method
  static fromSquare(side) {
    return new Rectangle(side, side);
  }
  
  // Getter
  get perimeter() {
    return 2 * (this.width + this.height);
  }
  
  // Setter
  set dimensions({width, height}) {
    this.width = width;
    this.height = height;
  }
}

// Usage
const rect = new Rectangle(10, 5);
console.log(rect.getArea()); // 50
console.log(rect.perimeter); // 30
```

### Inheritance with extends
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }
  
  // Override parent method
  speak() {
    return `${this.name} barks`;
  }
  
  // New method
  wagTail() {
    return `${this.name} wags tail`;
  }
}

const dog = new Dog("Rex", "Golden Retriever");
console.log(dog.speak()); // "Rex barks"
console.log(dog instanceof Dog); // true
console.log(dog instanceof Animal); // true
```

### Private Fields & Methods
```javascript
class Counter {
  // Private field
  #count = 0;
  #maxValue;
  
  constructor(maxValue = 100) {
    this.#maxValue = maxValue;
  }
  
  // Private method
  #validate(value) {
    return value >= 0 && value <= this.#maxValue;
  }
  
  increment() {
    if (this.#validate(this.#count + 1)) {
      this.#count++;
    }
  }
  
  get value() {
    return this.#count;
  }
}

const counter = new Counter(10);
counter.increment();
console.log(counter.value); // 1
// console.log(counter.#count); // SyntaxError: Private field
```

## Common Patterns

### Constructor Functions (Legacy)
```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Add methods to prototype
Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

Person.prototype.isAdult = function() {
  return this.age >= 18;
};

// Inheritance with prototypes
function Employee(name, age, jobTitle) {
  Person.call(this, name, age); // Call parent constructor
  this.jobTitle = jobTitle;
}

// Set up prototype chain
Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;

Employee.prototype.work = function() {
  return `${this.name} is working as ${this.jobTitle}`;
};
```

### Static Methods & Properties
```javascript
class MathUtils {
  static PI = 3.14159;
  
  static add(a, b) {
    return a + b;
  }
  
  static circleArea(radius) {
    return this.PI * radius * radius;
  }
}

// Factory methods
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
  
  static createAdmin(name) {
    return new User(name, `${name.toLowerCase()}@admin.com`);
  }
  
  static createGuest() {
    return new User("Guest", "guest@example.com");
  }
}

const admin = User.createAdmin("John");
```

### Getters & Setters
```javascript
class Temperature {
  constructor(celsius = 0) {
    this._celsius = celsius;
  }
  
  get celsius() {
    return this._celsius;
  }
  
  set celsius(value) {
    if (value < -273.15) {
      throw new Error("Below absolute zero");
    }
    this._celsius = value;
  }
  
  get fahrenheit() {
    return (this._celsius * 9/5) + 32;
  }
  
  set fahrenheit(value) {
    this._celsius = (value - 32) * 5/9;
  }
}

const temp = new Temperature(25);
console.log(temp.fahrenheit); // 77
temp.fahrenheit = 100;
console.log(temp.celsius); // 37.78
```

### Mixins & Composition
```javascript
// Mixin objects
const Flyable = {
  fly() {
    return `${this.name} is flying`;
  }
};

const Swimmable = {
  swim() {
    return `${this.name} is swimming`;
  }
};

// Base class
class Bird {
  constructor(name) {
    this.name = name;
  }
}

// Apply mixins
Object.assign(Bird.prototype, Flyable);

class Duck extends Bird {
  constructor(name) {
    super(name);
  }
}

Object.assign(Duck.prototype, Swimmable);

// Mixin factory functions
const Timestampable = (Base) => class extends Base {
  constructor(...args) {
    super(...args);
    this.createdAt = new Date();
  }
  
  touch() {
    this.updatedAt = new Date();
  }
};

class Product extends Timestampable(Object) {
  constructor(name, price) {
    super();
    this.name = name;
    this.price = price;
  }
}
```

## Advanced Concepts

### Abstract Classes
```javascript
class Shape {
  constructor() {
    if (this.constructor === Shape) {
      throw new Error("Abstract class cannot be instantiated");
    }
  }
  
  // Abstract method
  getArea() {
    throw new Error("Abstract method must be implemented");
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  
  getArea() {
    return Math.PI * this.radius * this.radius;
  }
}
```

### Method Chaining
```javascript
class QueryBuilder {
  constructor() {
    this.query = {};
  }
  
  select(fields) {
    this.query.select = fields;
    return this; // Return this for chaining
  }
  
  where(condition) {
    this.query.where = condition;
    return this;
  }
  
  orderBy(field) {
    this.query.orderBy = field;
    return this;
  }
  
  build() {
    return this.query;
  }
}

// Chainable usage
const query = new QueryBuilder()
  .select(['name', 'email'])
  .where('active = true')
  .orderBy('name')
  .build();
```

### this Binding Solutions
```javascript
class EventHandler {
  constructor() {
    this.count = 0;
  }
  
  // Arrow function preserves 'this'
  handleClick = () => {
    this.count++;
    console.log(`Clicked ${this.count} times`);
  }
  
  // Regular method needs binding
  regularMethod() {
    this.count++;
  }
  
  setupEvents() {
    // Bind regular method
    const boundMethod = this.regularMethod.bind(this);
    
    // Or use arrow function wrapper
    const wrapper = () => this.regularMethod();
  }
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use ES6 classes over constructor functions | Mix constructor functions with classes |
| Use private fields for true encapsulation | Rely on naming conventions for privacy |
| Prefer composition over inheritance | Create deep inheritance hierarchies |
| Use static methods for utility functions | Put instance-specific logic in static methods |
| Use getters/setters for computed properties | Use getters/setters for simple data access |
| Use super() in constructors when extending | Forget to call super() in child constructors |

## Quick Fixes
- **"Class constructor cannot be invoked without 'new'"** → Always use `new` with classes
- **"Must call super constructor before accessing 'this'"** → Add `super()` call first
- **"Private field '#field' must be declared in an enclosing class"** → Check private field syntax
- **"Cannot read property of undefined"** → Check `this` binding in methods
- **"Class extends value undefined is not a constructor"** → Verify parent class import
- **"Duplicate declaration"** → Check for conflicting property/method names

## Gotchas
⚠️ **'this' in methods depends on call context** - Use arrow functions or bind() for consistent behavior
⚠️ **Private fields are truly private** - Cannot be accessed from outside class, even subclasses
⚠️ **Static methods don't have access to instance properties** - Use static properties instead
⚠️ **Getters/setters can cause infinite loops** - Don't reference the same property inside them
⚠️ **Class hoisting behaves differently** - Classes must be declared before use
⚠️ **Inheritance creates tight coupling** - Consider composition for better flexibility

## Legacy Patterns

### Pre-ES6 Privacy (WeakMap)
```javascript
const privateData = new WeakMap();

class SecureClass {
  constructor(secret) {
    privateData.set(this, { secret });
  }
  
  getSecret() {
    return privateData.get(this).secret;
  }
}
```

### Symbol-based Privacy
```javascript
const _secret = Symbol('secret');

class SymbolPrivate {
  constructor(secret) {
    this[_secret] = secret;
  }
  
  getSecret() {
    return this[_secret];
  }
}
```

### Prototype Manipulation
```javascript
// Check prototype chain
console.log(Object.getPrototypeOf(instance));
console.log(instance.constructor);
console.log(instance instanceof Constructor);

// Modify prototype at runtime
MyClass.prototype.newMethod = function() {
  return "Added after creation";
};
```

## Checklist
- [ ] Use ES6 classes for new code
- [ ] Implement proper inheritance with super()
- [ ] Use private fields for encapsulation
- [ ] Add input validation in setters
- [ ] Use static methods for utility functions
- [ ] Handle 'this' binding in event handlers
- [ ] Consider composition over inheritance
- [ ] Implement abstract methods where appropriate
- [ ] Use getters for computed properties
- [ ] Document public APIs and hide implementation details