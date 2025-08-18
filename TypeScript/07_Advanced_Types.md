# TypeScript Advanced Types `v 5.3.3`

## Essentials

```typescript
// Union types - value can be one of several types
let id: string | number = "abc123";

// Intersection types - combine multiple types
type User = { name: string } & { age: number };

// Literal types - exact values as types
type Status = "loading" | "success" | "error";

// Type guards - runtime type checking
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

**Key Concepts**: Union (`|`) | Intersection (`&`) | Literal Values | Type Guards | Utility Types
**When to use**: Complex data modeling, API responses, form validation, and type-safe operations

## Syntax Reference

### Union Types

```typescript
// Basic union
type StringOrNumber = string | number;

// Function with union parameters
function formatId(id: string | number): string {
  return `ID: ${id}`;
}

// Union with objects
type Cat = { type: "cat"; meows: boolean };
type Dog = { type: "dog"; barks: boolean };
type Pet = Cat | Dog;
```

### Intersection Types

```typescript
// Combining object types
type Name = { firstName: string; lastName: string };
type Age = { age: number };
type Person = Name & Age;

// With interfaces
interface Flyable { fly(): void; }
interface Swimmable { swim(): void; }
type Duck = Flyable & Swimmable;
```

### Literal Types

```typescript
// String literals
type Theme = "light" | "dark" | "auto";
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";

// Numeric literals
type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;

// Boolean literals
type Success = true;
type SuccessOrFailure = true | false; // same as boolean
```

### Type Guards

```typescript
// typeof guard
function processValue(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase(); // TypeScript knows it's string
  }
  return value * 2; // TypeScript knows it's number
}

// in operator guard
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    animal.swim(); // TypeScript knows it's Fish
  } else {
    animal.fly(); // TypeScript knows it's Bird
  }
}

// instanceof guard
function handleError(error: Error | string) {
  if (error instanceof Error) {
    console.log(error.message); // TypeScript knows it's Error
  } else {
    console.log(error); // TypeScript knows it's string
  }
}

// Custom type guard
function isUser(obj: unknown): obj is { name: string; email: string } {
  return typeof obj === "object" && 
         obj !== null &&
         "name" in obj && 
         "email" in obj;
}
```

### Type Assertions

```typescript
// as keyword - when you know more than TypeScript
const canvas = document.getElementById("canvas") as HTMLCanvasElement;

// Angle bracket syntax (avoid in JSX)
const canvas2 = <HTMLCanvasElement>document.getElementById("canvas");

// Non-null assertion
const user = getUser()!; // Assert that getUser() is not null/undefined

// Double assertion (use sparingly)
const num = (someValue as unknown) as number;
```

### Utility Types

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

// keyof - get keys as union type
type UserKeys = keyof User; // "id" | "name" | "email" | "password"

// typeof - get type from value
const defaultUser = { name: "John", age: 30 };
type DefaultUser = typeof defaultUser; // { name: string; age: number }

// ReturnType - extract function return type
function getUser() { return { id: 1, name: "John" }; }
type UserType = ReturnType<typeof getUser>; // { id: number; name: string }

// Parameters - extract function parameter types
function createUser(name: string, age: number) {}
type CreateUserParams = Parameters<typeof createUser>; // [string, number]

// Partial - make all properties optional
type PartialUser = Partial<User>; // All properties become optional

// Pick - select specific properties
type UserProfile = Pick<User, "name" | "email">; // { name: string; email: string }

// Omit - exclude specific properties
type SafeUser = Omit<User, "password">; // User without password field
```

## Common Patterns

```typescript
// Discriminated unions with type guards
type ApiResponse<T> = 
  | { status: "success"; data: T }
  | { status: "error"; error: string };

function handleResponse<T>(response: ApiResponse<T>) {
  if (response.status === "success") {
    console.log(response.data); // TypeScript knows data exists
  } else {
    console.log(response.error); // TypeScript knows error exists
  }
}

// Conditional types with utility types
type NonNullable<T> = T extends null | undefined ? never : T;

// Mapped types with keyof
type ReadOnly<T> = {
  readonly [K in keyof T]: T[K];
};

// Template literal types
type EventName<T extends string> = `on${Capitalize<T>}`;
type ClickEvent = EventName<"click">; // "onClick"
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use union types for values that can be multiple types | Use `any` when you could use a union |
| Use type guards to narrow union types safely | Assume type without checking in unions |
| Use intersection for combining related types | Overuse intersection with conflicting properties |
| Use literal types for exact string/number values | Use broad types when literals would be clearer |
| Use utility types for DRY type definitions | Recreate common type transformations manually |
| Use `keyof` for type-safe property access | Use string literals for object keys |

## Quick Fixes

- **"Property doesn't exist on union type"** → Use type guards or `in` operator to narrow the type
- **"Type assertion error"** → Use double assertion via `unknown` or check if assertion is actually needed
- **"Cannot assign to union type"** → Ensure the value matches one of the union members exactly
- **"Argument not assignable to parameter"** → Check if you need to narrow the type with type guards
- **"keyof returns never"** → Make sure the type isn't empty or unknown

## Gotchas

⚠️ **Union vs Intersection**: `A | B` means "A OR B", `A & B` means "A AND B" - intersection combines properties
⚠️ **Type Guards**: Custom type guards must return `value is Type` to work properly with TypeScript's flow analysis
⚠️ **Type Assertions**: They don't perform runtime checks - they only tell TypeScript what you think the type is
⚠️ **Literal Types**: `let` variables widen to general types, use `const` or `as const` for literal types
⚠️ **Utility Types**: `Partial<T>` makes everything optional, which might not be what you want for updates

## Checklist

- [ ] Use union types instead of `any` for multiple possible types
- [ ] Implement proper type guards when working with union types
- [ ] Prefer utility types over manual type definitions
- [ ] Use literal types for constants and enums
- [ ] Add type assertions only when you're certain about the type
- [ ] Use `keyof` for type-safe property access patterns