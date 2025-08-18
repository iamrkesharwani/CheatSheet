# Tailwind CSS Sizing & Spacing `v3.4.x`

## Essentials

```css
/* Width & Height */
w-full h-screen        /* Full width, viewport height */
w-64 h-32             /* Fixed sizes (16rem × 8rem) */
max-w-md min-h-0      /* Constraints */

/* Padding & Margin */
p-4 m-2               /* Padding 1rem, margin 0.5rem */
px-6 py-3             /* Horizontal/vertical shortcuts */
mx-auto               /* Center horizontally */
-mt-2                 /* Negative top margin */

/* Gap & Spacing */
gap-4                 /* Grid/flex gap 1rem */
space-y-2             /* Vertical spacing between children */
```

**Key Concepts**: Scale System | Direction Shortcuts | Responsive Prefixes | Arbitrary Values
**When to use**: Layout structure, component spacing, responsive design, and visual hierarchy

## Syntax Reference

### Width & Height Scale

```css
/* Tailwind uses a consistent scale: 0, 0.5, 1, 1.5, 2, 2.5, 3, 3.5, 4, 5, 6, 8, 10, 12, 14, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64, 72, 80, 96 */

/* Width */
w-0          /* width: 0px */
w-px         /* width: 1px */
w-0.5        /* width: 0.125rem (2px) */
w-1          /* width: 0.25rem (4px) */
w-4          /* width: 1rem (16px) */
w-64         /* width: 16rem (256px) */

/* Special width values */
w-auto       /* width: auto */
w-full       /* width: 100% */
w-screen     /* width: 100vw */
w-min        /* width: min-content */
w-max        /* width: max-content */
w-fit        /* width: fit-content */

/* Arbitrary values */
w-[200px]    /* width: 200px */
w-[50vw]     /* width: 50vw */
```

### Min/Max Constraints

```css
/* Min Width */
min-w-0      /* min-width: 0px */
min-w-full   /* min-width: 100% */
min-w-min    /* min-width: min-content */
min-w-max    /* min-width: max-content */
min-w-fit    /* min-width: fit-content */

/* Max Width */
max-w-none   /* max-width: none */
max-w-xs     /* max-width: 20rem */
max-w-sm     /* max-width: 24rem */
max-w-md     /* max-width: 28rem */
max-w-lg     /* max-width: 32rem */
max-w-xl     /* max-width: 36rem */
max-w-2xl    /* max-width: 42rem */
max-w-4xl    /* max-width: 56rem */
max-w-full   /* max-width: 100% */
max-w-screen-sm /* max-width: 640px */
```

### Padding & Margin Directions

```css
/* All sides */
p-4          /* padding: 1rem */
m-4          /* margin: 1rem */

/* Horizontal & Vertical */
px-4         /* padding-left: 1rem; padding-right: 1rem */
py-4         /* padding-top: 1rem; padding-bottom: 1rem */
mx-4         /* margin-left: 1rem; margin-right: 1rem */
my-4         /* margin-top: 1rem; margin-bottom: 1rem */

/* Individual sides */
pt-4         /* padding-top: 1rem */
pr-4         /* padding-right: 1rem */
pb-4         /* padding-bottom: 1rem */
pl-4         /* padding-left: 1rem */

/* Negative margins */
-m-4         /* margin: -1rem */
-mt-2        /* margin-top: -0.5rem */
-mx-4        /* margin-left: -1rem; margin-right: -1rem */
```

### Gap & Space Between

```css
/* Gap (for Grid & Flexbox) */
gap-0        /* gap: 0px */
gap-4        /* gap: 1rem */
gap-x-4      /* column-gap: 1rem */
gap-y-2      /* row-gap: 0.5rem */

/* Space Between (adds margin to children) */
space-x-4    /* > * + * { margin-left: 1rem } */
space-y-2    /* > * + * { margin-top: 0.5rem } */
space-x-reverse /* reverses direction for RTL */
-space-y-2   /* > * + * { margin-top: -0.5rem } */
```

## Common Patterns

```css
/* Centered container with max width */
mx-auto max-w-4xl px-4

/* Card with consistent spacing */
p-6 space-y-4

/* Responsive grid with gaps */
grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6

/* Flexbox with item spacing */
flex space-x-4

/* Full-height sidebar */
min-h-screen w-64

/* Responsive padding */
p-4 md:p-6 lg:p-8

/* Negative margin overlap */
-mt-12 relative z-10
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `mx-auto` to center block elements | Use `text-center` for block centering |
| Use `space-y-*` for consistent vertical rhythm | Apply individual margins to each child |
| Use responsive prefixes (`md:p-8`) for different screen sizes | Use only one size for all breakpoints |
| Use `max-w-*` to prevent content from becoming too wide | Let text lines become too long on large screens |
| Use `gap-*` for grid and flexbox layouts | Use margins for grid/flex spacing |
| Use arbitrary values `w-[200px]` when needed | Create custom CSS for one-off sizes |

## Quick Fixes

- **Element not centering** → Use `mx-auto` (needs defined width) or `flex justify-center` on parent
- **Spacing inconsistent** → Use `space-y-*` instead of individual margins
- **Content too wide on large screens** → Add `max-w-*` constraint
- **Gap not working** → Ensure parent has `display: flex` or `display: grid`
- **Negative margin not working** → Check stacking context and z-index
- **Responsive spacing not applying** → Verify breakpoint prefix syntax (`md:p-4` not `md-p-4`)

## Gotchas

⚠️ **Space utilities affect direct children only** - `space-y-4` only adds margin between immediate child elements, not nested ones

⚠️ **Gap vs Space-between** - Use `gap-*` for grid/flex containers, `space-*` for adding margins between children

⚠️ **Auto margins need defined width** - `mx-auto` only centers elements with a specified width (not `w-auto`)

⚠️ **Negative margins can cause overflow** - Always test negative margins, especially on mobile devices

⚠️ **Min-height doesn't affect flex children** - Use `h-full` on flex children when parent has `min-h-screen`

## Checklist

- [ ] **Width constraints**: Applied `max-w-*` to prevent content from becoming too wide
- [ ] **Responsive spacing**: Used breakpoint prefixes for different screen sizes
- [ ] **Consistent rhythm**: Used `space-*` utilities for vertical/horizontal spacing between elements
- [ ] **Centering strategy**: Used appropriate centering method (`mx-auto`, `flex justify-center`, etc.)
- [ ] **Gap vs margin**: Used `gap-*` for grid/flex layouts, individual margins for other cases
- [ ] **Mobile-first approach**: Started with mobile spacing and enhanced for larger screens
- [ ] **Semantic spacing**: Used logical spacing that reflects content hierarchy
- [ ] **Overflow prevention**: Checked that negative margins don't cause horizontal scroll