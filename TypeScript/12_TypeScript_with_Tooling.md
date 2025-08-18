# TypeScript with Tooling `v 5.3.3`

## Essentials

```typescript
// Basic TypeScript with React
import React, { useState } from 'react';

interface Props {
  name: string;
  age?: number;
}

const MyComponent: React.FC<Props> = ({ name, age = 0 }) => {
  const [count, setCount] = useState<number>(0);
  return <div>{name} is {age} years old</div>;
};
```

**Key Concepts**: Static Typing | Build Integration | IDE Support | Code Quality
**When to use**: JavaScript projects needing type safety, better refactoring, and enhanced developer experience

## Syntax Reference

### TypeScript Config (tsconfig.json)

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["DOM", "DOM.Iterable", "ES6"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModules": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### ESLint + Prettier Setup

```json
// .eslintrc.json
{
  "extends": [
    "@typescript-eslint/recommended",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "rules": {
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/explicit-function-return-type": "warn"
  }
}
```

```json
// prettier.config.js
module.exports = {
  semi: true,
  singleQuote: true,
  tabWidth: 2,
  trailingComma: 'es5'
};
```

### React TypeScript Patterns

```tsx
// Functional component with props
interface ButtonProps {
  children: React.ReactNode;
  onClick: (event: React.MouseEvent<HTMLButtonElement>) => void;
  variant?: 'primary' | 'secondary';
}

export const Button: React.FC<ButtonProps> = ({ 
  children, 
  onClick, 
  variant = 'primary' 
}) => (
  <button className={`btn btn-${variant}`} onClick={onClick}>
    {children}
  </button>
);

// Custom hooks
function useCounter(initialValue: number = 0) {
  const [count, setCount] = useState<number>(initialValue);
  
  const increment = () => setCount(prev => prev + 1);
  const decrement = () => setCount(prev => prev - 1);
  
  return { count, increment, decrement };
}

// Context with TypeScript
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = React.createContext<ThemeContextType | undefined>(undefined);
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use strict mode in tsconfig.json | Disable strict checks without understanding |
| Define interfaces for component props | Use `any` type as default solution |
| Set up ESLint with TypeScript parser | Mix tabs and spaces without Prettier |
| Use explicit return types for functions | Ignore TypeScript errors in production |
| Configure path mapping for imports | Commit generated .js files to version control |

## Quick Fixes

- **"Cannot find module" error** → Add `"moduleResolution": "node"` to tsconfig.json
- **ESLint conflicts with Prettier** → Install `eslint-config-prettier` and add to extends array
- **React imports not working** → Set `"jsx": "react-jsx"` in tsconfig for React 17+
- **VSCode not picking up types** → Restart TypeScript service (Cmd/Ctrl + Shift + P → "TypeScript: Restart TS Server")
- **Slow TypeScript compilation** → Enable `"incremental": true` and add `"tsBuildInfoFile"`

## Gotchas

⚠️ **VSCode Extension Conflicts**: Multiple TypeScript extensions can cause conflicts - use only the built-in TypeScript support

⚠️ **ESLint Parser Mismatch**: Ensure `@typescript-eslint/parser` version matches your TypeScript version

⚠️ **React 18 Types**: Install `@types/react@^18.0.0` for proper React 18 type definitions

⚠️ **Strict Mode Breaking Changes**: Enabling strict mode can reveal hidden type issues in existing code

## Package Installation

```bash
# Core TypeScript setup
npm install -D typescript @types/node

# React TypeScript
npm install -D @types/react @types/react-dom

# ESLint + TypeScript
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin

# Prettier integration
npm install -D prettier eslint-config-prettier eslint-plugin-prettier

# VSCode settings.json for auto-formatting
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

## Checklist

- [ ] Install TypeScript and type definitions for dependencies
- [ ] Configure tsconfig.json with strict mode enabled
- [ ] Set up ESLint with TypeScript parser and rules
- [ ] Configure Prettier for consistent code formatting
- [ ] Install VSCode TypeScript extension (built-in)
- [ ] Set up auto-formatting on save in VSCode
- [ ] Configure path mapping for cleaner imports
- [ ] Add TypeScript build script to package.json
- [ ] Set up pre-commit hooks for linting/formatting