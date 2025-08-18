# CSS Selectors `v CSS3`

## Essentials

```css
/* Basic targeting - the foundation of CSS */
div { color: blue; }           /* All div elements */
.card { padding: 20px; }       /* Elements with class="card" */
#header { background: #333; }  /* Element with id="header" */

/* Combining selectors for precise targeting */
nav ul li { list-style: none; }              /* li inside ul inside nav */
.sidebar > .widget { margin-bottom: 15px; }  /* Direct .widget children of .sidebar */
h2 + p { margin-top: 0; }                    /* First paragraph after any h2 */
```

**Key Concepts**: Specificity | Combinators | Attribute Matching | Cascading Rules
**When to use**: Target HTML elements for styling with maximum precision while avoiding conflicts and maintaining clean, maintainable CSS

## Understanding Selector Types

### Basic Selectors - The Building Blocks

**Element Selectors** - Target all elements of a specific HTML tag
```css
/* Style all paragraphs */
p { 
    font-size: 16px; 
    line-height: 1.5; 
}

/* Style all headings */
h1 { 
    font-weight: bold; 
    margin-bottom: 20px; 
}

/* Useful for setting base styles */
body { font-family: Arial, sans-serif; }
a { text-decoration: none; }
img { max-width: 100%; height: auto; }
```

**Class Selectors** - Target elements with specific class attributes (most commonly used)
```css
/* Reusable component styles */
.button { 
    background: #007bff; 
    color: white; 
    padding: 10px 20px; 
    border: none; 
    border-radius: 4px; 
}

.card {
    background: white;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 20px;
}

/* Modifier classes for variations */
.button--large { padding: 15px 30px; font-size: 18px; }
.button--danger { background: #dc3545; }
.card--featured { border-color: #ffc107; background: #fff9c4; }
```

**ID Selectors** - Target single unique element (use sparingly for styling)
```css
/* Layout and unique page sections */
#header { 
    position: fixed; 
    top: 0; 
    width: 100%; 
    z-index: 1000; 
}

#main-content { 
    max-width: 1200px; 
    margin: 0 auto; 
    padding: 20px; 
}

/* Better for JavaScript hooks than styling */
#search-form { /* JavaScript will target this */ }
```

**Universal Selector** - Targets all elements (use carefully)
```css
/* CSS reset/normalize - common use case */
* { 
    box-sizing: border-box; 
    margin: 0; 
    padding: 0; 
}

/* Within specific contexts */
.reset-container * { 
    margin: 0; 
    padding: 0; 
}
```

**Grouping Selectors** - Apply same styles to multiple selectors
```css
/* Consistent heading styles */
h1, h2, h3, h4, h5, h6 { 
    font-family: 'Georgia', serif; 
    font-weight: bold; 
    color: #333; 
}

/* Button variations */
.btn, .button, input[type="submit"], input[type="button"] {
    cursor: pointer;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    font-size: 14px;
}
```

## Combinators - Relationships Between Elements

### Descendant Selector (Space) - Targets nested elements at any level
```css
/* Any paragraph inside an article, no matter how deeply nested */
article p { 
    line-height: 1.6; 
    margin-bottom: 15px; 
}

/* All links inside navigation */
nav a { 
    color: #333; 
    text-decoration: none; 
    padding: 10px 15px; 
    display: block; 
}

/* Useful for component styling */
.sidebar .widget { margin: 10px 0; }
.card .title { font-size: 1.2em; font-weight: bold; }
.form .input-group { margin-bottom: 20px; }
```

### Child Selector (>) - Targets direct children only
```css
/* Only direct ul children of nav, not nested ones */
nav > ul { 
    display: flex; 
    list-style: none; 
    margin: 0; 
    padding: 0; 
}

/* Direct .header children of .card */
.card > .header { 
    padding: 15px; 
    border-bottom: 1px solid #eee; 
    font-weight: bold; 
}

/* Prevents styling deeply nested elements */
.menu > li { /* Only top-level menu items */ }
.tabs > .tab { /* Only direct tab children */ }
```

