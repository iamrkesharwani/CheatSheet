# Tailwind CSS: Backgrounds & Borders `v3.4.0`

## Essentials

```html
<!-- Background Colors -->
<div class="bg-blue-500 hover:bg-blue-600"></div>

<!-- Background Images -->
<div class="bg-cover bg-center bg-no-repeat" style="background-image: url('hero.jpg')"></div>

<!-- Borders -->
<div class="border border-gray-300 rounded-lg"></div>

<!-- Border Sides -->
<div class="border-t-2 border-b border-blue-500"></div>
```

**Key Concepts**: Background Colors | Background Images | Border Utilities | Border Radius | Divide Utilities
**When to use**: Styling visual presentation, creating cards, separating content, responsive design states

## Syntax Reference

### Background Colors

```html
<!-- Solid Colors -->
<div class="bg-red-500"></div>        <!-- Color with opacity -->
<div class="bg-red-500/50"></div>     <!-- 50% opacity -->
<div class="bg-transparent"></div>     <!-- Transparent -->
<div class="bg-current"></div>        <!-- Inherits text color -->

<!-- Gradients -->
<div class="bg-gradient-to-r from-blue-500 to-purple-600"></div>
<div class="bg-gradient-to-br from-green-400 via-blue-500 to-purple-600"></div>
```

### Background Images

```html
<!-- Background Size -->
<div class="bg-cover"></div>      <!-- Scale to cover entire container -->
<div class="bg-contain"></div>    <!-- Scale to fit within container -->
<div class="bg-auto"></div>       <!-- Original size -->

<!-- Background Position -->
<div class="bg-center"></div>     <!-- Center the image -->
<div class="bg-top"></div>        <!-- Align to top -->
<div class="bg-bottom"></div>     <!-- Align to bottom -->
<div class="bg-left"></div>       <!-- Align to left -->
<div class="bg-right"></div>      <!-- Align to right -->

<!-- Background Repeat -->
<div class="bg-repeat"></div>     <!-- Repeat image -->
<div class="bg-no-repeat"></div>  <!-- Don't repeat -->
<div class="bg-repeat-x"></div>   <!-- Repeat horizontally -->
<div class="bg-repeat-y"></div>   <!-- Repeat vertically -->

<!-- Background Attachment -->
<div class="bg-fixed"></div>      <!-- Fixed during scroll -->
<div class="bg-local"></div>      <!-- Scrolls with content -->
<div class="bg-scroll"></div>     <!-- Scrolls with page -->
```

### Border Utilities

```html
<!-- Border Width -->
<div class="border"></div>        <!-- 1px all sides -->
<div class="border-2"></div>      <!-- 2px all sides -->
<div class="border-4"></div>      <!-- 4px all sides -->
<div class="border-8"></div>      <!-- 8px all sides -->

<!-- Individual Sides -->
<div class="border-t"></div>      <!-- Top only -->
<div class="border-r"></div>      <!-- Right only -->
<div class="border-b"></div>      <!-- Bottom only -->
<div class="border-l"></div>      <!-- Left only -->
<div class="border-x"></div>      <!-- Left and right -->
<div class="border-y"></div>      <!-- Top and bottom -->

<!-- Border Colors -->
<div class="border-red-500"></div>
<div class="border-gray-300"></div>
<div class="border-current"></div> <!-- Inherits text color -->

<!-- Border Styles -->
<div class="border-solid"></div>   <!-- Solid line (default) -->
<div class="border-dashed"></div>  <!-- Dashed line -->
<div class="border-dotted"></div>  <!-- Dotted line -->
<div class="border-double"></div>  <!-- Double line -->
<div class="border-none"></div>    <!-- No border -->
```

### Border Radius

```html
<!-- All Corners -->
<div class="rounded-none"></div>   <!-- No rounding -->
<div class="rounded-sm"></div>     <!-- 2px -->
<div class="rounded"></div>        <!-- 4px -->
<div class="rounded-md"></div>     <!-- 6px -->
<div class="rounded-lg"></div>     <!-- 8px -->
<div class="rounded-xl"></div>     <!-- 12px -->
<div class="rounded-2xl"></div>    <!-- 16px -->
<div class="rounded-3xl"></div>    <!-- 24px -->
<div class="rounded-full"></div>   <!-- 50% (perfect circle/pill) -->

<!-- Individual Corners -->
<div class="rounded-t-lg"></div>   <!-- Top corners -->
<div class="rounded-r-lg"></div>   <!-- Right corners -->
<div class="rounded-b-lg"></div>   <!-- Bottom corners -->
<div class="rounded-l-lg"></div>   <!-- Left corners -->
<div class="rounded-tl-lg"></div>  <!-- Top-left corner -->
<div class="rounded-tr-lg"></div>  <!-- Top-right corner -->
<div class="rounded-br-lg"></div>  <!-- Bottom-right corner -->
<div class="rounded-bl-lg"></div>  <!-- Bottom-left corner -->
```

