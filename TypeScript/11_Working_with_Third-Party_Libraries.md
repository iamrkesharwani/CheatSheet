# Working with Third-Party Libraries `v 5.3.0`

## Essentials

```typescript
// Install types from DefinitelyTyped
npm install --save-dev @types/lodash

// Use library with types
import _ from 'lodash';
const result: string[] = _.map(['a', 'b'], (x) => x.toUpperCase());

// Declare module for untyped libraries
declare module 'my-untyped-lib' {
  export function doSomething(input: string): number;
}
```

**Key Concepts**: DefinitelyTyped | Module Declarations | Global Types
**When to use**: Adding TypeScript support to JavaScript libraries and packages

## Syntax Reference

### Installing Types with DefinitelyTyped

```typescript
// Install types for popular libraries
npm install --save-dev @types/node
npm install --save-dev @types/express
npm install --save-dev @types/lodash
npm install --save-dev @types/react

// Check if types exist
npm search @types/library-name

// Import and use with full type support
import express from 'express';
import _ from 'lodash';
const app = express(); // Fully typed
```

### Declaring Modules Without Types

```typescript
// Basic module declaration
declare module 'untyped-library' {
  export function someFunction(param: string): number;
  export const someConstant: string;
}

// Module with default export
declare module 'default-export-lib' {
  interface Config {
    apiKey: string;
    timeout: number;
  }
  
  function initialize(config: Config): void;
  export = initialize;
}

// Module with mixed exports
declare module 'mixed-exports' {
  export default function main(input: string): void;
  export function helper(data: any[]): string;
  export const VERSION: string;
}
```

### Global Type Declarations

```typescript
// Create types.d.ts in your project root
declare global {
  interface Window {
    myGlobalFunction: (input: string) => void;
    APP_CONFIG: {
      apiUrl: string;
      version: string;
    };
  }
  
  var myGlobalVar: string;
}

// Augment existing modules
declare module 'express-serve-static-core' {
  interface Request {
    user?: {
      id: string;
      name: string;
    };
  }
}
```

## Common Patterns

```typescript
// Pattern 1: Namespace-style libraries
declare module 'namespace-lib' {
  namespace MyLib {
    interface Options {
      timeout: number;
    }
    
    function create(options: Options): Instance;
    
    class Instance {
      run(): void;
    }
  }
  
  export = MyLib;
}

// Pattern 2: jQuery-style plugins
declare module 'jquery' {
  interface JQuery {
    myPlugin(options?: { setting: string }): JQuery;
  }
}

// Pattern 3: Node.js modules
declare module 'custom-node-module' {
  import { EventEmitter } from 'events';
  
  export class CustomEmitter extends EventEmitter {
    start(): void;
    stop(): void;
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Check DefinitelyTyped first | Create custom types before checking @types |
| Use specific types in declarations | Use `any` everywhere in module declarations |
| Put global declarations in .d.ts files | Mix declarations with regular TypeScript code |
| Version-pin @types packages | Use latest @types with older library versions |
| Document custom module declarations | Leave undocumented type declarations |

## Quick Fixes

- **"Cannot find module" error** → Install `@types/library-name` or create declaration
- **Types don't match library version** → Check @types version compatibility
- **Global variable not recognized** → Add to global interface in .d.ts file
- **Module has no default export** → Use `import * as lib` or `export =` in declaration
- **Property doesn't exist on type** → Augment the module or interface

## Gotchas

⚠️ **@types versions**: @types packages may not match your library version exactly - check compatibility

⚠️ **Declaration file location**: Global declarations must be in .d.ts files and included in tsconfig.json

⚠️ **Module augmentation**: When augmenting modules, you must import the original module first

⚠️ **Namespace vs module**: Use `declare module` for external packages, `declare namespace` for global libraries

## Checklist

- [ ] Check if @types package exists before creating custom declarations
- [ ] Verify @types version matches your library version
- [ ] Include .d.ts files in tsconfig.json "include" or "files"
- [ ] Test that imports work correctly after adding type declarations
- [ ] Document any custom type declarations for team members