# TypeScript Type Aliases & Interfaces `v 5.3.3`

## Essentials

```typescript
// Type alias - creates a new name for any type
type UserId = string;
type User = { id: UserId; name: string; };

// Interface - defines object shape with enforcement
interface Product {
  id: number;
  name: string;
  price?: number; // optional property
}
```

**Key Concepts**: Custom Types | Object Shapes | Type Safety | Extensibility
**When to use**: Define reusable type structures and enforce consistent data shapes

## Syntax Reference

### Basic Structure

```typescript
// Type aliases - any type can be aliased
type StringOrNumber = string | number;
type UserRole = 'admin' | 'user' | 'guest';
type Callback = (data: string) => void;

// Interfaces - object structure definition
interface ApiResponse {
  success: boolean;
  data: any;
  message?: string; // optional with ?
  readonly timestamp: number; // immutable property
}
```

### Common Patterns

```typescript
// Pattern 1: Extending interfaces
interface BaseUser {
  id: string;
  name: string;
}

interface AdminUser extends BaseUser {
  permissions: string[];
  lastLogin: Date;
}

// Pattern 2: Composing types with intersection
type Timestamped = { createdAt: Date; updatedAt: Date; };
type UserWithTimestamp = User & Timestamped;

// Pattern 3: Generic types and interfaces
type ApiResult<T> = {
  data: T;
  error?: string;
};

interface Repository<T> {
  findById(id: string): Promise<T>;
  save(item: T): Promise<T>;
}

// Pattern 4: Index signatures for dynamic properties
interface StringDictionary {
  [key: string]: string;
}

interface UserPreferences {
  theme: 'light' | 'dark';
  [setting: string]: any; // allows additional properties
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `interface` for object shapes that might be extended | Use `type` when you need interface extension |
| Use `type` for unions, primitives, and computed types | Use `interface` for simple primitive aliases |
| Prefer `interface` for public APIs and library definitions | Mix `interface` and `type` inconsistently in same codebase |
| Use optional properties (`?`) for truly optional fields | Make required fields optional to avoid runtime errors |
| Use `readonly` for immutable properties | Modify readonly properties (causes compile errors) |
| Name interfaces with PascalCase | Use generic names like `Data` or `Info` |

## Quick Fixes

- **"Type 'X' is not assignable to type 'Y'"** → Check property names, types, and required vs optional fields
- **"Property 'X' does not exist on type 'Y'"** → Add property to interface or make it optional with `?`
- **"Cannot assign to 'X' because it is a read-only property"** → Remove `readonly` or create new object instead of mutating
- **"Interface 'X' incorrectly extends interface 'Y'"** → Ensure extending interface has compatible property types
- **"Duplicate identifier 'X'"** → Interfaces merge automatically; types don't. Use unique names or leverage merging intentionally

## Gotchas

⚠️ **Interface Declaration Merging**: Multiple interface declarations with same name automatically merge - useful for extending third-party types but can cause confusion

⚠️ **Type vs Interface Performance**: Interfaces are slightly faster for TypeScript compiler, especially with inheritance chains

⚠️ **Computed Properties**: Types can use computed property names and complex expressions; interfaces cannot

⚠️ **Union Types**: `type` can represent unions (`string | number`); interfaces cannot directly represent unions

## Checklist

- [ ] Choose `interface` for extensible object shapes, `type` for unions and aliases
- [ ] Use optional properties (`?`) appropriately - don't make everything optional
- [ ] Apply `readonly` to properties that shouldn't change after creation
- [ ] Consider using generic parameters (`<T>`) for reusable type definitions
- [ ] Name types and interfaces clearly and consistently (PascalCase recommended)
- [ ] Leverage interface extension (`extends`) and type intersection (`&`) for composition
- [ ] Add index signatures when objects have dynamic properties
- [ ] Document complex types with JSDoc comments for better developer experience