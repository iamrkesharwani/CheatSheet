# CSS Organization & Architecture `v CSS3+`

## Essentials

```css
/* CSS Variables for consistency */
:root {
  --primary-color: #007bff;
  --text-color: #333;
  --spacing-unit: 1rem;
}

/* BEM naming convention */
.card {}                    /* Block */
.card__title {}            /* Element */
.card--featured {}         /* Modifier */

/* Modern CSS reset */
*, *::before, *::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: system-ui, sans-serif;
}
```

**Key Concepts**: CSS Variables | BEM Methodology | Reset/Normalize | Separation of Concerns
**When to use**: Maintain scalable, readable, and maintainable CSS across large projects

## Syntax Reference

### CSS Variables (Custom Properties)

```css
/* Define variables in :root for global scope */
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  
  --font-size-base: 1rem;
  --font-size-lg: 1.25rem;
  --font-size-sm: 0.875rem;
  
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;
  
  --border-radius: 0.375rem;
  --box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Using variables */
.button {
  background-color: var(--primary-color);
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--border-radius);
  font-size: var(--font-size-base);
}

/* Variables with fallbacks */
.element {
  color: var(--text-color, #333);
  margin: var(--spacing-md, 1rem);
}

/* Contextual variables */
.dark-theme {
  --primary-color: #0d6efd;
  --text-color: #ffffff;
  --bg-color: #1a1a1a;
}
```

### BEM Naming Convention

```css
/* Block: Standalone component */
.header {}
.navigation {}
.card {}
.button {}

/* Element: Part of a block */
.header__logo {}
.header__nav {}
.navigation__item {}
.navigation__link {}
.card__title {}
.card__content {}
.card__footer {}
.button__icon {}

/* Modifier: Variation of block or element */
.header--sticky {}
.navigation--mobile {}
.card--featured {}
.card--large {}
.button--primary {}
.button--disabled {}
.card__title--center {}

/* Complex example */
.product-card {}                    /* Block */
.product-card__image {}            /* Element */
.product-card__title {}            /* Element */
.product-card__price {}            /* Element */
.product-card--featured {}         /* Block modifier */
.product-card--sold-out {}         /* Block modifier */
.product-card__price--discounted {} /* Element modifier */
```

### CSS Reset vs Normalize

```css
/* Modern CSS Reset (recommended) */
*, *::before, *::after {
  box-sizing: border-box;
}

* {
  margin: 0;
}

html, body {
  height: 100%;
}

body {
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

img, picture, video, canvas, svg {
  display: block;
  max-width: 100%;
}

input, button, textarea, select {
  font: inherit;
}

p, h1, h2, h3, h4, h5, h6 {
  overflow-wrap: break-word;
}

/* Alternative: Normalize.css approach */
html {
  line-height: 1.15;
  -webkit-text-size-adjust: 100%;
}

body {
  margin: 0;
}

main {
  display: block;
}

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}

button, input, optgroup, select, textarea {
  font-family: inherit;
  font-size: 100%;
  line-height: 1.15;
  margin: 0;
}
```

## Project Structure Patterns

```css
/* File organization approach */

/* 1. Base styles */
/* base/_reset.css */
/* base/_typography.css */
/* base/_forms.css */

/* 2. Layout components */
/* layout/_header.css */
/* layout/_footer.css */
/* layout/_sidebar.css */
/* layout/_grid.css */

/* 3. UI components */
/* components/_buttons.css */
/* components/_cards.css */
/* components/_modals.css */

/* 4. Utilities */
/* utilities/_spacing.css */
/* utilities/_colors.css */
/* utilities/_text.css */

/* 5. Pages/sections */
/* pages/_home.css */
/* pages/_contact.css */
```

### Separation of Concerns

```html
<!-- ❌ Poor separation - styling in HTML -->
<div style="color: red; margin: 20px; display: flex;">
  <span style="font-weight: bold;">Title</span>
</div>

<!-- ❌ Poor separation - presentation classes -->
<div class="red-text margin-20 flex-display">
  <span class="bold-text">Title</span>
</div>

<!-- ✅ Good separation - semantic classes -->
<div class="alert alert--error">
  <span class="alert__title">Title</span>
</div>
```

```css
/* ❌ Poor separation - HTML structure in CSS */
.header div div span {
  color: red;
}

div.content > p:first-child {
  font-weight: bold;
}

/* ✅ Good separation - semantic selectors */
.alert--error {
  color: var(--error-color);
  border: 1px solid var(--error-border);
}

.article__intro {
  font-weight: bold;
  font-size: var(--font-size-lg);
}
```

## Design System with Variables

