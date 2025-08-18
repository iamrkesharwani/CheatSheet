# TypeScript Basic Types `v 5.4`

## Essentials

```typescript
// Primitive types
let name: string = "Alice";
let age: number = 30;
let isActive: boolean = true;

// Special types
let data: any = "anything goes";
let value: unknown = getData(); // safer than any
let result: void = console.log("hello"); // function returns nothing
let error: never = throw new Error("oops"); // function never returns
```

**Key Concepts**: Type Annotations | Type Safety | Primitive vs Special Types
**When to use**: Explicit typing for variables, function parameters, and return values

## Syntax Reference

### Basic Structure

```typescript
// Variable declarations with type annotations
let variableName: type = value;

// Function with typed parameters and return
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Optional and default parameters
function createUser(name: string, age?: number, active: boolean = true): void {
    // implementation
}
```

### Common Patterns

```typescript
// Pattern 1: Explicit typing for clarity
let userId: number = 123;
let userName: string = "john_doe";
let isLoggedIn: boolean = false;

// Pattern 2: Type inference (TypeScript guesses)
let count = 0; // inferred as number
let message = "Hello"; // inferred as string
let ready = true; // inferred as boolean

// Pattern 3: Union types with primitives
let id: string | number = "abc123";
let status: "pending" | "complete" | "error" = "pending";

// Pattern 4: Arrays with specific types
let numbers: number[] = [1, 2, 3];
let names: string[] = ["Alice", "Bob"];
let flags: boolean[] = [true, false, true];

// Pattern 5: Function return types
function getAge(): number { return 25; }
function getName(): string { return "Alice"; }
function logMessage(): void { console.log("logged"); }
function throwError(): never { throw new Error("boom"); }

// Pattern 6: Handling null and undefined
let nullable: string | null = null;
let optional: string | undefined = undefined;
let maybeString: string | null | undefined;
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `unknown` for truly unknown data | Use `any` unless absolutely necessary |
| Be explicit with function return types | Assume TypeScript will catch everything |
| Use `void` for functions with no return | Use `undefined` as return type |
| Use `never` for functions that throw/infinite loop | Use `never` for normal functions |
| Use union types for multiple possibilities | Create overly complex type unions |

## Quick Fixes

- **Type 'string' is not assignable to type 'number'** → Check your variable assignments and function calls
- **Object is possibly 'null' or 'undefined'** → Add null checks or use optional chaining (`?.`)
- **Function lacks ending return statement** → Add explicit return or change return type to `void`
- **Type 'any' is not allowed** → Replace `any` with specific types or `unknown`
- **Unreachable code detected** → Check for `never` type usage in wrong context

## Gotchas

⚠️ **Warning**: `any` disables all type checking - avoid unless migrating JavaScript code
⚠️ **Warning**: `unknown` requires type checking before use - safer than `any` but needs guards
⚠️ **Warning**: `void` functions can still return `undefined` implicitly
⚠️ **Warning**: `never` type indicates unreachable code - often points to logic errors
⚠️ **Warning**: `null` and `undefined` are different - `null` is intentional absence, `undefined` is uninitialized

## Checklist

- [ ] Use primitive types (`string`, `number`, `boolean`) for basic values
- [ ] Prefer `unknown` over `any` for truly unknown data
- [ ] Use `void` for functions that perform actions without returning values
- [ ] Use `never` only for functions that throw errors or have infinite loops
- [ ] Handle `null` and `undefined` explicitly in your type definitions
- [ ] Enable strict null checks in tsconfig.json for better safety
- [ ] Use type inference when types are obvious, explicit annotations when clarity is needed