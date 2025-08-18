# TypeScript Classes `v 5.3`

## Essentials

```typescript
class User {
  private id: number;
  public name: string;
  protected email: string;
  readonly createdAt: Date;

  constructor(name: string, email: string) {
    this.id = Math.random();
    this.name = name;
    this.email = email;
    this.createdAt = new Date();
  }

  public getName(): string {
    return this.name;
  }
}
```

**Key Concepts**: Access Modifiers | Constructor Properties | Inheritance | Interfaces
**When to use**: Object-oriented programming, data modeling, enforcing structure and encapsulation

## Syntax Reference

### Basic Structure

```typescript
class ClassName {
  property: type;                    // Public by default
  private _internal: type;           // Private to class
  protected inherited: type;         // Available to subclasses
  readonly constant: type;           // Cannot be reassigned
  static shared: type;              // Belongs to class, not instance

  constructor(param: type) {
    this.property = param;
  }

  method(): returnType {
    return this.property;
  }
}
```

### Common Patterns

```typescript
// Pattern 1: Constructor shorthand
class Product {
  constructor(
    public name: string,           // Auto-creates public property
    private price: number,         // Auto-creates private property
    readonly category: string      // Auto-creates readonly property
  ) {}
}

// Pattern 2: Getters and setters
class Temperature {
  private _celsius: number = 0;

  get fahrenheit(): number {
    return (this._celsius * 9/5) + 32;
  }

  set fahrenheit(value: number) {
    this._celsius = (value - 32) * 5/9;
  }
}

// Pattern 3: Interface implementation
interface Flyable {
  fly(): void;
  altitude: number;
}

class Bird implements Flyable {
  altitude: number = 0;
  
  fly(): void {
    this.altitude += 100;
  }
}

// Pattern 4: Abstract classes
abstract class Animal {
  abstract makeSound(): void;      // Must be implemented by subclasses
  
  move(): void {                   // Can be inherited as-is
    console.log("Moving...");
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Woof!");
  }
}

// Pattern 5: Static members
class MathUtils {
  static PI = 3.14159;
  
  static calculateArea(radius: number): number {
    return MathUtils.PI * radius * radius;
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `private` for internal state | Make everything public by default |
| Use constructor shorthand for simple properties | Repeat property declarations unnecessarily |
| Implement interfaces for contracts | Use inheritance for code reuse alone |
| Use `readonly` for immutable data | Mutate readonly properties indirectly |
| Use static for utility methods | Create instances just for static methods |

## Quick Fixes

- **Property not accessible** → Check access modifier (`private`, `protected`, `public`)
- **Cannot assign to readonly** → Initialize in constructor or at declaration
- **Abstract class instantiation** → Extend the abstract class, don't instantiate directly
- **Interface not implemented** → Add all required properties and methods
- **Static method context** → Use class name, not `this`, to access static members

## Gotchas

⚠️ **Access modifiers are compile-time only**: JavaScript output has no true privacy
⚠️ **`this` binding**: Arrow functions preserve `this` context, regular methods don't
⚠️ **Constructor execution order**: Parent constructor runs before child constructor body
⚠️ **Static inheritance**: Static members are inherited and can be overridden

## Checklist

- [ ] Choose appropriate access modifiers for encapsulation
- [ ] Use constructor parameter properties for cleaner code
- [ ] Implement required interface methods completely
- [ ] Mark utility methods as `static`
- [ ] Use `abstract` classes for shared behavior with required implementations
- [ ] Initialize `readonly` properties in constructor or at declaration