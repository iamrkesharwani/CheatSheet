# CSS Pseudo-Classes & Pseudo-Elements `v CSS3+`

## Essentials

```css
/* Interactive states */
button:hover { background: #007bff; }
input:focus { border: 2px solid blue; }
a:active { color: red; }

/* Child selectors */
li:first-child { font-weight: bold; }
tr:nth-child(even) { background: #f5f5f5; }

/* Content creation */
.tooltip::before { content: "★"; }
input::placeholder { color: #999; }
```

**Key Concepts**: Interactive States | Child Selection | Content Generation
**When to use**: Style elements based on user interaction, position, or to add decorative content

## Syntax Reference

### Basic Structure

```css
/* Pseudo-classes (single colon) - select existing elements */
selector:pseudo-class { property: value; }

/* Pseudo-elements (double colon) - create virtual elements */
selector::pseudo-element { property: value; }
```

### Interactive Pseudo-Classes

```css
/* Hover effects */
.button:hover { 
  background: #0056b3; 
  transform: translateY(-2px); 
}

/* Focus states for accessibility */
input:focus, textarea:focus { 
  outline: 2px solid #007bff; 
  outline-offset: 2px; 
}

/* Active state (during click) */
.button:active { 
  transform: scale(0.98); 
}

/* Visited links */
a:visited { color: #6f42c1; }
```

### Structural Pseudo-Classes

```css
/* First and last children */
.nav-item:first-child { margin-left: 0; }
.nav-item:last-child { margin-right: 0; }

/* Nth-child patterns */
tr:nth-child(odd) { background: #f8f9fa; }  /* 1, 3, 5... */
tr:nth-child(even) { background: white; }   /* 2, 4, 6... */
.grid-item:nth-child(3n) { margin-right: 0; }  /* Every 3rd */
.card:nth-child(n+4) { display: none; }    /* 4th and beyond */

/* Type-based selection */
p:first-of-type { font-size: 1.2em; }
img:last-of-type { margin-bottom: 0; }
```

### Logic Pseudo-Classes

```css
/* Not selector - exclude elements */
input:not([type="submit"]) { margin-bottom: 1rem; }
.btn:not(.btn-primary) { border: 1px solid #ccc; }

/* Empty elements */
.message:empty { display: none; }
.card:empty::after { content: "No content available"; }
```

### Content Pseudo-Elements

```css
/* Before and after content */
.icon::before { 
  content: "→"; 
  margin-right: 0.5rem; 
}

.quote::after { 
  content: """; 
  font-size: 1.5em; 
}

/* Decorative elements */
.section-title::before {
  content: "";
  display: block;
  width: 50px;
  height: 3px;
  background: #007bff;
  margin-bottom: 1rem;
}
```

### Form Pseudo-Elements

```css
/* Placeholder styling */
input::placeholder { 
  color: #6c757d; 
  font-style: italic; 
}

/* File input button */
input[type="file"]::file-selector-button {
  padding: 0.5rem 1rem;
  border: none;
  background: #007bff;
  color: white;
  border-radius: 4px;
}
```

## Do's & Don'ts

| ✅ Do                                    | ❌ Don't                                |
| ---------------------------------------- | --------------------------------------- |
| Use single colon for pseudo-classes     | Mix up :: and : syntax                 |
| Include focus states for accessibility  | Forget keyboard users                   |
| Test hover on mobile (converts to tap)  | Assume hover works on touch devices    |
| Use ::before/::after for decoration     | Use pseudo-elements for essential text  |

## Quick Fixes

- **::before content not showing** → Add `content: "";` (required property)
- **nth-child not working** → Check if parent has other element types mixed in
- **Hover sticking on mobile** → Add `@media (hover: hover)` wrapper
- **Focus outline missing** → Never use `outline: none` without replacement
- **Pseudo-element positioning issues** → Set `position: relative` on parent

## Gotchas

⚠️ **Warning**: `::before` and `::after` require `content` property, even if empty (`content: "";`)
⚠️ **Warning**: `:nth-child()` counts ALL siblings, not just same element type
⚠️ **Warning**: Pseudo-elements can't be applied to void elements (img, input, br)
⚠️ **Warning**: `:hover` on mobile triggers on tap and stays until tapping elsewhere

## Advanced Patterns

```css
/* Combining pseudo-classes */
.nav-link:hover:not(.active) { opacity: 0.7; }

/* Complex nth-child formulas */
.item:nth-child(3n+1) { /* 1st, 4th, 7th... */ }
.item:nth-child(-n+3) { /* First 3 items */ }

/* Layered pseudo-elements */
.card::before, .card::after {
  content: "";
  position: absolute;
  inset: 0;
}
.card::before { background: linear-gradient(...); }
.card::after { backdrop-filter: blur(10px); }
```

## Checklist

- [ ] Added focus styles for all interactive elements
- [ ] Tested hover effects on both desktop and mobile
- [ ] Used semantic pseudo-classes over complex selectors where possible
- [ ] Included `content` property for all `::before` and `::after` elements
- [ ] Verified pseudo-elements work with target element types