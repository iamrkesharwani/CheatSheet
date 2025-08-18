# CSS Basics `v 3.0.25.01`

## Essentials
```css
/* Basic CSS rule structure */
selector {
    property: value;
    property: value;
}

/* Example: Style all paragraphs */
p {
    color: blue;
    font-size: 16px;
    margin: 10px 0;
}
```

**Key Concepts**: Selectors | Properties | Values | Cascade | Specificity
**When to use**: Style HTML elements, control layout, create responsive designs

## Syntax Reference

### Basic Structure
```css
/* Selector targets HTML elements */
h1 {
    color: #333;           /* Property: value pair */
    font-size: 2rem;       /* End with semicolon */
}

/* Multiple selectors */
h1, h2, h3 {
    font-family: Arial, sans-serif;
}

/* Class selector (reusable) */
.highlight {
    background-color: yellow;
}

/* ID selector (unique) */
#header {
    width: 100%;
}
```

### CSS Implementation Methods
```html
<!-- 1. Inline CSS (highest specificity) -->
<p style="color: red; font-weight: bold;">Inline styled text</p>

<!-- 2. Internal CSS (in <head> section) -->
<head>
    <style>
        .internal-style {
            color: green;
            padding: 20px;
        }
    </style>
</head>

<!-- 3. External CSS (separate .css file) -->
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

### Comments and Special Declarations
```css
/* This is a single-line comment */

/* 
   This is a 
   multi-line comment 
*/

.important-rule {
    color: red !important;    /* Overrides other declarations */
    font-size: 18px;
}

/* CSS is case-insensitive for properties but case-sensitive for values */
.example {
    COLOR: blue;              /* Valid - property names ignore case */
    font-family: Arial;       /* Case-sensitive value */
    FONT-FAMILY: arial;       /* Different from above */
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use external CSS for maintainability | Overuse `!important` - breaks cascade |
| Write meaningful class names (.nav-button) | Use inline styles for complex styling |
| Group related properties together | Forget semicolons after declarations |
| Use comments to explain complex rules | Mix different naming conventions |

## Quick Fixes
- **Styles not applying** → Check selector specificity and spelling
- **Colors not showing** → Verify hex codes (#fff not #white) or use color names
- **!important not working** → Another !important rule has higher specificity
- **External CSS not loading** → Check file path and <link> tag placement in <head>

## Gotchas
⚠️ **Case sensitivity**: Property names are case-insensitive, but values (like font names) are case-sensitive
⚠️ **Whitespace matters**: `font-family: Arial` ≠ `font-family:Arial` (spacing around colons is flexible)
⚠️ **!important cascade**: Multiple !important rules still follow specificity - ID beats class beats element
⚠️ **Inline style priority**: Inline styles override internal/external CSS unless !important is used

## Checklist
- [ ] External CSS linked in HTML <head> section
- [ ] All CSS rules end with semicolons
- [ ] Class names use consistent naming convention (kebab-case recommended)
- [ ] Comments explain complex or non-obvious styling decisions
- [ ] !important used sparingly and only when necessary