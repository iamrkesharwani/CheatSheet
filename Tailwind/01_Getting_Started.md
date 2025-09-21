# Getting Started with Tailwind CSS `v3.4`

## Essentials

```html
<!-- CDN Setup (Quick Start) -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Basic Usage -->
<div class="bg-blue-500 text-white p-4 rounded-lg">
  <h1 class="text-2xl font-bold">Hello Tailwind!</h1>
</div>
```

**Key Concepts**: Utility-First | Responsive Design | Component Composition
**When to use**: Rapid prototyping, consistent design systems, component-based development

## Syntax Reference

### CDN Setup (Prototyping)

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-8 rounded-lg shadow-md">
      <h1 class="text-3xl font-bold text-gray-900">Ready to go!</h1>
    </div>
  </div>
</body>
</html>
```

### CLI Installation

```bash
# Install Tailwind CLI
npm install -D tailwindcss
npm install -D tailwindcss@3 postcss autoprefixer

npx tailwindcss init

# Generate CSS
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```

### PostCSS Setup

```bash
# Install dependencies
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```javascript
// postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

### Framework Integration

```bash
# React/Next.js
npx create-next-app@latest my-project --tailwind

# Vite
npm create vite@latest my-project
cd my-project
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

// If error occurred, then use this:
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
```

## Common Patterns

```html
<!-- Card Component -->
<div class="max-w-sm mx-auto bg-white rounded-xl shadow-md overflow-hidden">
  <img class="w-full h-48 object-cover" src="image.jpg" alt="">
  <div class="p-6">
    <h3 class="text-xl font-semibold text-gray-900">Card Title</h3>
    <p class="text-gray-600 mt-2">Card description text here.</p>
  </div>
</div>

<!-- Button Variants -->
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Primary Button
</button>
<button class="bg-transparent hover:bg-blue-500 text-blue-700 hover:text-white border border-blue-500 hover:border-transparent rounded">
  Outlined Button
</button>

<!-- Responsive Grid -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <div class="bg-gray-200 p-4 rounded">Item 1</div>
  <div class="bg-gray-200 p-4 rounded">Item 2</div>
  <div class="bg-gray-200 p-4 rounded">Item 3</div>
</div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use semantic HTML with Tailwind classes | Rely only on divs with styling classes |
| Configure purge/content paths properly | Skip content configuration (bloated CSS) |
| Create component abstractions for repeated patterns | Repeat long class strings everywhere |
| Use responsive prefixes (sm:, md:, lg:) | Write separate CSS for breakpoints |
| Leverage design tokens (spacing, colors) | Use arbitrary values excessively |

## Quick Fixes

- **Large bundle size** → Configure `content` paths in `tailwind.config.js`
- **Styles not applying** → Check if Tailwind CSS is properly imported
- **Hover/focus not working** → Use proper pseudo-class prefixes (`hover:`, `focus:`)
- **Responsive not working** → Apply mobile-first approach (`base`, then `sm:`, `md:`, etc.)
- **Custom colors not available** → Extend theme in config file

## Gotchas

⚠️ **Mobile-First Responsive**: `md:text-lg` applies from medium screens UP, not down  
⚠️ **Purging**: Unused styles are removed in production - dynamic class names need safelist  
⚠️ **Specificity**: Custom CSS may be overridden by Tailwind utilities due to specificity  
⚠️ **Content Paths**: Must include all files using Tailwind classes in `content` array

## Configuration Setup

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{html,js,jsx,ts,tsx,vue}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a',
        }
      }
    },
  },
  plugins: [],
}
```

```css
/* src/input.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Custom component classes */
@layer components {
  .btn-primary {
    @apply bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded;
  }
}
```

## Checklist

- [ ] Install Tailwind CSS via CDN or package manager
- [ ] Configure `tailwind.config.js` with correct content paths
- [ ] Set up build process (CLI, PostCSS, or framework integration)
- [ ] Import Tailwind directives in CSS file
- [ ] Test responsive utilities and hover states
- [ ] Verify production build removes unused styles
