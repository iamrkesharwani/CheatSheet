# CSS Properties `v 0.07.25.01`

## Essentials

```css
/* Text & Layout Fundamentals */
.element {
  color: #333;
  font-family: 'Arial', sans-serif;
  font-size: 16px;
  padding: 20px;
  margin: 10px auto;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

**Key Concepts**: Box Model | Flexbox | Grid | Typography | Positioning
**When to use**: Styling web elements, creating layouts, controlling visual presentation

## Syntax Reference

### Text and Typography

```css
/* Font Properties */
font-family: 'Helvetica', Arial, sans-serif;
font-size: 16px;           /* or 1rem, 1.2em */
font-weight: 400;          /* or normal, bold, 100-900 */
line-height: 1.5;          /* unitless preferred */
color: #333;               /* hex, rgb(), hsl() */

/* Text Formatting */
text-align: left;          /* center, right, justify */
text-transform: uppercase; /* lowercase, capitalize */
text-decoration: underline; /* none, line-through */
letter-spacing: 0.1em;
word-spacing: 0.2em;

/* Text Overflow */
white-space: nowrap;       /* normal, pre, pre-wrap */
text-overflow: ellipsis;   /* requires overflow: hidden */
overflow-wrap: break-word; /* for long words */
```

### Box Model

```css
/* Sizing */
width: 300px;              /* auto, 100%, min-content */
height: 200px;             /* auto, 100vh, fit-content */
max-width: 100%;           /* responsive constraint */
min-height: 400px;         /* minimum size */

/* Spacing */
padding: 20px;             /* all sides */
padding: 10px 20px;        /* vertical horizontal */
padding: 10px 20px 15px 5px; /* top right bottom left */
margin: 0 auto;            /* center horizontally */

/* Borders */
border: 2px solid #ccc;    /* width style color */
border-radius: 8px;        /* rounded corners */
border-top: 1px solid red; /* individual sides */

/* Box Sizing */
box-sizing: border-box;    /* includes padding/border in width */
box-sizing: content-box;   /* default, excludes padding/border */
```

### Backgrounds

```css
/* Basic Backgrounds */
background-color: #f5f5f5;
background-image: url('image.jpg');

/* Advanced Background Properties */
background-repeat: no-repeat;     /* repeat, repeat-x, repeat-y */
background-position: center top;  /* left/center/right top/center/bottom */
background-size: cover;           /* contain, 100px 200px, 50% */
background-attachment: fixed;     /* scroll, local */

/* Shorthand */
background: url('bg.jpg') no-repeat center/cover fixed;
```

### Display and Layout

```css
/* Display Types */
display: block;            /* takes full width */
display: inline;           /* flows with text */
display: inline-block;     /* inline but accepts width/height */
display: none;             /* removes from layout */
display: flex;             /* flexible box layout */
display: grid;             /* grid layout */

/* Visibility */
visibility: hidden;        /* hides but keeps space */
visibility: visible;       /* default */

/* Positioning */
position: static;          /* default, normal flow */
position: relative;        /* relative to normal position */
position: absolute;        /* relative to positioned parent */
position: fixed;           /* relative to viewport */
position: sticky;          /* sticky when scrolling */

/* Position Offsets */
top: 10px;
right: 20px;
bottom: 30px;
left: 40px;
z-index: 10;              /* stacking order */
```

### Flexbox

```css
/* Flex Container */
.container {
  display: flex;
  flex-direction: row;        /* column, row-reverse, column-reverse */
  justify-content: center;    /* flex-start, flex-end, space-between, space-around */
  align-items: center;        /* flex-start, flex-end, stretch, baseline */
  flex-wrap: wrap;            /* nowrap, wrap-reverse */
  gap: 20px;                  /* space between items */
}

