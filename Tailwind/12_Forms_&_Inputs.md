# Tailwind CSS Forms & Inputs `v3.4.0`

## Essentials

```html
<!-- Basic input styling -->
<input class="border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent">

<!-- Select dropdown -->
<select class="border border-gray-300 rounded-md px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-blue-500">
  <option>Choose option</option>
</select>

<!-- Textarea -->
<textarea class="border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500 resize-none"></textarea>
```

**Key Concepts**: Focus States | Border Styling | Shadow Effects | State Modifiers
**When to use**: Creating consistent, accessible form inputs with proper visual feedback

## Syntax Reference

### Basic Input Structure

```html
<!-- Standard input pattern -->
<input 
  type="text" 
  class="
    block w-full
    border border-gray-300 
    rounded-md shadow-sm
    px-3 py-2
    focus:outline-none 
    focus:ring-2 focus:ring-blue-500 
    focus:border-transparent
  "
>

<!-- With Tailwind Forms Plugin -->
<input type="text" class="form-input rounded-md border-gray-300 focus:border-blue-500 focus:ring-blue-500">
```

### Common Input Patterns

```html
<!-- Pattern 1: Clean modern input -->
<input class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-colors">

<!-- Pattern 2: Bordered input with shadow -->
<input class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500">

<!-- Pattern 3: Underline style -->
<input class="w-full px-0 py-2 border-0 border-b-2 border-gray-300 focus:ring-0 focus:border-blue-500 bg-transparent">

<!-- Pattern 4: Filled/background style -->
<input class="w-full px-4 py-3 bg-gray-100 border border-transparent rounded-lg focus:bg-white focus:border-blue-500 focus:ring-2 focus:ring-blue-500/20">
```

### Select & Textarea Styling

```html
<!-- Custom select -->
<select class="w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500">
  <option>Option 1</option>
  <option>Option 2</option>
</select>

<!-- Textarea with resize control -->
<textarea class="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-1 focus:ring-blue-500 focus:border-blue-500 resize-y min-h-[100px]"></textarea>
```

### State Modifiers

```html
<!-- Disabled state -->
<input class="px-3 py-2 border border-gray-300 rounded-md disabled:opacity-50 disabled:cursor-not-allowed disabled:bg-gray-50" disabled>

<!-- Read-only state -->
<input class="px-3 py-2 border border-gray-300 rounded-md read-only:bg-gray-50 read-only:focus:ring-0" readonly>

<!-- Invalid/Error state -->
<input class="px-3 py-2 border border-red-300 rounded-md focus:ring-red-500 focus:border-red-500 invalid:border-red-500 invalid:ring-red-500">

<!-- Valid state -->
<input class="px-3 py-2 border border-green-300 rounded-md focus:ring-green-500 focus:border-green-500 valid:border-green-500">
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `focus:outline-none` to remove default outline | Forget to add focus ring when removing outline |
| Install @tailwindcss/forms plugin for consistency | Rely on browser defaults for form styling |
| Use semantic color names (blue-500, gray-300) | Use arbitrary color values without good reason |
| Add `transition-colors` for smooth state changes | Make focus changes jarring or instant |
| Use `block w-full` for full-width inputs | Forget to set proper width containers |
| Test disabled and readonly states | Assume all states look good by default |

## Quick Fixes

- **Input looks unstyled** → Add border, padding, and focus states: `border border-gray-300 px-3 py-2 focus:ring-2 focus:ring-blue-500`
- **Focus ring missing** → Remove `focus:outline-none` or add `focus:ring-2 focus:ring-blue-500`
- **Select dropdown looks weird** → Add `bg-white` and proper border styling
- **Textarea resizes strangely** → Use `resize-none`, `resize-y`, or `resize-x` to control
- **Forms look inconsistent** → Install and configure @tailwindcss/forms plugin
- **Disabled inputs unclear** → Add `disabled:opacity-50 disabled:cursor-not-allowed`

## Gotchas

⚠️ **Focus outline removal**: Always replace removed outlines with focus rings for accessibility
⚠️ **Select styling limitations**: Some select styling is browser-dependent; consider custom dropdowns for complex designs
⚠️ **Forms plugin conflicts**: @tailwindcss/forms resets all form styling - you'll need to re-add custom styles
⚠️ **Mobile input zoom**: Use `text-base` (16px) or larger to prevent iOS zoom on focus
⚠️ **Placeholder styling**: Use `placeholder:text-gray-400` to style placeholder text

## Tailwind Forms Plugin Setup

```bash
# Install the plugin
npm install -D @tailwindcss/forms
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/forms'),
    // or with specific strategy
    require('@tailwindcss/forms')({
      strategy: 'class', // 'base' or 'class'
    }),
  ],
}
```

```html
<!-- With forms plugin - cleaner classes -->
<input type="email" class="form-input rounded-md border-gray-300 focus:border-indigo-500 focus:ring-indigo-500">
<select class="form-select rounded-md border-gray-300 focus:border-indigo-500 focus:ring-indigo-500">
<textarea class="form-textarea rounded-md border-gray-300 focus:border-indigo-500 focus:ring-indigo-500"></textarea>
```

## Checklist

- [ ] Added proper focus states with `focus:ring-2` and `focus:ring-[color]`
- [ ] Removed default outline with `focus:outline-none` when using custom focus styles
- [ ] Applied consistent padding (`px-3 py-2` is standard)
- [ ] Set appropriate border radius (`rounded-md` is common)
- [ ] Handled disabled states with `disabled:opacity-50`
- [ ] Tested keyboard navigation and accessibility
- [ ] Considered mobile experience (16px+ font size)
- [ ] Added transitions for smooth state changes
- [ ] Styled error/validation states appropriately
- [ ] Used semantic color choices consistently