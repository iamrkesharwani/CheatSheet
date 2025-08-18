# Tailwind CSS Effects & Transitions `v3.4.0`

## Essentials

```html
<!-- Basic shadow and hover effect -->
<div class="shadow-lg hover:shadow-xl transition-shadow duration-300">
  Card with shadow
</div>

<!-- Scale on hover with smooth transition -->
<button class="hover:scale-105 active:scale-95 transition-transform duration-200">
  Interactive Button
</button>
```

**Key Concepts**: Box Shadows | Opacity | Transforms | Transitions | Hover States
**When to use**: Adding visual feedback, smooth interactions, and depth to UI elements

## Syntax Reference

### Box Shadows

```html
<!-- Shadow variations -->
<div class="shadow-sm">Small shadow</div>
<div class="shadow">Default shadow</div>
<div class="shadow-md">Medium shadow</div>
<div class="shadow-lg">Large shadow</div>
<div class="shadow-xl">Extra large shadow</div>
<div class="shadow-2xl">2xl shadow</div>
<div class="shadow-inner">Inner shadow</div>
<div class="shadow-none">No shadow</div>

<!-- Colored shadows -->
<div class="shadow-lg shadow-blue-500/50">Blue shadow</div>
<div class="shadow-xl shadow-red-500/25">Red shadow</div>
```

### Text Shadow (Custom)

```css
/* Add to your CSS or @layer utilities */
@layer utilities {
  .text-shadow {
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  }
  .text-shadow-lg {
    text-shadow: 4px 4px 8px rgba(0, 0, 0, 0.2);
  }
}
```

### Opacity

```html
<!-- Static opacity -->
<div class="opacity-0">Invisible</div>
<div class="opacity-25">25% visible</div>
<div class="opacity-50">50% visible</div>
<div class="opacity-75">75% visible</div>
<div class="opacity-100">Fully visible</div>

<!-- Interactive opacity -->
<div class="opacity-50 hover:opacity-100 transition-opacity">
  Fade in on hover
</div>
```

### Transforms

```html
<!-- Scale transforms -->
<div class="scale-50">50% size</div>
<div class="scale-75">75% size</div>
<div class="scale-100">Normal size</div>
<div class="scale-105">105% size</div>
<div class="scale-110">110% size</div>
<div class="scale-125">125% size</div>

<!-- Rotation -->
<div class="rotate-0">No rotation</div>
<div class="rotate-45">45 degrees</div>
<div class="rotate-90">90 degrees</div>
<div class="rotate-180">180 degrees</div>
<div class="-rotate-45">-45 degrees</div>

<!-- Translation -->
<div class="translate-x-4">Move right</div>
<div class="translate-y-4">Move down</div>
<div class="-translate-x-4">Move left</div>
<div class="translate-x-4 translate-y-4">Move diagonal</div>
```

### Transitions

```html
<!-- Transition properties -->
<div class="transition-none">No transition</div>
<div class="transition-all">All properties</div>
<div class="transition">Default (color, bg, border, text, shadow)</div>
<div class="transition-colors">Colors only</div>
<div class="transition-opacity">Opacity only</div>
<div class="transition-shadow">Shadow only</div>
<div class="transition-transform">Transform only</div>

<!-- Duration -->
<div class="transition duration-75">75ms</div>
<div class="transition duration-150">150ms</div>
<div class="transition duration-300">300ms (common)</div>
<div class="transition duration-500">500ms</div>
<div class="transition duration-1000">1000ms</div>

<!-- Timing functions -->
<div class="transition ease-linear">Linear</div>
<div class="transition ease-in">Ease in</div>
<div class="transition ease-out">Ease out</div>
<div class="transition ease-in-out">Ease in-out</div>
```

## Common Patterns

```html
<!-- Pattern 1: Card hover effect -->
<div class="bg-white shadow-md hover:shadow-lg transition-shadow duration-300 rounded-lg p-6">
  <h3>Hover Card</h3>
  <p>Shadow increases on hover</p>
</div>

<!-- Pattern 2: Button with multiple effects -->
<button class="bg-blue-500 text-white px-4 py-2 rounded 
               hover:bg-blue-600 hover:scale-105 
               active:scale-95 
               transition-all duration-200 
               shadow-md hover:shadow-lg">
  Interactive Button
</button>

<!-- Pattern 3: Image zoom on hover -->
<div class="overflow-hidden rounded-lg">
  <img src="image.jpg" class="hover:scale-110 transition-transform duration-500" />
</div>

<!-- Pattern 4: Fade in/out overlay -->
<div class="relative">
  <img src="image.jpg" class="w-full" />
  <div class="absolute inset-0 bg-black opacity-0 hover:opacity-50 
              transition-opacity duration-300 
              flex items-center justify-center">
    <span class="text-white">Overlay Content</span>
  </div>
</div>

<!-- Pattern 5: Loading spinner -->
<div class="animate-spin w-8 h-8 border-4 border-blue-500 border-t-transparent rounded-full"></div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use duration-300 for most transitions | Make transitions longer than 500ms |
| Combine hover:scale-105 with active:scale-95 | Use transforms without transitions |
| Apply transitions to transform properties | Transition all properties unnecessarily |
| Use ease-out for hover effects | Stack too many effects on one element |
| Test on mobile devices | Forget focus states for accessibility |

## Quick Fixes

- **Choppy animations** → Add `transition-transform duration-300`
- **Shadow not visible** → Check background color and increase shadow size
- **Transform not working** → Ensure element isn't `inline` (use `inline-block` or `block`)
- **Transition on page load** → Add transition class after component mounts
- **Performance issues** → Use `transform` instead of changing `left/top` properties

## Gotchas

⚠️ **Transform origin**: Transforms scale/rotate from center by default. Use `origin-top-left` etc. to change pivot point

⚠️ **Stacking context**: Transforms create new stacking contexts, affecting z-index behavior

⚠️ **Text shadow**: Not included in Tailwind by default - requires custom CSS in @layer utilities

⚠️ **Mobile performance**: Avoid animating expensive properties like box-shadow on mobile; prefer transform and opacity

⚠️ **Accessibility**: Always provide `prefers-reduced-motion` alternatives for users with motion sensitivity

## Checklist

- [ ] Added appropriate transition duration (usually 200-300ms)
- [ ] Included hover AND active states for buttons
- [ ] Tested animation performance on slower devices
- [ ] Verified transforms don't break layout
- [ ] Added focus states for keyboard navigation
- [ ] Considered reduced motion preferences
- [ ] Used transform/opacity for smooth 60fps animations