# ES Modules & Build Tools `v 0.07.25.01`

## Essentials
```javascript
// Named exports
export const name = 'value';
export function myFunc() {}

// Default export
export default class MyClass {}

// Import examples
import MyClass, { name, myFunc } from './module.js';
import('./module.js').then(module => { /* dynamic */ });
```

**Key Concepts**: Named/Default Exports | Static/Dynamic Imports | Module Resolution | Tree Shaking
**When to use**: Organizing code into reusable modules, enabling better bundling and optimization

## Syntax Reference

### Basic Structure
```javascript
// math.js - Export module
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export { multiply as mul }; // Rename export
export default function subtract(a, b) { return a - b; }

// main.js - Import module
import subtract, { PI, add, mul } from './math.js';
import * as math from './math.js'; // Namespace import
```

### Script Type Module
```html
<!-- Enable ES modules in browser -->
<script type="module" src="./main.js"></script>
<script type="module">
  import { myFunction } from './utils.js';
  myFunction();
</script>

<!-- Fallback for older browsers -->
<script nomodule src="./legacy-bundle.js"></script>
```

### Dynamic Imports
```javascript
// Conditional loading
if (condition) {
  const module = await import('./feature.js');
  module.initialize();
}

// Lazy loading with error handling
async function loadComponent() {
  try {
    const { default: Component } = await import('./Component.js');
    return Component;
  } catch (error) {
    console.error('Failed to load component:', error);
  }
}

// Top-level await (modern environments)
const config = await import('./config.js');
```

### Common Patterns
```javascript
// Pattern 1: Re-exporting from multiple modules
export { Button } from './Button.js';
export { Modal } from './Modal.js';
export * from './utils.js';

// Pattern 2: Barrel exports (index.js)
export { default as Component1 } from './Component1.js';
export { default as Component2 } from './Component2.js';

// Pattern 3: Conditional exports
export const feature = process.env.NODE_ENV === 'development' 
  ? await import('./dev-feature.js')
  : await import('./prod-feature.js');
```

## Browser vs Node.js

### Browser Modules
```javascript
// Explicit file extensions required
import utils from './utils.js';  // ✅ Works
import utils from './utils';     // ❌ Fails

// Relative/absolute paths or URLs
import lodash from 'https://cdn.skypack.dev/lodash';
```

### Node.js Modules
```javascript
// package.json setup
{
  "type": "module",  // Enable ES modules
  "exports": {       // Define entry points
    ".": "./index.js",
    "./utils": "./utils.js"
  }
}

// Import Node.js built-ins
import { readFile } from 'fs/promises';
import path from 'path';

// Import JSON (Node 17.5+)
import config from './config.json' assert { type: 'json' };
```

## Build Tools Overview

### Webpack
```javascript
// webpack.config.js - Basic setup
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist'
  },
  optimization: {
    usedExports: true, // Enable tree shaking
    splitChunks: {     // Code splitting
      chunks: 'all'
    }
  }
};
```

### Vite
```javascript
// vite.config.js - Modern dev server
import { defineConfig } from 'vite';

export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'] // Code splitting
        }
      }
    }
  }
});
```

### Babel
```javascript
// .babelrc - Transform modern JS
{
  "presets": [
    ["@babel/preset-env", {
      "modules": false,  // Preserve ES modules for bundler
      "targets": "> 0.25%, not dead"
    }]
  ]
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `.js` extensions in browser imports | Mix CommonJS and ES modules without configuration |
| Prefer named exports for utilities | Export everything as default |
| Use dynamic imports for code splitting | Import entire libraries when only using parts |
| Keep module graphs shallow | Create circular dependencies |

## Quick Fixes
- **"Unexpected token 'export'"** → Add `"type": "module"` to package.json or use `.mjs` extension
- **"Cannot use import outside module"** → Use `<script type="module">` in browser
- **"Module not found"** → Check file extensions (.js required in browser) and relative paths
- **"Cannot read properties of undefined"** → Verify export names match import names exactly

## Gotchas
⚠️ **Module scope**: Each module has its own scope; variables aren't global by default
⚠️ **Hoisting difference**: Import declarations are hoisted, but import() expressions are not
⚠️ **Live bindings**: Imported values reflect changes in the exporting module
⚠️ **Browser CORS**: Local file:// protocol blocks module loading; use a dev server

## Tree Shaking & Optimization

### Tree Shaking Setup
```javascript
// Enable in bundler (Webpack/Rollup/Vite)
// package.json
{
  "sideEffects": false  // Mark package as side-effect free
}

// Or specify files with side effects
{
  "sideEffects": ["./src/polyfills.js", "*.css"]
}
```

### Code Splitting Patterns
```javascript
// Route-based splitting (React example)
const HomePage = lazy(() => import('./pages/Home'));
const AboutPage = lazy(() => import('./pages/About'));

// Feature-based splitting
const analytics = await import('./analytics');
if (user.hasPremium) {
  const premium = await import('./premium-features');
}
```

## Checklist
- [ ] Added `type="module"` to HTML script tags or package.json
- [ ] Used explicit file extensions (.js) in browser imports
- [ ] Configured build tool for tree shaking (sideEffects: false)
- [ ] Implemented code splitting for large features/routes
- [ ] Set up proper module resolution in bundler config
- [ ] Tested both development and production builds