### Adjacent Sibling Selector (+) - Targets immediate next sibling
```css
/* First paragraph immediately after any h2 */
h2 + p { 
    margin-top: 0; 
    font-weight: 500; 
    color: #666; 
}

/* Input immediately after label */
label + input { 
    margin-left: 10px; 
}

/* Remove margin from first element after heading */
.section-title + * { margin-top: 0; }
```

### General Sibling Selector (~) - Targets all following siblings
```css
/* All paragraphs that come after h2 (not just the first) */
h2 ~ p { 
    color: #666; 
    font-size: 14px; 
}

/* All form groups after an alert */
.alert ~ .form-group { 
    margin-top: 20px; 
}

/* Style subsequent items differently */
.feature-item ~ .feature-item { border-top: 1px solid #eee; }
```

## Attribute Selectors - Target by HTML Attributes

### Exact Match - Target elements with specific attribute values
```css
/* Style different input types */
input[type="text"] { 
    border: 1px solid #ccc; 
    padding: 8px; 
    border-radius: 4px; 
}

input[type="email"] { 
    background: url(email-icon.png) no-repeat 10px center; 
    padding-left: 35px; 
}

input[type="password"] { 
    font-family: monospace; 
}

/* Target links that open in new window */
a[target="_blank"] { 
    text-decoration: underline; 
    color: #0066cc; 
}

a[target="_blank"]::after { 
    content: " ↗"; 
    font-size: 0.8em; 
}
```

### Prefix Match (^=) - Attribute starts with value
```css
/* Style external links */
a[href^="http"] { 
    color: #0066cc; 
}

a[href^="https"] { 
    color: green; 
    font-weight: 500; 
}

/* Target telephone links */
a[href^="tel:"] { 
    color: #007bff; 
    text-decoration: none; 
}

/* Image paths */
img[src^="/images/thumbnails/"] { 
    border-radius: 4px; 
    max-width: 150px; 
}
```

### Suffix Match ($=) - Attribute ends with value
```css
/* Style links to different file types */
a[href$=".pdf"] { 
    background: url(pdf-icon.png) no-repeat left center; 
    padding-left: 20px; 
    color: #d32f2f; 
}

a[href$=".doc"], a[href$=".docx"] { 
    background: url(word-icon.png) no-repeat left center; 
    padding-left: 20px; 
    color: #1976d2; 
}

/* Image formats */
img[src$=".jpg"], img[src$=".jpeg"] { 
    border: 2px solid #f0f0f0; 
}
```

### Contains Match (*=) - Attribute contains value anywhere
```css
/* Target grid system classes */
div[class*="col-"] { 
    float: left; 
    padding: 0 15px; 
}

/* Email-related inputs */
input[name*="email"] { 
    background: #f0f8ff; 
    border-color: #0066cc; 
}

/* Language-specific content */
[lang*="en"] { font-family: "Helvetica", sans-serif; }
[lang*="ar"] { direction: rtl; text-align: right; }
```

### Case-Insensitive Match (i) - Ignore case when matching
```css
/* Match regardless of case */
input[type="EMAIL" i] { 
    text-transform: lowercase; 
}

[data-state="ACTIVE" i] { 
    display: block; 
    opacity: 1; 
}
```

### Data Attributes - Custom HTML attributes
```css
/* JavaScript state management */
[data-state="loading"] { 
    opacity: 0.5; 
    pointer-events: none; 
}

[data-state="active"] { 
    display: block; 
    animation: fadeIn 0.3s ease; 
}

[data-theme="dark"] { 
    background: #222; 
    color: #fff; 
}

/* Component variations */
[data-size="large"] { 
    font-size: 1.2em; 
    padding: 15px 25px; 
}

[data-variant="outline"] { 
    background: transparent; 
    border: 2px solid currentColor; 
}
```

## Specificity and the Cascade

Understanding how CSS determines which styles to apply:

