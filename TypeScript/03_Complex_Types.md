# TypeScript Complex Types `v 5.3`

## Essentials

```typescript
// Arrays - two syntaxes, same result
const names: string[] = ['Alice', 'Bob'];
const scores: Array<number> = [95, 87, 92];

// Tuples - fixed length, ordered types
const user: [string, number] = ['John', 25];

// Enums - named constants
enum Status { Pending, Active, Inactive }
const currentStatus = Status.Active; // 1

// Object types - define structure
const person: { name: string; age: number } = {
  name: 'Sarah',
  age: 30
};
```

**Key Concepts**: Arrays | Tuples | Enums | Object Types
**When to use**: When simple primitives aren't enough to describe your data structure

## Syntax Reference

### Array Types

```typescript
// Basic arrays
const fruits: string[] = ['apple', 'banana'];
const numbers: Array<number> = [1, 2, 3];

// Nested arrays
const matrix: number[][] = [[1, 2], [3, 4]];
const users: Array<Array<string>> = [['John', 'Doe'], ['Jane', 'Smith']];

// Mixed type arrays (union)
const mixed: (string | number)[] = ['hello', 42, 'world'];
```

### Tuple Types

```typescript
// Basic tuple - exact length and order
const coordinates: [number, number] = [10, 20];
const nameAge: [string, number] = ['Alice', 25];

// Optional tuple elements
const partial: [string, number?] = ['Bob']; // second element optional

// Rest in tuples
const scores: [string, ...number[]] = ['Math', 95, 87, 92];

// Named tuple elements (TypeScript 4.0+)
const point: [x: number, y: number] = [10, 20];
```

### Enum Types

```typescript
// Numeric enum (default - starts at 0)
enum Direction { Up, Down, Left, Right }
// Up = 0, Down = 1, Left = 2, Right = 3

// String enum
enum Color {
  Red = 'red',
  Green = 'green',
  Blue = 'blue'
}

// Mixed enum
enum Status {
  Loading = 'loading',
  Success = 200,
  Error = 500
}

// Const enum (compile-time optimization)
const enum HttpStatus {
  OK = 200,
  NotFound = 404
}
```

### Object Type Annotations

```typescript
// Inline object type
const user: { name: string; email: string; age?: number } = {
  name: 'John',
  email: 'john@example.com'
};

// Nested objects
const product: {
  id: number;
  details: {
    name: string;
    price: number;
  };
} = {
  id: 1,
  details: { name: 'Laptop', price: 999 }
};

// Index signatures for dynamic properties
const settings: { [key: string]: boolean } = {
  darkMode: true,
  notifications: false
};

// Methods in object types
const calculator: {
  add: (a: number, b: number) => number;
  multiply(a: number, b: number): number;
} = {
  add: (a, b) => a + b,
  multiply(a, b) { return a * b; }
};
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `string[]` for simple arrays | Use `Array<string>` unless generics needed |
| Use tuples for fixed, ordered data | Use tuples for variable-length lists |
| Use string enums for readability | Use numeric enums for public APIs |
| Define object shapes upfront | Use `any` instead of proper typing |
| Use optional properties `?` when appropriate | Make all properties required unnecessarily |

## Quick Fixes

- **"Type 'string' is not assignable to type 'number'"** → Check array/tuple type definitions
- **"Object is possibly 'undefined'"** → Add optional chaining `?.` or null checks
- **"Element implicitly has an 'any' type"** → Add explicit index signature to object type
- **"Enum member must have initializer"** → Provide explicit values for mixed enums
- **"Tuple type has no element at index"** → Check tuple length matches usage

## Gotchas

⚠️ **Array vs Tuple**: `[string, number]` is fixed length, `string[]` is variable length
⚠️ **Enum Values**: Numeric enums auto-increment, but gaps can cause confusion
⚠️ **Object Mutation**: TypeScript won't prevent runtime mutations of readonly-looking types
⚠️ **Tuple Destructuring**: Extra elements are ignored, missing ones are undefined

## Checklist

- [ ] Choose appropriate array syntax (`[]` vs `Array<>`)
- [ ] Use tuples only for fixed-length, ordered data
- [ ] Prefer string enums over numeric for better debugging
- [ ] Add optional properties (`?`) where data might be missing
- [ ] Consider readonly modifiers for immutable data
- [ ] Use index signatures for dynamic object properties
- [ ] Test edge cases like empty arrays and undefined object properties