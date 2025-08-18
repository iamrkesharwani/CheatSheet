# Tailwind CSS Typography `v3.4.0`

## Essentials

```html
<!-- Core text styling -->
<p class="text-lg font-medium text-gray-900">Heading text</p>
<p class="text-base font-normal text-gray-700 leading-relaxed">Body text with comfortable line height</p>
<span class="text-sm text-blue-600 underline hover:text-blue-800">Link text</span>
```

**Key Concepts**: Text Size | Font Weight | Line Height | Color
**When to use**: All text styling in web interfaces - headings, body text, links, and emphasis

## Syntax Reference

### Text Sizes

```html
<!-- Size scale: xs, sm, base, lg, xl, 2xl, 3xl, 4xl, 5xl, 6xl, 7xl, 8xl, 9xl -->
<h1 class="text-4xl">Main Heading</h1>
<h2 class="text-2xl">Section Heading</h2>
<p class="text-base">Body text (16px default)</p>
<small class="text-sm">Small text (14px)</small>
<span class="text-xs">Tiny text (12px)</span>
```

### Font Families & Weights

```html
<!-- Font families -->
<p class="font-sans">Default sans-serif</p>
<p class="font-serif">Serif typeface</p>
<code class="font-mono">Monospace code</code>

<!-- Font weights: thin, light, normal, medium, semibold, bold, extrabold, black -->
<p class="font-light">Light text (300)</p>
<p class="font-normal">Normal text (400)</p>
<p class="font-medium">Medium text (500)</p>
<p class="font-bold">Bold text (700)</p>
```

### Line Height & Spacing

```html
<!-- Line height: leading-none, tight, snug, normal, relaxed, loose -->
<p class="leading-tight">Tight line spacing (1.25)</p>
<p class="leading-normal">Default spacing (1.5)</p>
<p class="leading-relaxed">Relaxed spacing (1.625)</p>

<!-- Letter spacing: tracking-tighter, tight, normal, wide, wider, widest -->
<h1 class="tracking-tight">Tight letter spacing</h1>
<p class="tracking-normal">Normal spacing</p>
<span class="tracking-wide">Wide letter spacing</span>
```

## Common Patterns

```html
<!-- Pattern 1: Article heading -->
<h1 class="text-3xl font-bold text-gray-900 leading-tight">
  Article Title
</h1>

<!-- Pattern 2: Body paragraph -->
<p class="text-base text-gray-700 leading-relaxed">
  Body content with comfortable reading spacing
</p>

<!-- Pattern 3: Interactive link -->
<a href="#" class="text-blue-600 underline hover:text-blue-800 hover:no-underline">
  Click here
</a>

<!-- Pattern 4: Emphasized text -->
<span class="font-semibold text-gray-900">Important information</span>

<!-- Pattern 5: Code snippet -->
<code class="font-mono text-sm bg-gray-100 px-2 py-1 rounded">
  const variable = 'value'
</code>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `text-base` for body text (maintains 16px) | Use `text-sm` for main content (too small) |
| Combine `leading-relaxed` with body text | Use tight leading on long paragraphs |
| Use `font-medium` for subtle emphasis | Overuse `font-bold` everywhere |
| Apply `hover:` states to interactive text | Style static text with hover effects |
| Use semantic color names like `text-gray-700` | Use arbitrary color values in classes |

## Quick Fixes

- **Text too small to read** → Change `text-sm` to `text-base` or larger
- **Lines too cramped** → Add `leading-relaxed` or `leading-loose`
- **Text lacks hierarchy** → Use different `text-*` sizes and `font-*` weights
- **Links don't look clickable** → Add `text-blue-600 underline hover:text-blue-800`
- **Code hard to distinguish** → Use `font-mono bg-gray-100 px-2 py-1 rounded`

## Gotchas

⚠️ **Warning**: `text-base` is 16px, not 14px - don't assume it's "small"
⚠️ **Warning**: `truncate` only works on single lines, use `line-clamp-*` for multiple lines
⚠️ **Warning**: Color contrast - ensure sufficient contrast ratios for accessibility
⚠️ **Warning**: `leading-*` affects the entire line height, not just spacing between lines

## Text Decoration & Alignment

```html
<!-- Text decoration -->
<p class="underline">Underlined text</p>
<p class="line-through">Strikethrough text</p>
<p class="no-underline">Remove underline</p>

<!-- Text alignment -->
<p class="text-left">Left aligned</p>
<p class="text-center">Center aligned</p>
<p class="text-right">Right aligned</p>
<p class="text-justify">Justified text</p>

<!-- Text overflow -->
<p class="truncate">This text will be truncated with ellipsis...</p>
<p class="line-clamp-3">This text will be clamped to 3 lines with ellipsis</p>
```

## Color Examples

```html
<!-- Common text colors -->
<p class="text-gray-900">Primary text (dark)</p>
<p class="text-gray-700">Secondary text</p>
<p class="text-gray-500">Muted text</p>
<p class="text-blue-600">Link color</p>
<p class="text-red-600">Error text</p>
<p class="text-green-600">Success text</p>

<!-- With hover states -->
<a class="text-blue-600 hover:text-blue-800">Interactive link</a>
<button class="text-gray-700 hover:text-gray-900">Button text</button>
```

## Checklist

- [ ] Use appropriate text sizes for hierarchy (text-3xl for h1, text-xl for h2, etc.)
- [ ] Apply comfortable line height (leading-relaxed) to body text
- [ ] Set proper font weights (font-medium for emphasis, font-bold sparingly)
- [ ] Ensure sufficient color contrast for accessibility
- [ ] Add hover states to interactive text elements
- [ ] Use font-mono for code and technical content
- [ ] Test text truncation and line clamping on different screen sizes
- [ ] Choose semantic color names over arbitrary values