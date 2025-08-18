# Tailwind CSS Layout Utilities `v 3.4.0`

## Essentials

```html
<!-- Core layout patterns -->
<div class="flex justify-center items-center">Centered content</div>
<div class="grid grid-cols-3 gap-4">Grid layout</div>
<div class="absolute top-4 right-4">Positioned element</div>
<div class="hidden md:block">Responsive visibility</div>
```

**Key Concepts**: Display | Position | Flexbox | Grid
**When to use**: Creating responsive layouts, aligning content, positioning elements

## Syntax Reference

### Display & Visibility

```html
<!-- Display types -->
<div class="block">Block element</div>
<div class="inline-block">Inline-block element</div>
<div class="inline">Inline element</div>
<div class="flex">Flexbox container</div>
<div class="grid">Grid container</div>
<div class="hidden">Hidden element</div>

<!-- Table display -->
<div class="table">Table display</div>
<div class="table-row">Table row</div>
<div class="table-cell">Table cell</div>
```

### Position & Placement

```html
<!-- Position types -->
<div class="static">Default positioning</div>
<div class="relative">Relative to normal position</div>
<div class="absolute">Absolute positioning</div>
<div class="fixed">Fixed to viewport</div>
<div class="sticky">Sticky positioning</div>

<!-- Position values -->
<div class="absolute top-0 left-0">Top-left corner</div>
<div class="absolute bottom-4 right-4">Bottom-right with spacing</div>
<div class="absolute inset-0">Fill entire container</div>
<div class="absolute inset-x-0 top-0">Full width, top aligned</div>
```

### Flexbox Layouts

```html
<!-- Flex containers -->
<div class="flex">Horizontal flex</div>
<div class="flex flex-col">Vertical flex</div>
<div class="inline-flex">Inline flex container</div>

<!-- Justify content (main axis) -->
<div class="flex justify-start">Start alignment</div>
<div class="flex justify-center">Center alignment</div>
<div class="flex justify-between">Space between items</div>
<div class="flex justify-around">Space around items</div>
<div class="flex justify-evenly">Even spacing</div>

<!-- Align items (cross axis) -->
<div class="flex items-start">Start alignment</div>
<div class="flex items-center">Center alignment</div>
<div class="flex items-end">End alignment</div>
<div class="flex items-stretch">Stretch to fill</div>

<!-- Flex wrapping -->
<div class="flex flex-wrap">Allow wrapping</div>
<div class="flex flex-nowrap">No wrapping</div>
<div class="flex flex-wrap-reverse">Reverse wrap</div>
```

### Grid Layouts

```html
<!-- Grid containers -->
<div class="grid grid-cols-1">Single column</div>
<div class="grid grid-cols-2">Two columns</div>
<div class="grid grid-cols-3">Three columns</div>
<div class="grid grid-cols-12">12-column grid</div>

<!-- Auto-fit columns -->
<div class="grid grid-cols-[repeat(auto-fit,minmax(200px,1fr))]">
  Responsive auto-fit grid
</div>

<!-- Grid rows -->
<div class="grid grid-rows-2">Two rows</div>
<div class="grid grid-rows-4">Four rows</div>

<!-- Grid gaps -->
<div class="grid grid-cols-3 gap-4">Uniform gap</div>
<div class="grid grid-cols-3 gap-x-4 gap-y-2">Different x/y gaps</div>
<div class="grid grid-cols-3 row-gap-2 col-gap-4">Row/column gaps</div>
```

### Spacing & Alignment

```html
<!-- Flex/grid gaps -->
<div class="flex gap-4">Flex with gap</div>
<div class="flex gap-x-4 gap-y-2">Different x/y gaps</div>

<!-- Space between (legacy) -->
<div class="flex space-x-4">Horizontal spacing</div>
<div class="flex flex-col space-y-4">Vertical spacing</div>

<!-- Text alignment -->
<div class="text-left">Left aligned text</div>
<div class="text-center">Center aligned text</div>
<div class="text-right">Right aligned text</div>
<div class="text-justify">Justified text</div>

<!-- Vertical alignment -->
<div class="align-top">Top aligned</div>
<div class="align-middle">Middle aligned</div>
<div class="align-bottom">Bottom aligned</div>
```

## Common Patterns

```html
<!-- Pattern 1: Centered card -->
<div class="flex justify-center items-center min-h-screen">
  <div class="bg-white p-6 rounded-lg shadow-lg">
    Centered card content
  </div>
</div>

<!-- Pattern 2: Header with navigation -->
<header class="flex justify-between items-center p-4">
  <div class="logo">Logo</div>
  <nav class="flex space-x-4">
    <a href="#">Link 1</a>
    <a href="#">Link 2</a>
  </nav>
</header>

<!-- Pattern 3: Responsive grid -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>

<!-- Pattern 4: Sidebar layout -->
<div class="flex">
  <aside class="w-64 bg-gray-100">Sidebar</aside>
  <main class="flex-1 p-6">Main content</main>
</div>

<!-- Pattern 5: Sticky footer -->
<div class="min-h-screen flex flex-col">
  <header class="bg-blue-500 p-4">Header</header>
  <main class="flex-1 p-4">Main content</main>
  <footer class="bg-gray-800 p-4">Footer</footer>
</div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `gap` for modern spacing | Use `space-x/y` with gap together |
| Use `flex` for 1D layouts | Use grid for simple single-row layouts |
| Use `grid` for 2D layouts | Force flexbox for complex 2D layouts |
| Use responsive prefixes (md:, lg:) | Create layouts that break on mobile |
| Combine positioning classes logically | Mix conflicting position values |
| Use `inset-0` for full coverage | Use individual top/right/bottom/left when unnecessary |

## Quick Fixes

- **Elements not centering** → Check both `justify-center` and `items-center` on flex container
- **Grid items not fitting** → Use `grid-cols-[repeat(auto-fit,minmax(min-width,1fr))]`
- **Flex items not wrapping** → Add `flex-wrap` to container
- **Absolute positioning not working** → Ensure parent has `relative` positioning
- **Gap not working** → Verify you're using it on flex/grid container, not individual items
- **Sticky not sticking** → Check parent container doesn't have `overflow: hidden`

## Gotchas

⚠️ **Flexbox shrinking**: Flex items shrink by default. Use `flex-shrink-0` to prevent shrinking
⚠️ **Grid auto-placement**: Items without explicit placement may not appear where expected
⚠️ **Space utilities vs Gap**: `space-*` adds margins between children, `gap` is container property
⚠️ **Position context**: `absolute` elements position relative to nearest positioned ancestor
⚠️ **Z-index stacking**: Use `z-*` utilities to control stacking order of positioned elements
⚠️ **Mobile-first**: Tailwind is mobile-first, so `md:` applies to medium screens and up

## Checklist

- [ ] Choose appropriate display type (block, flex, grid)
- [ ] Set container dimensions and constraints
- [ ] Apply positioning context for absolute elements
- [ ] Use responsive prefixes for different screen sizes
- [ ] Test layout behavior on mobile devices
- [ ] Verify accessibility with keyboard navigation
- [ ] Check spacing consistency across components
- [ ] Validate layout doesn't break with dynamic content