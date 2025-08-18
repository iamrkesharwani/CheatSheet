# Tailwind CSS Plugins & Customization `v3.4`

## Essentials

```bash
# Install official plugins
npm install @tailwindcss/forms @tailwindcss/typography @tailwindcss/aspect-ratio

# Add to tailwind.config.js
plugins: [
  require('@tailwindcss/forms'),
  require('@tailwindcss/typography'),
  require('@tailwindcss/aspect-ratio'),
]
```

**Key Concepts**: Official Plugins | Custom Utilities | Layer Organization | Component Extraction
**When to use**: Enhanced form styling, rich text content, responsive media, and custom utility creation

## Syntax Reference

### Plugin Installation & Setup

```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{html,js}'],
  plugins: [
    // Official plugins
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
    
    // Custom plugin
    function({ addUtilities, addComponents, theme }) {
      addUtilities({
        '.scrollbar-hide': {
          'scrollbar-width': 'none',
          '&::-webkit-scrollbar': { display: 'none' }
        }
      })
    }
  ]
}
```

### Forms Plugin Usage

```html
<!-- Automatic form styling -->
<input type="text" class="rounded-md border-gray-300 focus:border-blue-500">
<select class="rounded-md border-gray-300">
  <option>Option 1</option>
</select>
<textarea class="rounded-md border-gray-300"></textarea>

<!-- Checkbox and radio styling -->
<input type="checkbox" class="rounded border-gray-300 text-blue-600">
<input type="radio" class="border-gray-300 text-blue-600">
```

### Typography Plugin Usage

```html
<!-- Rich text content -->
<article class="prose lg:prose-xl">
  <h1>Article Title</h1>
  <p>Automatically styled paragraph with proper spacing...</p>
  <blockquote>Styled blockquote</blockquote>
</article>

<!-- Dark mode prose -->
<div class="prose dark:prose-invert">
  <h2>Dark mode content</h2>
</div>

<!-- Custom prose colors -->
<div class="prose prose-slate prose-lg">
  <p>Slate-themed large prose</p>
</div>
```

### Aspect Ratio Plugin Usage

```html
<!-- 16:9 video embed -->
<div class="aspect-w-16 aspect-h-9">
  <iframe src="video-url"></iframe>
</div>

<!-- Square image -->
<div class="aspect-w-1 aspect-h-1">
  <img src="image.jpg" class="object-cover">
</div>

<!-- Custom ratios -->
<div class="aspect-w-4 aspect-h-3">
  <div class="bg-gray-200"></div>
</div>
```

## Common Patterns

### Using @apply for Component Extraction

```css
/* styles.css */
@layer components {
  .btn-primary {
    @apply px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 focus:ring-2 focus:ring-blue-300;
  }
  
  .card {
    @apply bg-white rounded-lg shadow-md p-6;
  }
  
  .form-input {
    @apply block w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500;
  }
}
```

### Custom Layer Organization

```css
/* Layer order: base → components → utilities */

@layer base {
  /* Base styles for HTML elements */
  h1 { @apply text-2xl font-bold; }
  h2 { @apply text-xl font-semibold; }
  p { @apply text-gray-700 leading-relaxed; }
}

@layer components {
  /* Reusable component patterns */
  .alert {
    @apply px-4 py-3 rounded border-l-4;
  }
  .alert-error {
    @apply alert border-red-500 bg-red-50 text-red-700;
  }
}

@layer utilities {
  /* Custom utility classes */
  .text-shadow {
    text-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .scrollbar-thin {
    scrollbar-width: thin;
  }
}
```

### Creating Custom Plugins

```javascript
// tailwind.config.js
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    // Simple utility plugin
    plugin(function({ addUtilities }) {
      addUtilities({
        '.rotate-y-180': {
          transform: 'rotateY(180deg)',
        },
        '.preserve-3d': {
          'transform-style': 'preserve-3d',
        },
      })
    }),
    
    // Plugin with theme values
    plugin(function({ addUtilities, theme }) {
      const colors = theme('colors')
      const textShadowUtilities = Object.keys(colors).reduce((acc, key) => {
        if (typeof colors[key] === 'string') {
          acc[`.text-shadow-${key}`] = {
            'text-shadow': `0 2px 4px ${colors[key]}`,
          }
        }
        return acc
      }, {})
      
      addUtilities(textShadowUtilities)
    }),
    
    // Plugin with components and variants
    plugin(function({ addComponents, addVariant }) {
      addComponents({
        '.glass': {
          background: 'rgba(255, 255, 255, 0.1)',
          backdropFilter: 'blur(10px)',
          border: '1px solid rgba(255, 255, 255, 0.2)',
        }
      })
      
      addVariant('hocus', ['&:hover', '&:focus'])
    })
  ]
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use @apply sparingly for truly reusable components | Overuse @apply - defeats utility-first purpose |
| Organize custom CSS using @layer directives | Write CSS without layer organization |
| Create plugins for project-specific utilities | Create plugins for one-off styles |
| Use forms plugin for consistent form styling | Style forms manually when plugin exists |
| Use typography plugin for content-heavy pages | Apply prose classes to UI components |

## Quick Fixes

- **Forms not styled** → Install @tailwindcss/forms plugin and restart build
- **Prose content unstyled** → Add prose class and @tailwindcss/typography plugin  
- **@apply not working** → Ensure styles are in @layer components or utilities
- **Custom utilities not generating** → Check plugin syntax and restart build process
- **Aspect ratio not responsive** → Use aspect-w-X aspect-h-Y classes, not aspect-ratio property

## Gotchas

⚠️ **@apply extraction**: Only works with utility classes, not arbitrary values or CSS properties
⚠️ **Plugin loading**: Plugins run in order - later plugins can override earlier ones
⚠️ **Forms plugin**: Automatically styles ALL form elements - may conflict with existing styles
⚠️ **Typography plugin**: Only style content areas, not UI components (buttons, navs, etc.)
⚠️ **Layer importance**: Utilities layer always wins, even over !important in components layer

## Checklist

- [ ] Install required plugins via npm/yarn
- [ ] Add plugins to tailwind.config.js plugins array
- [ ] Restart build process after plugin installation
- [ ] Use @layer directives for custom CSS organization
- [ ] Test @apply extractions work with all utility combinations
- [ ] Verify custom plugins generate expected classes
- [ ] Check prose classes only on content, not UI elements
- [ ] Ensure form plugins don't conflict with existing styles