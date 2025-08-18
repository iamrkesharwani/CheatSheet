# Core Concepts `v3.4`

## Essentials

```html
<!-- Mobile-first responsive design -->
<div class="p-4 bg-blue-500 text-white md:p-8 lg:bg-green-500">
  <h1 class="text-xl font-bold hover:text-gray-200 focus:outline-none">
    Responsive Header
  </h1>
</div>
```

**Key Concepts**: Utility-First | Mobile-First | Responsive | State Variants
**When to use**: Rapid prototyping, consistent design systems, component-based development

## Syntax Reference

### Basic Structure

```html
<!-- Class format: property-value or property-side-value -->
<div class="margin-top-4 padding-x-6 background-blue-500">
  <!-- Shorthand: mt-4 px-6 bg-blue-500 -->
</div>

<!-- Responsive prefixes: sm: md: lg: xl: 2xl: -->
<div class="w-full md:w-1/2 lg:w-1/3">Content</div>

<!-- State variants: hover: focus: active: group-hover: -->
<button class="bg-blue-500 hover:bg-blue-600 focus:ring-2">Click me</button>
```

### Common Patterns

```html
<!-- Pattern 1: Card component with hover effects -->
<div class="bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow p-6">
  <h2 class="text-xl font-semibold mb-2">Card Title</h2>
  <p class="text-gray-600">Card content</p>
</div>

<!-- Pattern 2: Responsive grid layout -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <div class="bg-gray-100 p-4 rounded">Item 1</div>
  <div class="bg-gray-100 p-4 rounded">Item 2</div>
  <div class="bg-gray-100 p-4 rounded">Item 3</div>
</div>

<!-- Pattern 3: Flexbox navigation -->
<nav class="flex items-center justify-between p-4 bg-white shadow">
  <div class="flex items-center space-x-4">
    <img class="h-8 w-8" src="logo.png" alt="Logo">
    <span class="font-bold text-xl">Brand</span>
  </div>
  <div class="hidden md:flex space-x-6">
    <a class="text-gray-600 hover:text-gray-900" href="#">Home</a>
    <a class="text-gray-600 hover:text-gray-900" href="#">About</a>
  </div>
</nav>

<!-- Pattern 4: Form with focus states -->
<form class="space-y-4">
  <input class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" 
         type="text" placeholder="Enter text">
  <button class="w-full bg-blue-500 text-white py-2 px-4 rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
    Submit
  </button>
</form>

<!-- Pattern 5: Group hover effects -->
<div class="group cursor-pointer">
  <img class="group-hover:scale-105 transition-transform" src="image.jpg" alt="Image">
  <h3 class="group-hover:text-blue-500 transition-colors">Hover to see effect</h3>
</div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use mobile-first approach (base → sm → md → lg) | Start with desktop classes |
| Combine utilities for complex designs | Write custom CSS for simple spacing |
| Use semantic grouping with space-x/space-y | Apply margins to every child element |
| Leverage component extraction for repeated patterns | Repeat long class strings everywhere |
| Use focus states for accessibility | Forget focus indicators on interactive elements |
| Apply hover effects to interactive elements | Add hover effects to non-interactive content |

## Quick Fixes

- **Classes not applying** → Check for typos and ensure Tailwind is properly installed
- **Responsive not working** → Remember mobile-first: use base classes, then add breakpoint prefixes
- **Hover effects not showing** → Ensure element is interactive (button, link) or add cursor-pointer
- **Layout breaking on mobile** → Remove fixed widths, use responsive utilities like w-full md:w-auto
- **Focus states missing** → Add focus:outline-none focus:ring-2 focus:ring-[color] for custom focus styles
- **Spacing inconsistent** → Use Tailwind's spacing scale (4, 8, 12, 16, 20, 24) instead of arbitrary values

## Gotchas

⚠️ **Mobile-first means base classes apply to all sizes** - `text-xl` affects all breakpoints unless overridden
⚠️ **Group utilities only work on direct children** - Use group-hover: on child elements, not nested grandchildren
⚠️ **Arbitrary values [10px] bypass the design system** - Use sparingly and prefer standard spacing scale
⚠️ **Purging removes unused classes** - Ensure class names are complete strings in your HTML, not dynamically generated
⚠️ **Focus states are removed by focus:outline-none** - Always replace with custom focus styles for accessibility

## Checklist

- [ ] Design mobile-first, then enhance for larger screens
- [ ] Use consistent spacing scale (multiples of 4px: 1, 2, 3, 4, 6, 8, 12, 16, 20, 24)
- [ ] Include hover and focus states for interactive elements
- [ ] Test responsive breakpoints: sm (640px), md (768px), lg (1024px), xl (1280px)
- [ ] Group related utilities logically in class strings
- [ ] Extract repeated patterns into reusable components
- [ ] Verify accessibility with proper focus indicators and semantic HTML