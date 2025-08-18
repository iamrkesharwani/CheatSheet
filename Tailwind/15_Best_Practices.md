# Tailwind CSS Best Practices `v3.4.1`

## Essentials

```html
<!-- Component-based approach -->
<button class="btn-primary">Primary Button</button>
<div class="card shadow-lg">Content</div>

<!-- Utility-first with semantic HTML -->
<article class="prose max-w-none">
  <h1 class="text-3xl font-bold mb-4">Article Title</h1>
  <p class="text-gray-700 leading-relaxed">Content...</p>
</article>
```

**Key Concepts**: Component Classes | Utility Composition | Semantic HTML | Build Optimization
**When to use**: Rapid UI development with consistent design systems and optimized production builds

## Syntax Reference

### Basic Structure

```css
/* tailwind.config.js - Essential configuration */
module.exports = {
  content: ['./src/**/*.{html,js,jsx,ts,tsx}'],
  theme: {
    extend: {
      colors: { brand: '#3B82F6' }
    }
  },
  plugins: []
}
```

### Common Patterns

```css
/* Pattern 1: Component extraction with @apply */
.btn-primary {
  @apply bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors;
}

/* Pattern 2: Responsive design */
.responsive-grid {
  @apply grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6;
}

/* Pattern 3: State variants */
.input-field {
  @apply border border-gray-300 px-3 py-2 rounded focus:ring-2 focus:ring-blue-500 focus:border-transparent;
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use component classes for repeated patterns | Chain 10+ utility classes inline |
| Configure purge/content paths correctly | Import entire Tailwind in production |
| Use semantic HTML elements first | Rely only on divs with utility classes |
| Extract patterns with @apply in CSS | Inline @apply in HTML class attributes |
| Use Prettier plugin for class ordering | Manually sort utility classes |
| Test responsive breakpoints thoroughly | Assume mobile-first works everywhere |

## Quick Fixes

- **Classes not applying** → Check content paths in tailwind.config.js
- **Large bundle size** → Enable purging with correct file paths
- **Responsive not working** → Use mobile-first breakpoints (sm:, md:, lg:)
- **Custom styles overridden** → Use !important or increase specificity
- **Hover states flashing** → Add transition classes for smooth animations
- **Build failing** → Ensure PostCSS and Autoprefixer are configured

## Gotchas

⚠️ **Purging removes used classes**: Dynamic class names need safelist configuration or use complete class names
⚠️ **Mobile-first breakpoints**: `md:text-lg` applies from medium screens UP, not just medium screens
⚠️ **Specificity conflicts**: Utility classes have same specificity - source order matters in conflicts
⚠️ **@apply limitations**: Cannot use with pseudo-selectors or responsive variants directly

## Checklist

- [ ] Configure content paths for all template files
- [ ] Enable purging/optimization for production builds
- [ ] Use component classes for patterns used 3+ times
- [ ] Test responsive design on actual devices
- [ ] Install Prettier plugin for automatic class sorting
- [ ] Validate HTML semantics before adding utility classes
- [ ] Set up proper development/production build pipeline
- [ ] Document custom component classes for team consistency