# TypeScript Functions `v 5.5.0`

## Essentials

```typescript
// Basic function with type annotations
function greet(name: string): string {
  return `Hello, ${name}!`;
}

// Arrow function with types
const add = (a: number, b: number): number => a + b;

// Optional parameters
function createUser(name: string, age?: number): User {
  return { name, age: age ?? 18 };
}

// Default parameters
function fetchData(url: string, timeout: number = 5000): Promise<Data> {
  // implementation
}
```

**Key Concepts**: Type Annotations | Optional Parameters | Function Overloading | Rest Parameters
**When to use**: Define clear contracts for function inputs/outputs and enable better IDE support

## Syntax Reference

### Basic Structure

```typescript
// Function declaration with return type
function functionName(param: Type): ReturnType {
  return value;
}

// Arrow function syntax
const functionName = (param: Type): ReturnType => {
  return value;
};

// Shorthand arrow function
const functionName = (param: Type): ReturnType => value;
```

### Parameter Types

```typescript
// Required parameters
function process(data: string, count: number): void { }

// Optional parameters (must come after required)
function log(message: string, level?: 'info' | 'warn' | 'error'): void { }

// Default parameters
function connect(host: string, port: number = 3000): Connection { }

// Rest parameters
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}

// Destructured parameters
function updateUser({ id, name, email }: { id: number; name: string; email: string }): void { }
```

### Function Overloading

```typescript
// Overload signatures
function format(value: string): string;
function format(value: number): string;
function format(value: Date): string;

// Implementation signature
function format(value: string | number | Date): string {
  if (typeof value === 'string') return value.toUpperCase();
  if (typeof value === 'number') return value.toFixed(2);
  return value.toISOString();
}

// Usage
const str = format("hello");     // string
const num = format(42);          // string
const date = format(new Date()); // string
```

### Advanced Arrow Functions

```typescript
// Generic arrow function
const identity = <T>(value: T): T => value;

// Arrow function with object return (parentheses required)
const createPoint = (x: number, y: number) => ({ x, y });

// Async arrow function
const fetchUser = async (id: number): Promise<User> => {
  const response = await fetch(`/users/${id}`);
  return response.json();
};

// Higher-order function
const createMultiplier = (factor: number) => (value: number): number => value * factor;
const double = createMultiplier(2);
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Always annotate parameters | Rely on implicit `any` types |
| Use optional parameters (`?`) for truly optional values | Use `undefined` union types unnecessarily |
| Place optional parameters after required ones | Mix optional and required parameter order |
| Use rest parameters for variable arguments | Use `arguments` object in TypeScript |
| Prefer arrow functions for callbacks | Use `function` for simple expressions |
| Use function overloading for different input types | Create separate functions with similar names |

## Quick Fixes

- **"Parameter implicitly has 'any' type"** → Add explicit type annotation: `param: string`
- **"Cannot invoke without all required parameters"** → Make parameter optional with `?` or provide default value
- **"Argument of type 'X' not assignable to parameter"** → Check parameter type or use type assertion
- **"Function lacks ending return statement"** → Add return statement or change return type to `void`
- **"Overload signature not compatible"** → Ensure implementation signature handles all overload cases

## Gotchas

⚠️ **Optional parameters must come after required parameters** - TypeScript enforces this order
⚠️ **Function overloads need implementation signature** - The implementation must handle all overload cases
⚠️ **Arrow functions don't have `this` binding** - Use regular functions when you need dynamic `this`
⚠️ **Rest parameters create arrays** - `...args: string[]` means `args` is always an array
⚠️ **Default parameter values are evaluated at runtime** - Avoid expensive computations in defaults

## Checklist

- [ ] All parameters have explicit type annotations
- [ ] Return type is specified (unless void or easily inferred)
- [ ] Optional parameters come after required ones
- [ ] Function overloads cover all expected use cases
- [ ] Default values are appropriate for the parameter type
- [ ] Rest parameters use correct array typing
- [ ] Arrow vs regular function choice matches use case