### Divide Utilities

```html
<!-- Horizontal Dividers (between children) -->
<div class="divide-y divide-gray-300">
  <div class="py-2">Item 1</div>
  <div class="py-2">Item 2</div>
  <div class="py-2">Item 3</div>
</div>

<!-- Vertical Dividers -->
<div class="flex divide-x divide-gray-300">
  <div class="px-4">Column 1</div>
  <div class="px-4">Column 2</div>
  <div class="px-4">Column 3</div>
</div>

<!-- Divide Styles -->
<div class="divide-y divide-dashed divide-gray-300">
<div class="divide-y divide-dotted divide-blue-500">
```

## Common Patterns

```html
<!-- Card with Shadow -->
<div class="bg-white border border-gray-200 rounded-lg shadow-sm p-6">
  Card content
</div>

<!-- Hero Section with Background Image -->
<section class="bg-cover bg-center bg-no-repeat bg-gray-900" 
         style="background-image: url('hero.jpg')">
  <div class="bg-black/50 min-h-screen flex items-center justify-center">
    <h1 class="text-white text-4xl font-bold">Hero Title</h1>
  </div>
</section>

<!-- Button with Hover States -->
<button class="bg-blue-500 hover:bg-blue-600 focus:bg-blue-700 
               border border-blue-600 rounded-md px-4 py-2 text-white
               transition-colors duration-200">
  Click me
</button>

<!-- Navigation with Dividers -->
<nav class="flex divide-x divide-gray-300">
  <a href="#" class="px-4 py-2 hover:bg-gray-100">Home</a>
  <a href="#" class="px-4 py-2 hover:bg-gray-100">About</a>
  <a href="#" class="px-4 py-2 hover:bg-gray-100">Contact</a>
</nav>

<!-- Gradient Background -->
<div class="bg-gradient-to-r from-purple-500 via-pink-500 to-red-500 
           text-white p-8 rounded-xl">
  Gradient background content
</div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use semantic color names (bg-red-500) | Use arbitrary values unless necessary |
| Combine hover/focus states for interactive elements | Forget to add transitions for smooth state changes |
| Use bg-cover for hero images | Use bg-contain for decorative backgrounds |
| Apply rounded-full for perfect circles | Use high border-radius values on rectangular elements |
| Use divide utilities for consistent spacing | Add borders manually to each child element |
| Use opacity modifiers (bg-blue-500/50) | Create custom CSS for simple transparency |

## Quick Fixes

- **Background image not showing** → Check if container has height and image path is correct
- **Border not visible** → Ensure border color is set (defaults to transparent)
- **Rounded corners not working** → Check for conflicting border-radius or overflow hidden
- **Gradient not smooth** → Add more color stops with via-* classes
- **Divide not working** → Ensure direct children and correct axis (divide-x vs divide-y)
- **Hover state not smooth** → Add transition-colors or transition-all class

## Gotchas

⚠️ **Background images require inline styles**: Tailwind can't dynamically generate background-image classes
⚠️ **Border color defaults to transparent**: Always specify border color or it won't be visible
⚠️ **Divide utilities only work on direct children**: Won't affect nested elements
⚠️ **bg-fixed may cause performance issues**: Use sparingly on mobile devices
⚠️ **Gradient directions**: to-r = left to right, to-br = top-left to bottom-right

## Interactive States

```html
<!-- Background Hover States -->
<div class="bg-gray-100 hover:bg-gray-200 active:bg-gray-300"></div>

<!-- Border Hover States -->
<div class="border border-gray-300 hover:border-blue-500 focus:border-blue-600"></div>

<!-- Combined States -->
<button class="bg-white border border-gray-300 rounded-md
               hover:bg-gray-50 hover:border-gray-400
               focus:bg-gray-50 focus:border-blue-500 focus:outline-none
               active:bg-gray-100
               transition-all duration-200">
  Interactive Button
</button>
```

## Responsive Breakpoints

```html
<!-- Responsive Backgrounds -->
<div class="bg-red-500 md:bg-blue-500 lg:bg-green-500"></div>

<!-- Responsive Borders -->
<div class="border md:border-2 lg:border-4"></div>

<!-- Responsive Border Radius -->
<div class="rounded md:rounded-lg lg:rounded-xl"></div>
```

## Checklist

- [ ] Set background colors with appropriate opacity for overlays
- [ ] Use bg-cover for hero images and bg-contain for icons/logos  
- [ ] Add hover and focus states for interactive elements
- [ ] Include transition classes for smooth state changes
- [ ] Test background attachment (bg-fixed) on mobile devices
- [ ] Ensure border colors are specified (not just border width)
- [ ] Use divide utilities instead of individual borders for lists
- [ ] Consider responsive breakpoints for different screen sizes
- [ ] Validate gradient directions match design requirements
- [ ] Test rounded corners on different content sizes