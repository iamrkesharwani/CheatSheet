# TypeScript Error Handling & Safety `v 5.3`

## Essentials

```typescript
// Enable strict null checks in tsconfig.json
{
  "compilerOptions": {
    "strictNullChecks": true,
    "strict": true
  }
}

// Nullable types - explicit handling
let value: string | null = getValue();
if (value !== null) {
  console.log(value.toUpperCase()); // Safe to use
}

// Custom error with type safety
class ValidationError extends Error {
  constructor(public field: string, message: string) {
    super(message);
    this.name = 'ValidationError';
  }
}
```

**Key Concepts**: Nullable Types | Exhaustive Checking | Type Guards | Custom Errors
**When to use**: Prevent runtime errors and create robust, type-safe applications

## Syntax Reference

### Nullable Types & Strict Null Checks

```typescript
// Basic nullable types
let name: string | null = null;
let age: number | undefined = undefined;
let data: User | null | undefined;

// Optional chaining (safe navigation)
user?.profile?.email?.toLowerCase();

// Nullish coalescing (default values)
const displayName = user?.name ?? 'Anonymous';
const port = process.env.PORT ?? 3000;

// Type guards for null checking
function isNotNull<T>(value: T | null): value is T {
  return value !== null;
}

const validUsers = users.filter(isNotNull);
```

### Exhaustiveness Checking

```typescript
// Union types with exhaustive switch
type Status = 'pending' | 'approved' | 'rejected' | 'cancelled';

function handleStatus(status: Status): string {
  switch (status) {
    case 'pending':
      return 'Waiting for review';
    case 'approved':
      return 'Request approved';
    case 'rejected':
      return 'Request denied';
    case 'cancelled':
      return 'Request cancelled';
    default:
      // This will error if we miss a case
      const exhaustiveCheck: never = status;
      throw new Error(`Unhandled status: ${exhaustiveCheck}`);
  }
}

// Discriminated unions for complex scenarios
type ApiResponse = 
  | { success: true; data: User[] }
  | { success: false; error: string };

function processResponse(response: ApiResponse) {
  if (response.success) {
    // TypeScript knows this is success case
    response.data.forEach(user => console.log(user.name));
  } else {
    // TypeScript knows this is error case
    console.error(response.error);
  }
}
```

### Custom Error Classes

```typescript
// Base custom error
abstract class AppError extends Error {
  abstract readonly statusCode: number;
  abstract readonly isOperational: boolean;

  constructor(message: string, public readonly context?: Record<string, any>) {
    super(message);
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }
}

// Specific error types
class ValidationError extends AppError {
  readonly statusCode = 400;
  readonly isOperational = true;

  constructor(public readonly field: string, message: string) {
    super(`Validation failed for ${field}: ${message}`);
  }
}

class NotFoundError extends AppError {
  readonly statusCode = 404;
  readonly isOperational = true;

  constructor(resource: string, id: string | number) {
    super(`${resource} with id ${id} not found`);
  }
}

// Usage with type narrowing
function handleError(error: unknown): void {
  if (error instanceof ValidationError) {
    console.log(`Invalid ${error.field}: ${error.message}`);
  } else if (error instanceof NotFoundError) {
    console.log(`Resource not found: ${error.message}`);
  } else if (error instanceof Error) {
    console.log(`Unexpected error: ${error.message}`);
  } else {
    console.log('Unknown error occurred');
  }
}
```

## Common Patterns

```typescript
// Pattern 1: Result type for error handling
type Result<T, E = Error> = 
  | { success: true; data: T }
  | { success: false; error: E };

async function safeApiCall<T>(url: string): Promise<Result<T>> {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      return { success: false, error: new Error(`HTTP ${response.status}`) };
    }
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error as Error };
  }
}

// Pattern 2: Type predicate functions
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function isDefined<T>(value: T | undefined): value is T {
  return value !== undefined;
}

// Pattern 3: Assertion functions
function assertIsNumber(value: unknown): asserts value is number {
  if (typeof value !== 'number') {
    throw new TypeError(`Expected number, got ${typeof value}`);
  }
}

function assertNotNull<T>(value: T | null): asserts value is T {
  if (value === null) {
    throw new Error('Value cannot be null');
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `strictNullChecks` in production | Use `any` type to bypass errors |
| Handle all union type cases explicitly | Ignore TypeScript errors with `@ts-ignore` |
| Create specific error classes with context | Use generic `Error` for all failures |
| Use optional chaining `?.` for safe access | Use non-null assertion `!` without certainty |
| Validate external data at boundaries | Trust that API responses match your types |
| Use type guards for runtime type checking | Cast `unknown` to specific types directly |

## Quick Fixes

- **"Object is possibly null"** → Add null check: `if (obj !== null) { ... }`
- **"Property doesn't exist on type 'never'"** → Add missing case to switch statement
- **"Argument of type 'unknown' not assignable"** → Use type guard or assertion function
- **"Cannot invoke object which is possibly undefined"** → Use optional chaining: `func?.()`
- **"Type assertion may be unsafe"** → Replace with type guard or proper validation
- **"Switch statement should have default case"** → Add exhaustiveness check in default

## Gotchas

⚠️ **Non-null assertion (!) bypasses null checks**: Only use when you're 100% certain value exists
⚠️ **`as` assertions don't perform runtime checks**: They only change compile-time types
⚠️ **Optional properties vs null**: `{ name?: string }` allows undefined, not null
⚠️ **Error subclassing**: Always call `Error.captureStackTrace()` for proper stack traces
⚠️ **Exhaustiveness checking**: Add new union members carefully - missing cases won't error until you add the never check

## Checklist

- [ ] Enable `strictNullChecks` and `strict` mode in tsconfig.json
- [ ] Handle all nullable types explicitly with guards or optional chaining
- [ ] Add exhaustiveness checking to switch statements with union types
- [ ] Replace `any` with proper types or `unknown` + type guards
- [ ] Create custom error classes with meaningful context
- [ ] Use assertion functions instead of unsafe type assertions
- [ ] Validate external data at API boundaries
- [ ] Add proper error handling to async operations
- [ ] Use Result types or similar patterns for predictable error handling