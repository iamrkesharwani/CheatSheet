 # Tailwind CSS: Colors `v3.4.0`

## Essentials

```html
<!-- Text Colors -->
<p class="text-blue-600">Blue text</p>

<!-- Background Colors -->
<div class="bg-red-500">Red background</div>

<!-- Border Colors -->
<div class="border border-green-400">Green border</div>

<!-- With Opacity -->
<div class="bg-purple-500/75 text-white/90">Semi-transparent</div>

<!-- Interactive States -->
<button class="bg-blue-500 hover:bg-blue-600 focus:bg-blue-700">Button</button>
```

**Key Concepts**: Color Utilities | Opacity Modifiers | Interactive States | Dark Mode | Custom Colors
**When to use**: Theming, branding, accessibility, responsive design, dark mode support

## Syntax Reference

### Color Scale System

```html
<!-- Tailwind's Color Scale: 50 (lightest) to 950 (darkest) -->
<div class="bg-gray-50">Very light gray</div>
<div class="bg-gray-100">Light gray</div>
<div class="bg-gray-200">Lighter gray</div>
<div class="bg-gray-300">Light gray</div>
<div class="bg-gray-400">Gray</div>
<div class="bg-gray-500">Medium gray</div>
<div class="bg-gray-600">Dark gray</div>
<div class="bg-gray-700">Darker gray</div>
<div class="bg-gray-800">Very dark gray</div>
<div class="bg-gray-900">Almost black</div>
<div class="bg-gray-950">Darkest gray</div>
```

### Text Colors

```html
<!-- Basic Text Colors -->
<p class="text-black">Black text</p>
<p class="text-white">White text</p>
<p class="text-gray-500">Gray text</p>
<p class="text-red-600">Red text</p>
<p class="text-blue-500">Blue text</p>
<p class="text-green-600">Green text</p>

<!-- Special Color Keywords -->
<p class="text-inherit">Inherits parent color</p>
<p class="text-current">Uses currentColor</p>
<p class="text-transparent">Transparent text</p>

<!-- With Opacity -->
<p class="text-red-500/75">75% opacity red</p>
<p class="text-blue-600/50">50% opacity blue</p>
<p class="text-black/25">25% opacity black</p>
```

### Background Colors

```html
<!-- Solid Backgrounds -->
<div class="bg-white">White background</div>
<div class="bg-black">Black background</div>
<div class="bg-red-500">Red background</div>
<div class="bg-blue-600">Blue background</div>

<!-- Special Keywords -->
<div class="bg-inherit">Inherits parent background</div>
<div class="bg-current">Uses currentColor</div>
<div class="bg-transparent">Transparent background</div>

<!-- With Opacity -->
<div class="bg-red-500/90">90% opacity red</div>
<div class="bg-blue-600/60">60% opacity blue</div>
<div class="bg-black/40">40% opacity black overlay</div>
```

### Border Colors

```html
<!-- Border Colors -->
<div class="border border-gray-300">Gray border</div>
<div class="border border-red-500">Red border</div>
<div class="border border-blue-600">Blue border</div>

<!-- Individual Border Sides -->
<div class="border-t-2 border-t-red-500">Red top border</div>
<div class="border-l-4 border-l-blue-600">Blue left border</div>

<!-- Special Keywords -->
<div class="border border-inherit">Inherits border color</div>
<div class="border border-current">Current color border</div>
<div class="border border-transparent">Transparent border</div>

<!-- With Opacity -->
<div class="border border-red-500/50">50% opacity red border</div>
```

### Available Color Palette

