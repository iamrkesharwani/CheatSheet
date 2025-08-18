# Introduction to TypeScript `v 5.3+`

## Essentials

```typescript
// Basic type annotations
let name: string = "Alice";
let age: number = 30;
let isActive: boolean = true;

// Function with types
function greet(name: string): string {
  return `Hello, ${name}!`;
}

// Interface for objects
interface User {
  id: number;
  name: string;
  email?: string; // Optional property
}
```

**Key Concepts**: Static Typing | Compile-time Checking | Type Inference | Interface Definition
**When to use**: Large codebases, team projects, catching errors early, better IDE support

## Syntax Reference

### Basic Structure

```typescript
// Variable declarations with explicit types
let username: string = "john_doe";
let count: number = 42;
let items: string[] = ["apple", "banana"];
let user: { name: string; age: number } = { name: "Alice", age: 25 };

// Type inference (TypeScript guesses the type)
let message = "Hello"; // inferred as string
let numbers = [1, 2, 3]; // inferred as number[]
```

### Common Patterns

```typescript
// Union types: multiple possible types
let id: string | number = "abc123";

// Array types: two equivalent syntaxes
let names: string[] = ["Alice", "Bob"];
let ages: Array<number> = [25, 30];

// Function types with parameters and return
function calculate(x: number, y: number): number {
  return x + y;
}

// Optional parameters with default values
function createUser(name: string, age: number = 18): User {
  return { id: Date.now(), name, age };
}

// Interface inheritance
interface Admin extends User {
  permissions: string[];
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use type inference when obvious | Over-annotate simple variables |
| Define interfaces for object shapes | Use `any` type without good reason |
| Enable strict mode in tsconfig.json | Ignore TypeScript compiler errors |
| Use meaningful interface names | Use generic names like `Data` or `Info` |

## Quick Fixes

- **"Cannot find name 'require'"** → Add `"moduleResolution": "node"` to tsconfig.json
- **"Object is possibly 'undefined'"** → Use optional chaining `obj?.property` or null check
- **"Argument of type 'X' is not assignable to type 'Y'"** → Check type definitions and ensure compatibility
- **Module not found errors** → Install @types packages: `npm install @types/node`

## Gotchas

⚠️ **Type assertions**: `value as Type` bypasses type checking - use sparingly and only when you're certain
⚠️ **Array mutation**: TypeScript allows pushing wrong types to arrays in some contexts - be explicit with array types
⚠️ **JSON parsing**: `JSON.parse()` returns `any` - always type the result or use type guards

## Setup & Compilation

### Installation & Setup

```bash
# Global installation
npm install -g typescript

# Project-specific installation
npm install -D typescript @types/node

# Initialize TypeScript config
tsc --init
```

### Essential tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Compilation Commands

```bash
# Compile once
tsc

# Watch mode (recompile on changes)
tsc --watch

# Compile specific file
tsc app.ts

# Run TypeScript directly (requires ts-node)
npx ts-node app.ts
```

## Checklist

- [ ] Install TypeScript globally or in project
- [ ] Create tsconfig.json with strict mode enabled
- [ ] Set up proper folder structure (src/, dist/)
- [ ] Install @types packages for Node.js and libraries
- [ ] Configure your editor for TypeScript support
- [ ] Test compilation with `tsc --noEmit` for type checking only