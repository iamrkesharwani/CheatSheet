# TypeScript Generics `v 5.3`

## Essentials

```typescript
// Generic function - reusable with different types
function identity<T>(arg: T): T {
  return arg;
}

// Generic interface - flexible data structures
interface Container<T> {
  value: T;
  getValue(): T;
}

// Generic class - type-safe collections
class Stack<T> {
  private items: T[] = [];
  push(item: T): void { this.items.push(item); }
  pop(): T | undefined { return this.items.pop(); }
}
```

**Key Concepts**: Type Parameters | Type Safety | Code Reusability | Constraints
**When to use**: When you need type-safe code that works with multiple types without losing type information

## Syntax Reference

### Basic Structure

```typescript
// T is a type parameter - placeholder for actual type
function example<T>(param: T): T {
  return param;
}

// Multiple type parameters
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}

// Usage - TypeScript infers types
const num = example(42);        // T becomes number
const str = example("hello");   // T becomes string
const result = pair(1, "hi");   // T=number, U=string
```

### Common Patterns

```typescript
// Pattern 1: Generic Array Operations
function getFirst<T>(array: T[]): T | undefined {
  return array[0];
}

const firstNumber = getFirst([1, 2, 3]);     // number | undefined
const firstString = getFirst(["a", "b"]);   // string | undefined

// Pattern 2: Generic Promise Wrapper
async function fetchData<T>(url: string): Promise<T> {
  const response = await fetch(url);
  return response.json() as T;
}

interface User { id: number; name: string; }
const user = await fetchData<User>("/api/user/1");

// Pattern 3: Generic Utility Types
type Partial<T> = {
  [P in keyof T]?: T[P];
}

type Optional<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;
```

## Constraints & Advanced Usage

```typescript
// Constraint with extends - limit what T can be
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length); // Now we know T has length
  return arg;
}

// keyof constraint - T must be key of U
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Conditional types with constraints
type NonNullable<T> = T extends null | undefined ? never : T;

// Default type parameters
interface Repository<T = any> {
  save(entity: T): void;
  findById(id: string): T | null;
}

// Generic class with constraints
class DataProcessor<T extends { id: string }> {
  process(items: T[]): T[] {
    return items.filter(item => item.id.length > 0);
  }
}
```

## Do's & Don'ts

| ✅ Do                                    | ❌ Don't                               |
| ---------------------------------------- | -------------------------------------- |
| Use meaningful type parameter names      | Use single letters beyond T, U, V     |
| Add constraints when you need properties | Make everything generic unnecessarily  |
| Let TypeScript infer types when possible | Over-specify types in function calls   |
| Use default type parameters sensibly     | Create overly complex generic chains   |

## Quick Fixes

- **"Type 'T' is not assignable"** → Add proper constraints with `extends`
- **"Cannot find name 'T'"** → Declare type parameter in angle brackets `<T>`
- **"Type instantiation is excessively deep"** → Simplify generic constraints or add base cases
- **"Generic type requires type arguments"** → Provide default type parameters or explicit types

## Gotchas

⚠️ **Generic constraints are not type guards**: `T extends string` doesn't narrow T to string in the function body
⚠️ **Type erasure at runtime**: Generics only exist at compile time, can't use `instanceof T`
⚠️ **Inference limitations**: Complex generic functions may need explicit type arguments
⚠️ **Circular constraints**: Avoid `T extends U, U extends T` patterns

## Real-World Examples

```typescript
// API Response Wrapper
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

async function apiCall<T>(endpoint: string): Promise<ApiResponse<T>> {
  // Implementation
}

// Event System
class EventEmitter<T extends Record<string, any[]>> {
  on<K extends keyof T>(event: K, listener: (...args: T[K]) => void): void {
    // Type-safe event handling
  }
  
  emit<K extends keyof T>(event: K, ...args: T[K]): void {
    // Type-safe event emission
  }
}

// Form Validation
type ValidationResult<T> = {
  [K in keyof T]: string | null;
};

function validateForm<T extends Record<string, any>>(
  data: T,
  rules: { [K in keyof T]?: (value: T[K]) => string | null }
): ValidationResult<T> {
  // Implementation
}
```

## Checklist

- [ ] Understand when to use generics vs `any` or `unknown`
- [ ] Know how to add constraints with `extends` keyword
- [ ] Practice with generic functions, interfaces, and classes
- [ ] Learn built-in utility types (`Partial`, `Pick`, `Omit`, etc.)
- [ ] Understand type inference and when to provide explicit types