```html
<!-- Primary Colors -->
<div class="bg-slate-500">Slate (cool gray)</div>
<div class="bg-gray-500">Gray (neutral)</div>
<div class="bg-zinc-500">Zinc (warm gray)</div>
<div class="bg-neutral-500">Neutral</div>
<div class="bg-stone-500">Stone (warm gray)</div>

<!-- Brand Colors -->
<div class="bg-red-500">Red</div>
<div class="bg-orange-500">Orange</div>
<div class="bg-amber-500">Amber</div>
<div class="bg-yellow-500">Yellow</div>
<div class="bg-lime-500">Lime</div>
<div class="bg-green-500">Green</div>
<div class="bg-emerald-500">Emerald</div>
<div class="bg-teal-500">Teal</div>
<div class="bg-cyan-500">Cyan</div>
<div class="bg-sky-500">Sky</div>
<div class="bg-blue-500">Blue</div>
<div class="bg-indigo-500">Indigo</div>
<div class="bg-violet-500">Violet</div>
<div class="bg-purple-500">Purple</div>
<div class="bg-fuchsia-500">Fuchsia</div>
<div class="bg-pink-500">Pink</div>
<div class="bg-rose-500">Rose</div>
```

## Interactive States

### Hover & Focus States

```html
<!-- Text Color States -->
<a href="#" class="text-blue-600 hover:text-blue-800 focus:text-blue-900">
  Hover link
</a>

<!-- Background Color States -->
<button class="bg-green-500 hover:bg-green-600 focus:bg-green-700 
               active:bg-green-800 text-white px-4 py-2 rounded">
  Interactive Button
</button>

<!-- Border Color States -->
<input class="border border-gray-300 focus:border-blue-500 
              focus:outline-none px-3 py-2 rounded" 
       placeholder="Focus me">

<!-- Combined States with Opacity -->
<div class="bg-purple-500/20 hover:bg-purple-500/40 
           border border-purple-500/50 hover:border-purple-500/75">
  Hover for opacity change
</div>
```

### Multi-State Combinations

```html
<!-- Complex Interactive Card -->
<div class="bg-white border border-gray-200 hover:border-gray-300 
           hover:bg-gray-50 focus-within:border-blue-500 
           transition-colors duration-200 rounded-lg p-4">
  Interactive Card Content
</div>

<!-- Form Input States -->
<input class="border border-gray-300 text-gray-900 
              focus:border-blue-500 focus:text-black
              invalid:border-red-500 invalid:text-red-900
              disabled:bg-gray-100 disabled:text-gray-400">
```

## Dark Mode

### Basic Dark Mode

```html
<!-- Text Colors -->
<p class="text-gray-900 dark:text-gray-100">
  Dark text on light, light text on dark
</p>

<!-- Background Colors -->
<div class="bg-white dark:bg-gray-900">
  White background switches to dark
</div>

<!-- Border Colors -->
<div class="border border-gray-300 dark:border-gray-700">
  Adaptive border color
</div>
```

### Dark Mode Patterns

```html
<!-- Card Component -->
<div class="bg-white dark:bg-gray-800 
           border border-gray-200 dark:border-gray-700 
           text-gray-900 dark:text-gray-100 
           rounded-lg p-6">
  <h3 class="text-gray-900 dark:text-white">Card Title</h3>
  <p class="text-gray-600 dark:text-gray-300">Card description</p>
</div>

<!-- Interactive Elements -->
<button class="bg-blue-500 hover:bg-blue-600 
               dark:bg-blue-600 dark:hover:bg-blue-700 
               text-white px-4 py-2 rounded">
  Dark Mode Button
</button>

<!-- Form Elements -->
<input class="bg-white dark:bg-gray-800 
              border border-gray-300 dark:border-gray-600 
              text-gray-900 dark:text-gray-100 
              focus:border-blue-500 dark:focus:border-blue-400">
```

## Custom Colors Configuration

### tailwind.config.js

```javascript
// Basic custom colors
module.exports = {
  theme: {
    extend: {
      colors: {
        'brand': '#1a73e8',
        'brand-dark': '#1557b0',
        'custom-gray': '#f8f9fa',
      }
    }
  }
}

// Complete color palette
module.exports = {
  theme: {
    extend: {
      colors: {
        'primary': {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',  // Main brand color
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
        },
        'success': '#10b981',
        'warning': '#f59e0b',
        'error': '#ef4444',
      }
    }
  }
}

// Using CSS custom properties
module.exports = {
  theme: {
    extend: {
      colors: {
        'brand': 'var(--color-brand)',
        'surface': 'var(--color-surface)',
      }
    }
  }
}
```