```css
/* Specificity values (from lowest to highest): */
/* Element selector: 1 point */
p { color: black; }

/* Class selector: 10 points */
.text { color: blue; }

/* ID selector: 100 points */
#content { color: red; }

/* Inline styles: 1000 points */
/* <p style="color: green;">Text</p> */

/* !important: Overrides everything (use sparingly) */
.urgent { color: orange !important; }
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use classes for reusable component styles | Use IDs for styling multiple elements |
| Keep specificity low and consistent | Over-qualify selectors (`div.content.main.wrapper`) |
| Use semantic combinators that match HTML structure | Chain too many descendant selectors (impacts performance) |
| Group related selectors to reduce repetition | Repeat identical declaration blocks |
| Use attribute selectors for form styling | Rely on universal selector in large stylesheets |
| Test selectors in browser dev tools | Assume selectors work without testing |

## Real-World Examples

### Navigation Menu
```css
/* Clean, semantic navigation styling */
.main-nav ul { 
    display: flex; 
    list-style: none; 
    margin: 0; 
    padding: 0; 
}

.main-nav li { 
    position: relative; 
}

.main-nav a { 
    display: block; 
    padding: 15px 20px; 
    text-decoration: none; 
    color: #333; 
    transition: background-color 0.3s; 
}

.main-nav a:hover { 
    background-color: #f5f5f5; 
}

/* Dropdown styling */
.main-nav li:hover > ul { 
    display: block; 
}
```

### Form Styling
```css
/* Comprehensive form styling using various selectors */
.form-group { 
    margin-bottom: 20px; 
}

.form-group label { 
    display: block; 
    margin-bottom: 5px; 
    font-weight: 500; 
}

input[type="text"], 
input[type="email"], 
input[type="password"], 
textarea { 
    width: 100%; 
    padding: 10px; 
    border: 1px solid #ddd; 
    border-radius: 4px; 
    font-size: 16px; 
}

input:focus { 
    outline: none; 
    border-color: #007bff; 
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25); 
}

input[required] + label::after { 
    content: " *"; 
    color: red; 
}
```

## Quick Fixes

- **Style not applying** → Check selector spelling, specificity, and cascade order
- **Too specific selector** → Remove unnecessary qualifiers, use single classes instead
- **Performance issues** → Avoid universal selector `*`, minimize descendant selectors
- **Conflicting styles** → Use more specific selectors or reorganize CSS order
- **Responsive issues** → Test selectors work with dynamic content and different screen sizes
- **Accessibility problems** → Ensure focus states are visible and interactive elements are targetable

## Gotchas

⚠️ **Specificity cascade**: `#nav .menu` (110 points) beats `.navigation .menu-item` (20 points)
⚠️ **Whitespace matters**: `div p` (descendant) ≠ `div>p` (child) ≠ `div+p` (adjacent sibling)
⚠️ **Attribute quotes**: Always quote attribute values: `[type="text"]` not `[type=text]`
⚠️ **Case sensitivity**: Class/ID names are case-sensitive, HTML attributes usually aren't
⚠️ **Performance impact**: `* html body` is slower than `.page-wrapper`
⚠️ **Browser support**: Some attribute selectors have limited IE support

## Advanced Patterns

### Component Architecture
```css
/* BEM-style component organization */
.card { /* Block */ }
.card__header { /* Element */ }
.card--featured { /* Modifier */ }

/* Target modifier combinations */
.card--featured .card__header { 
    background: linear-gradient(45deg, #ffd700, #ffed4e); 
}
```

### State Management
```css
/* Use data attributes for JavaScript-driven states */
[data-loading="true"] .content { opacity: 0.5; }
[data-expanded="true"] .collapsible { max-height: none; }
[data-theme="dark"] .component { /* dark theme styles */ }
```

## Checklist

- [ ] Choose the most appropriate selector type for each use case
- [ ] Keep specificity as low as possible while maintaining functionality
- [ ] Test selectors match intended elements using browser dev tools
- [ ] Verify selectors work with dynamic content and different HTML structures
- [ ] Group related selectors to reduce CSS redundancy and improve maintainability
- [ ] Consider performance impact of complex selectors in large stylesheets
- [ ] Ensure selectors remain functional when HTML structure changes
- [ ] Use semantic selectors that reflect content meaning, not just appearance
- [ ] Document complex selectors with comments explaining their purpose