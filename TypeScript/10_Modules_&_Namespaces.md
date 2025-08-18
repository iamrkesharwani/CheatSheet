# TypeScript Modules & Namespaces `v 5.3`

## Essentials

```typescript
// Named exports
export const userName = "John";
export function greet() { return "Hello"; }

// Default export
export default class User { name: string; }

// Import combinations
import User, { userName, greet } from './user';
import type { UserType } from './types';
```

**Key Concepts**: ES Modules | Named vs Default Exports | Type-only Imports | Module Resolution
**When to use**: Organizing code into reusable, maintainable modules with proper type safety

## Syntax Reference

### Basic Structure

```typescript
// user.ts - Module with mixed exports
export interface UserData {
  id: number;
  name: string;
}

export const DEFAULT_USER: UserData = { id: 0, name: "Guest" };

export function createUser(name: string): UserData {
  return { id: Math.random(), name };
}

export default class UserManager {
  users: UserData[] = [];
}
```

### Common Patterns

```typescript
// Pattern 1: Named exports (multiple exports)
export const config = { apiUrl: "https://api.example.com" };
export const utils = { formatDate: (d: Date) => d.toISOString() };

// Pattern 2: Default export (single main export)
export default function calculateTax(amount: number): number {
  return amount * 0.08;
}

// Pattern 3: Re-exports (barrel exports)
export { UserManager as default } from './UserManager';
export { createUser, type UserData } from './user';

// Pattern 4: Type-only imports/exports
export type { ApiResponse, ErrorType } from './api-types';
import type { DatabaseConfig } from './config';
```

## Import/Export Variations

### Named Exports

```typescript
// Exporting
export const PI = 3.14159;
export function multiply(a: number, b: number) { return a * b; }
export class Calculator { /* ... */ }

// Importing
import { PI, multiply, Calculator } from './math';
import { PI as CircleConstant } from './math'; // Alias
import * as MathUtils from './math'; // Namespace import
```

### Default Exports

```typescript
// Single default export
export default function logger(message: string) {
  console.log(`[LOG]: ${message}`);
}

// Import default (any name)
import log from './logger';
import writeLog from './logger'; // Same thing, different name
```

### Mixed Imports

```typescript
// Module with both default and named exports
export default class Database { /* ... */ }
export const connectionString = "mongodb://localhost";
export function connect(): Promise<void> { /* ... */ }

// Import both
import Database, { connectionString, connect } from './database';
```

### Type-only Imports

```typescript
// types.ts
export interface User { id: number; name: string; }
export type Status = 'active' | 'inactive';

// main.ts - Only import types, not runtime values
import type { User, Status } from './types';

let user: User = { id: 1, name: "Alice" };
let status: Status = 'active';
```

## Namespaces (Legacy)

```typescript
// Old TypeScript style - avoid in modern code
namespace Utilities {
  export function formatDate(date: Date): string {
    return date.toLocaleDateString();
  }
  
  export const version = "1.0.0";
}

// Usage
Utilities.formatDate(new Date());

// Modern equivalent using modules
export const Utilities = {
  formatDate: (date: Date) => date.toLocaleDateString(),
  version: "1.0.0"
};
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use named exports for utilities and constants | Mix default and named exports unnecessarily |
| Use default exports for main class/function | Use namespaces in new code |
| Use `import type` for type-only imports | Import entire modules when you need one thing |
| Create barrel exports for clean API | Create circular dependencies |
| Use descriptive import aliases | Use `import *` unless necessary |

## Quick Fixes

- **"Cannot find module"** → Check file paths and extensions (.ts, .js)
- **"Module has no default export"** → Use named import `{ name }` instead of default
- **"Module has no exported member"** → Check export spelling and availability
- **Circular dependency error** → Restructure imports or use dynamic imports
- **Type import at runtime** → Use `import type` for types only

## Gotchas

⚠️ **Default export naming**: Default imports can use any name, which can cause confusion across files

⚠️ **Type vs value imports**: Mixing type and value imports can cause bundling issues - use `import type` for types

⚠️ **File extensions**: TypeScript resolves modules without extensions, but bundlers might require them

⚠️ **Barrel exports performance**: Re-exporting everything can increase bundle size and slow tree-shaking

## Module Resolution

```typescript
// Relative imports
import { utils } from './utils';      // Same directory
import { config } from '../config';   // Parent directory
import { types } from './types/index'; // Folder with index file

// Absolute imports (with path mapping in tsconfig.json)
import { Button } from '@/components/Button';
import { api } from '@/services/api';

// Node modules
import express from 'express';
import { readFile } from 'fs/promises';
```

## Checklist

- [ ] Use ES modules (import/export) instead of namespaces
- [ ] Prefer named exports for multiple utilities
- [ ] Use default exports sparingly for main exports
- [ ] Import types with `import type` for better performance
- [ ] Avoid circular dependencies between modules
- [ ] Use barrel exports for clean public APIs
- [ ] Configure module resolution in tsconfig.json