/* Flex Items */
.item {
  flex-grow: 1;               /* how much to grow */
  flex-shrink: 0;             /* how much to shrink */
  flex-basis: 200px;          /* initial size */
  flex: 1 0 200px;            /* shorthand: grow shrink basis */
  align-self: flex-end;       /* override container's align-items */
}
```

### Grid

```css
/* Grid Container */
.grid {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;    /* flexible fractions */
  grid-template-columns: repeat(3, 1fr);  /* repeat pattern */
  grid-template-rows: 100px auto 200px;
  gap: 20px;                              /* row and column gap */
  justify-items: center;                  /* align items horizontally */
  align-items: start;                     /* align items vertically */
}

/* Grid Areas */
.grid {
  grid-template-areas: 
    "header header header"
    "sidebar main main"
    "footer footer footer";
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
```

### Overflow and Scrolling

```css
/* Overflow Control */
overflow: visible;         /* default, content overflows */
overflow: hidden;          /* clips overflowing content */
overflow: scroll;          /* always shows scrollbars */
overflow: auto;            /* scrollbars when needed */

/* Individual Axes */
overflow-x: hidden;
overflow-y: scroll;

/* Scrollbar Styling (Webkit) */
::-webkit-scrollbar {
  width: 12px;
}
::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 6px;
}
```

### Units and Sizing

```css
/* Absolute Units */
width: 300px;              /* pixels */
font-size: 14pt;           /* points */

/* Relative Units */
font-size: 1.2em;          /* relative to parent font-size */
font-size: 1.2rem;         /* relative to root font-size */
width: 50%;                /* percentage of parent */
height: 100vh;             /* viewport height */
width: 80vw;               /* viewport width */

/* Modern CSS Functions */
width: calc(100% - 40px);           /* calculations */
font-size: clamp(1rem, 2.5vw, 2rem); /* responsive sizing */
width: min(90%, 600px);             /* minimum of values */
height: max(200px, 50vh);           /* maximum of values */
```

### Interaction and Effects

```css
/* Cursor */
cursor: pointer;           /* hand pointer */
cursor: default;           /* arrow */
cursor: wait;              /* loading spinner */
cursor: not-allowed;       /* prohibited */

/* User Interaction */
user-select: none;         /* prevent text selection */
pointer-events: none;      /* disable all interactions */

/* Opacity and Visibility */
opacity: 0.7;              /* 0 (transparent) to 1 (opaque) */
visibility: hidden;        /* hides but keeps layout space */
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `box-sizing: border-box` globally | Forget to reset default margins/padding |
| Use relative units (rem, %) for scalability | Use only px for everything |
| Group related properties together | Mix layout and styling randomly |
| Use shorthand properties when appropriate | Write verbose individual properties |
| Test responsive behavior early | Assume fixed widths work everywhere |
| Use semantic class names | Use styling-based class names |

## Quick Fixes

- **Text not wrapping** → Add `overflow-wrap: break-word`
- **Flexbox items not centering** → Check both `justify-content` and `align-items`
- **Grid items overlapping** → Verify `grid-template-columns/rows` values
- **Background image not showing** → Check file path and add `background-size`
- **Element invisible but clickable** → Use `display: none` instead of `opacity: 0`
- **Scrollbar appearing unexpectedly** → Set `overflow: hidden` on parent

## Gotchas

⚠️ **Margin Collapse**: Vertical margins between adjacent elements collapse to the larger value
⚠️ **Flexbox Shrinking**: Items shrink by default (`flex-shrink: 1`) - use `flex-shrink: 0` to prevent
⚠️ **Z-index Context**: Only works on positioned elements (`position` other than `static`)
⚠️ **Percentage Heights**: Require parent element to have explicit height
⚠️ **Inline Elements**: Cannot have `width`, `height`, or vertical `margin/padding`
⚠️ **CSS Specificity**: More specific selectors override less specific ones regardless of order

## Checklist

- [ ] Set `box-sizing: border-box` on all elements
- [ ] Use CSS reset or normalize.css for consistent defaults
- [ ] Test layouts on different screen sizes
- [ ] Validate CSS syntax and check browser compatibility
- [ ] Use developer tools to debug layout issues
- [ ] Optimize for performance (avoid expensive properties like `filter`)
- [ ] Ensure sufficient color contrast for accessibility
- [ ] Test keyboard navigation and screen reader compatibility