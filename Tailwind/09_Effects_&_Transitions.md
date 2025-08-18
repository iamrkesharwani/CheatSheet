# Tailwind CSS Effects & Transitions `v 3.4.0`

## Essentials

```css
/* Basic hover effect with transition */
<button class="bg-blue-500 hover:bg-blue-600 transition-colors duration-300">
  Click me
</button>

/* Scale animation with shadow */
<div class="hover:scale-105 hover:shadow-lg transition-transform duration-200">
  Card content
</div>
```

**Key Concepts**: Transitions | Transforms | Shadows | Opacity | Hover States
**When to use**: Interactive elements, micro-animations, visual feedback, state changes

## Syntax Reference

### Basic Structure

```html
<!-- Transition syntax: transition-{property} duration-{time} ease-{timing} -->
<element class="transition-all duration-300 ease-in-out hover:transform">
  
<!-- Transform syntax: {state}:{transform}-{value} -->
<element class="hover:scale-110 active:scale-95">

<!-- Shadow syntax: shadow-{size} or shadow-{color}/{opacity} -->
<element class="shadow-md hover:shadow-xl">
```

### Common Patterns

```html
<!-- Pattern 1: Button hover effect -->
<button class="bg-indigo-500 hover:bg-indigo-600 hover:scale-105 
               transition-all duration-200 ease-out shadow-md hover:shadow-lg">
  Interactive Button
</button>

<!-- Pattern 2: Card lift animation -->
<div class="bg-white rounded-lg shadow-sm hover:shadow-xl 
           hover:-translate-y-1 transition-all duration-300">
  Card Content
</div>

<!-- Pattern 3: Fade in/out -->
<div class="opacity-0 hover:opacity-100 transition-opacity duration-500">
  Fade content
</div>

<!-- Pattern 4: Rotate on hover -->
<div class="transform hover:rotate-6 transition-transform duration-300">
  Rotatable element
</div>

<!-- Pattern 5: Text shadow (custom) -->
<h1 class="text-4xl font-bold" style="text-shadow: 2px 2px 4px rgba(0,0,0,0.3);">
  Custom Text Shadow
</h1>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use transition-all for multiple property changes | Animate too many elements simultaneously |
| Keep durations under 400ms for micro-interactions | Use very long durations (>1s) for hover effects |
| Combine transforms for compound animations | Chain multiple transition classes unnecessarily |
| Use ease-out for natural feeling animations | Forget to add transition classes before hover states |
| Test on mobile devices for touch interactions | Use transforms that break layout flow |

## Quick Fixes

- **Transform not working** → Add `transition-transform` class
- **Choppy animations** → Use `will-change-transform` or reduce animated elements
- **Hover stuck on mobile** → Add `@media (hover: hover)` wrapper or use focus states
- **Layout shift during animation** → Use `transform` instead of changing `width`/`height`
- **Shadow not visible** → Check background color contrast or use `shadow-colored`

## Gotchas

⚠️ **Transform origin**: Transforms scale/rotate from center by default. Use `origin-{position}` to change.
⚠️ **Stacking context**: Transforms create new stacking contexts, affecting z-index behavior.
⚠️ **Performance**: Prefer `transform` and `opacity` over animating other properties for 60fps.
⚠️ **Touch devices**: Hover states persist on touch. Consider `:focus` or `:active` alternatives.

## Reference Tables

### Transform Values
```html
<!-- Scale -->
scale-0, scale-50, scale-75, scale-90, scale-95
scale-100, scale-105, scale-110, scale-125, scale-150

<!-- Rotate -->
rotate-0, rotate-1, rotate-2, rotate-3, rotate-6, rotate-12
rotate-45, rotate-90, rotate-180, -rotate-{value}

<!-- Translate -->
translate-x-{size}, translate-y-{size}
-translate-x-{size}, -translate-y-{size}
```

### Transition Durations
```html
duration-75    <!-- 75ms -->
duration-100   <!-- 100ms -->
duration-150   <!-- 150ms -->
duration-200   <!-- 200ms -->
duration-300   <!-- 300ms -->
duration-500   <!-- 500ms -->
duration-700   <!-- 700ms -->
duration-1000  <!-- 1000ms -->
```

### Shadow Variants
```html
shadow-sm      <!-- Small shadow -->
shadow         <!-- Default shadow -->
shadow-md      <!-- Medium shadow -->
shadow-lg      <!-- Large shadow -->
shadow-xl      <!-- Extra large -->
shadow-2xl     <!-- 2x extra large -->
shadow-inner   <!-- Inset shadow -->
shadow-none    <!-- Remove shadow -->
```

### Opacity Levels
```html
opacity-0, opacity-5, opacity-10, opacity-20, opacity-25
opacity-30, opacity-40, opacity-50, opacity-60, opacity-70
opacity-75, opacity-80, opacity-90, opacity-95, opacity-100
```

## Checklist

- [ ] Added `transition-{property}` before hover states
- [ ] Set appropriate `duration-{time}` (200-300ms for most interactions)
- [ ] Used `ease-out` for natural feeling animations
- [ ] Tested hover effects on touch devices
- [ ] Verified transforms don't break layout
- [ ] Added focus states for accessibility
- [ ] Checked animation performance on slower devices