### Using Custom Colors

```html
<!-- Using custom colors -->
<div class="bg-brand text-white">Brand background</div>
<div class="text-primary-600">Primary text</div>
<div class="border border-success">Success border</div>

<!-- With opacity -->
<div class="bg-brand/75">75% opacity brand color</div>
<div class="text-primary-500/90">90% opacity primary text</div>
```

## Common Patterns

```html
<!-- Status Indicators -->
<span class="bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200 
             px-2 py-1 rounded-full text-sm">
  Success
</span>
<span class="bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-200 
             px-2 py-1 rounded-full text-sm">
  Error
</span>

<!-- Gradient Text -->
<h1 class="bg-gradient-to-r from-purple-600 to-blue-600 
           bg-clip-text text-transparent text-4xl font-bold">
  Gradient Text
</h1>

<!-- Accessible Color Combinations -->
<div class="bg-blue-600 text-white">High contrast</div>
<div class="bg-gray-100 text-gray-900">Light mode readable</div>
<div class="bg-gray-800 text-gray-100">Dark mode readable</div>

<!-- Color-coded Navigation -->
<nav class="space-x-4">
  <a class="text-blue-600 hover:text-blue-800">Home</a>
  <a class="text-green-600 hover:text-green-800">Products</a>
  <a class="text-purple-600 hover:text-purple-800">Services</a>
  <a class="text-red-600 hover:text-red-800">Contact</a>
</nav>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use semantic color names in custom config | Use arbitrary hex values in HTML classes |
| Test color contrast for accessibility | Assume colors work in both light and dark modes |
| Use opacity modifiers (/50, /75) for overlays | Use separate opacity utilities when modifiers work |
| Plan dark mode colors from the start | Add dark mode as an afterthought |
| Use consistent color scales (50-950) | Mix different color naming systems |
| Test with colorblind users | Rely only on color for important information |

## Quick Fixes

- **Colors not showing in dark mode** → Add dark: variants to all color utilities
- **Custom colors not working** → Check tailwind.config.js syntax and rebuild
- **Poor contrast accessibility** → Use color contrast checkers and follow WCAG guidelines
- **Colors look different than expected** → Check color space and monitor calibration
- **Opacity not working** → Use /opacity syntax or ensure backdrop-blur if needed
- **Gradient text not working** → Add bg-clip-text and text-transparent classes

## Gotchas

⚠️ **Opacity modifiers don't work with arbitrary values**: Use rgb() with alpha channel instead
⚠️ **currentColor inherits from text color**: Useful for icons but can cause unexpected results
⚠️ **Dark mode requires class or media strategy**: Configure in tailwind.config.js
⚠️ **Color blindness considerations**: Don't rely solely on color for important information
⚠️ **Custom colors need rebuild**: Tailwind must regenerate CSS after config changes

## Accessibility Guidelines

```html
<!-- Good Contrast Ratios -->
<div class="bg-blue-600 text-white">✅ High contrast (4.5:1+)</div>
<div class="bg-yellow-400 text-yellow-900">✅ Good contrast</div>

<!-- Avoid Poor Contrast -->
<div class="bg-yellow-400 text-white">❌ Poor contrast</div>
<div class="bg-gray-400 text-gray-500">❌ Insufficient contrast</div>

<!-- Color + Text for Important Info -->
<div class="flex items-center space-x-2">
  <div class="w-3 h-3 bg-green-500 rounded-full"></div>
  <span class="text-green-700">✅ Available (color + text)</span>
</div>
```

## Checklist

- [ ] Test color combinations for accessibility (4.5:1 contrast minimum)
- [ ] Plan dark mode color variants from the beginning
- [ ] Use semantic color names in custom configuration
- [ ] Test colors with colorblind simulation tools  
- [ ] Ensure interactive states have sufficient color changes
- [ ] Use opacity modifiers instead of separate opacity utilities
- [ ] Document custom color meanings for team consistency
- [ ] Test colors across different devices and monitors
- [ ] Provide non-color alternatives for important information
- [ ] Configure dark mode strategy (class vs media preference)