# Tailwind CSS Interactivity `v3.4.1`

## Essentials

```html
<!-- Cursor states -->
<button class="cursor-pointer hover:bg-blue-600">Click me</button>
<button class="cursor-not-allowed opacity-50" disabled>Disabled</button>

<!-- Focus rings -->
<input class="focus:ring-2 focus:ring-blue-500 focus:outline-none" />

<!-- Text selection -->
<p class="select-none">Can't select this text</p>
```

**Key Concepts**: Cursor Control | User Selection | Focus Management | Pointer Events
**When to use**: Enhancing user interaction feedback and controlling element behavior

## Syntax Reference

### Cursor Control

```html
<!-- Pointer cursors -->
<div class="cursor-pointer">Clickable element</div>
<div class="cursor-not-allowed">Disabled element</div>
<div class="cursor-default">Default cursor</div>
<div class="cursor-wait">Loading state</div>
<div class="cursor-help">Help tooltip trigger</div>

<!-- Interactive cursors -->
<div class="cursor-grab">Draggable item</div>
<div class="cursor-grabbing">Currently dragging</div>
<div class="cursor-move">Moveable element</div>
```

### Pointer Events

```html
<!-- Disable all pointer events -->
<div class="pointer-events-none">
  <button>This button won't work</button>
</div>

<!-- Re-enable pointer events on child -->
<div class="pointer-events-none">
  <button class="pointer-events-auto">This button works</button>
</div>
```

### Text Selection

```html
<!-- Prevent text selection -->
<div class="select-none">Unselectable UI text</div>

<!-- Allow text selection -->
<p class="select-text">Selectable content</p>
<code class="select-all">Click to select all</code>

<!-- Default browser behavior -->
<p class="select-auto">Normal selection</p>
```

### Resize Control

```html
<!-- Resizable elements -->
<textarea class="resize">Both directions</textarea>
<textarea class="resize-x">Horizontal only</textarea>
<textarea class="resize-y">Vertical only</textarea>
<textarea class="resize-none">No resize</textarea>
```

### Focus & Ring Utilities

```html
<!-- Focus rings -->
<input class="focus:ring-2 focus:ring-blue-500" />
<input class="focus:ring-4 focus:ring-red-300 focus:ring-opacity-50" />

<!-- Ring offset -->
<button class="ring-2 ring-blue-500 ring-offset-2">Always visible ring</button>

<!-- Focus outline control -->
<input class="focus:outline-none focus:ring-2 focus:ring-purple-500" />
<button class="outline-2 outline-dashed outline-gray-400">Custom outline</button>
```

## Common Patterns

```html
<!-- Interactive button with states -->
<button class="cursor-pointer focus:ring-2 focus:ring-blue-500 focus:outline-none 
               hover:bg-blue-600 disabled:cursor-not-allowed disabled:opacity-50">
  Submit
</button>

<!-- Card with hover interaction -->
<div class="cursor-pointer select-none hover:shadow-lg transition-shadow">
  <h3 class="select-text">Card Title</h3>
  <p class="select-text">Selectable content</p>
</div>

<!-- Draggable item -->
<div class="cursor-grab active:cursor-grabbing select-none 
           hover:shadow-md transition-shadow">
  Drag me around
</div>

<!-- Modal overlay pattern -->
<div class="pointer-events-none fixed inset-0">
  <div class="pointer-events-auto bg-white rounded-lg p-6">
    Modal content
  </div>
</div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `cursor-pointer` on interactive elements | Use `cursor-pointer` on non-clickable text |
| Combine `focus:ring` with `focus:outline-none` | Remove focus indicators without replacement |
| Use `select-none` on UI labels and buttons | Use `select-none` on readable content |
| Apply `pointer-events-none` to overlay backgrounds | Use `pointer-events-none` without `pointer-events-auto` fallback |
| Use `resize-none` on textareas in forms | Remove resize on large text inputs |

## Quick Fixes

- **Button not showing pointer cursor** → Add `cursor-pointer` class
- **Can't click through overlay** → Use `pointer-events-none` on overlay, `pointer-events-auto` on content
- **Unwanted text selection on UI elements** → Add `select-none` to buttons and labels
- **Missing focus indicators** → Add `focus:ring-2 focus:ring-color-500` to inputs
- **Textarea resize issues** → Use `resize-none` or specific `resize-x`/`resize-y`

## Gotchas

⚠️ **Focus rings don't show**: Browsers hide focus rings on mouse clicks by default - this is normal behavior

⚠️ **Pointer events inheritance**: `pointer-events-none` affects all children unless explicitly overridden

⚠️ **Select-none breaks accessibility**: Screen readers may still need to access the text content

⚠️ **Ring offset needs background**: `ring-offset` uses the background color of parent element

## Checklist

- [ ] Interactive elements have appropriate cursor states
- [ ] Focus indicators are visible and accessible
- [ ] Text selection is controlled appropriately for content type
- [ ] Pointer events allow proper interaction flow
- [ ] Resize behavior matches user expectations
- [ ] Ring styles provide clear visual feedback