```css
/* Color system */
:root {
  /* Brand colors */
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-900: #1e3a8a;
  
  /* Semantic colors */
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-info: #06b6d4;
  
  /* Neutral colors */
  --color-gray-50: #f9fafb;
  --color-gray-100: #f3f4f6;
  --color-gray-500: #6b7280;
  --color-gray-900: #111827;
}

/* Typography scale */
:root {
  --font-family-sans: system-ui, -apple-system, sans-serif;
  --font-family-mono: 'SF Mono', Monaco, 'Cascadia Code', monospace;
  
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;
  --font-size-3xl: 1.875rem;
  
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
  
  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;
}

/* Spacing scale */
:root {
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  --space-12: 3rem;    /* 48px */
  --space-16: 4rem;    /* 64px */
}
```

## BEM in Practice

```css
/* News article component */
.article {}

.article__header {}
.article__title {}
.article__meta {}
.article__author {}
.article__date {}
.article__content {}
.article__image {}
.article__caption {}
.article__footer {}
.article__tags {}
.article__share {}

/* Modifiers */
.article--featured {
  border: 2px solid var(--color-primary-500);
}

.article--breaking {
  background: var(--color-error);
  color: white;
}

.article__title--large {
  font-size: var(--font-size-3xl);
}

.article__image--full-width {
  width: 100vw;
  margin-left: calc(-50vw + 50%);
}

/* Navigation component */
.nav {}
.nav__list {}
.nav__item {}
.nav__link {}

.nav--horizontal .nav__list {
  display: flex;
}

.nav--vertical .nav__list {
  flex-direction: column;
}

.nav__link--active {
  color: var(--color-primary-500);
  font-weight: var(--font-weight-semibold);
}
```

## Do's & Don'ts

| ✅ Do                                    | ❌ Don't                               |
| ---------------------------------------- | -------------------------------------- |
| Use CSS variables for consistent values  | Hardcode colors and sizes everywhere   |
| Follow BEM naming consistently          | Mix naming conventions in same project |
| Keep HTML semantic and CSS presentational | Put styling logic in HTML classes    |
| Use meaningful class names              | Use abbreviations or cryptic names     |
| Group related styles together          | Scatter component styles across files  |

## Quick Fixes

- **Variable not updating** → Check if it's defined in correct scope (:root for global)
- **BEM class names too long** → Consider if component is too complex, break it down
- **Styles conflicting** → Use more specific BEM selectors instead of !important
- **Reset breaking design** → Use normalize.css instead of aggressive reset
- **HTML and CSS tightly coupled** → Refactor to use semantic class names

## Gotchas

⚠️ **Warning**: CSS variables are case-sensitive: `--Main-Color` ≠ `--main-color`
⚠️ **Warning**: BEM modifiers should modify the base block/element, not replace it entirely
⚠️ **Warning**: CSS resets can override useful browser defaults - use normalize for safer approach
⚠️ **Warning**: Overly nested BEM structures indicate component should be broken down

## Advanced Organization

```css
/* Utility-first approach (when needed) */
.u-margin-bottom-lg { margin-bottom: var(--space-8); }
.u-text-center { text-align: center; }
.u-visually-hidden { 
  position: absolute;
  clip: rect(0 0 0 0);
  width: 1px;
  height: 1px;
  overflow: hidden;
}

/* Component variants with CSS variables */
.button {
  background: var(--button-bg, var(--color-primary-500));
  color: var(--button-color, white);
  border: var(--button-border, none);
}

.button--secondary {
  --button-bg: transparent;
  --button-color: var(--color-primary-500);
  --button-border: 1px solid var(--color-primary-500);
}

/* Theme switching */
[data-theme="dark"] {
  --color-bg: var(--color-gray-900);
  --color-text: var(--color-gray-50);
}

[data-theme="light"] {
  --color-bg: var(--color-gray-50);
  --color-text: var(--color-gray-900);
}
```

## CSS Architecture Example

```css
/* Complete component example */
.card {
  background: var(--color-white);
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
  overflow: hidden;
  transition: transform 0.2s ease;
}

.card:hover {
  transform: translateY(-2px);
}

.card__image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card__content {
  padding: var(--space-4);
}

.card__title {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-semibold);
  margin-bottom: var(--space-2);
  color: var(--color-gray-900);
}

.card__description {
  color: var(--color-gray-600);
  line-height: var(--line-height-relaxed);
  margin-bottom: var(--space-4);
}

.card__actions {
  display: flex;
  gap: var(--space-2);
  justify-content: flex-end;
}

.card--featured {
  border: 2px solid var(--color-primary-500);
}

.card--horizontal {
  display: flex;
}

.card--horizontal .card__image {
  width: 200px;
  height: auto;
}
```

## Checklist

- [ ] Defined CSS variables for colors, spacing, and typography in :root
- [ ] Implemented consistent BEM naming convention across components
- [ ] Added appropriate CSS reset or normalize stylesheet
- [ ] Separated presentation (CSS) from structure (HTML) concerns
- [ ] Organized CSS files by purpose (base, components, utilities)
- [ ] Used semantic class names that describe purpose, not appearance
- [ ] Created reusable component patterns with modifiers