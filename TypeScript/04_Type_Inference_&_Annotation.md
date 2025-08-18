# Type Inference & Type Annotation `v TypeScript 5.3`

## Essentials

```typescript
// Type inference - TypeScript guesses the type
let name = "John";        // string (inferred)
let age = 25;            // number (inferred)
let items = [1, 2, 3];   // number[] (inferred)

// Type annotation - explicitly specify the type
let name: string = "John";
let age: number = 25;
let items: number[] = [1, 2, 3];
```

**Key Concepts**: Inference | Annotation | Narrowing
**When to use**: Let TypeScript infer simple types, annotate complex types and function parameters

## Syntax Reference

### Basic Structure

```typescript
// Implicit typing (inference)
let value = initialValue;  // Type inferred from value

// Explicit typing (annotation)
let value: Type = initialValue;  // Type explicitly declared
```

### Common Patterns

```typescript
// Pattern 1: Function parameters (always annotate)
function greet(name: string, age: number): string {
  return `Hello ${name}, you are ${age}`;
}

// Pattern 2: Complex object inference
const user = {
  id: 1,           // number (inferred)
  name: "Alice",   // string (inferred)
  active: true     // boolean (inferred)
};  // Type: { id: number; name: string; active: boolean }

// Pattern 3: Array inference with mixed types
const mixed = [1, "hello", true];  // (string | number | boolean)[]

// Pattern 4: Type narrowing with guards
function processValue(value: string | number) {
  if (typeof value === "string") {
    // value is narrowed to string here
    return value.toUpperCase();
  }
  // value is narrowed to number here
  return value * 2;
}

// Pattern 5: Return type inference
function calculate(x: number) {
  return x * 2;  // Return type inferred as number
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Let simple variables be inferred | Over-annotate obvious types |
| Annotate function parameters | Skip parameter types |
| Use type narrowing with guards | Ignore narrowing opportunities |
| Annotate return types for public APIs | Rely on inference for complex returns |
| Use `const` assertions when needed | Miss readonly inference benefits |

## Quick Fixes

- **Type 'string' is not assignable to type 'number'** → Check your type annotations or use union types
- **Parameter implicitly has 'any' type** → Add explicit type annotations to function parameters
- **Object is possibly 'undefined'** → Use type narrowing or optional chaining
- **Cannot invoke an object which is possibly 'undefined'** → Add type guards before function calls
- **Type narrowing not working** → Use discriminated unions or more specific type guards

## Gotchas

⚠️ **Empty arrays default to `any[]`** - Specify type explicitly: `const items: string[] = []`

⚠️ **`let` vs `const` affects inference** - `const` gives literal types, `let` gives broader types
```typescript
const status = "pending";  // Type: "pending" (literal)
let status = "pending";    // Type: string (broader)
```

⚠️ **Object method inference can be tricky** - Methods may need explicit return type annotations
```typescript
const api = {
  getData() {  // Return type inferred from implementation
    return fetch('/data');  // Promise<Response>
  }
};
```

⚠️ **Type narrowing resets in nested functions** - Guards don't carry over to callbacks
```typescript
function example(value: string | null) {
  if (value) {
    // value is string here
    setTimeout(() => {
      // value might be null here again!
      console.log(value.length);  // Error!
    }, 100);
  }
}
```

## Checklist

- [ ] Function parameters have explicit type annotations
- [ ] Complex return types are annotated for public APIs
- [ ] Simple variable types are left to inference
- [ ] Type guards are used for narrowing union types
- [ ] `const` assertions used where literal types needed
- [ ] Optional chaining (`?.`) used for potentially undefined values
- [ ] Discriminated unions used for complex type narrowing scenarios