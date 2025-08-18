# CSS Media Queries & Responsive Design `v CSS3+`

## Essentials

```css
/* Mobile-first approach */
.container {
  width: 100%;
  padding: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    max-width: 750px;
    margin: 0 auto;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    padding: 2rem;
  }
}
```

**Key Concepts**: Mobile-First Design | Breakpoint Strategy | Flexible Units
**When to use**: Create layouts that adapt seamlessly across all device sizes and orientations

## Syntax Reference

### Basic Media Query Structure

```css
/* Basic syntax */
@media media-type and (condition) {
  /* CSS rules */
}

/* Screen-only queries (most common) */
@media screen and (min-width: 768px) {
  .element { /* styles */ }
}

/* Multiple conditions */
@media screen and (min-width: 768px) and (max-width: 1023px) {
  .element { /* tablet-only styles */ }
}
```

### Standard Breakpoints

```css
/* Mobile-first breakpoints */
/* Base styles: 0px - 767px (mobile) */
.grid {
  display: block;
}

/* Small tablets: 768px+ */
@media (min-width: 768px) {
  .grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Large tablets/small desktop: 1024px+ */
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Desktop: 1200px+ */
@media (min-width: 1200px) {
  .grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
  }
}

/* Large desktop: 1440px+ */
@media (min-width: 1440px) {
  .grid {
    max-width: 1400px;
    margin: 0 auto;
  }
}
```

### Orientation Queries

```css
/* Portrait orientation (height > width) */
@media (orientation: portrait) {
  .hero {
    height: 50vh;
    flex-direction: column;
  }
  
  .sidebar {
    position: static;
    width: 100%;
  }
}

/* Landscape orientation (width > height) */
@media (orientation: landscape) {
  .hero {
    height: 80vh;
    flex-direction: row;
  }
  
  .mobile-nav {
    flex-direction: row;
    justify-content: space-around;
  }
}

/* Specific device orientations */
@media screen and (max-width: 768px) and (orientation: landscape) {
  .header {
    height: 60px; /* Shorter header on mobile landscape */
  }
}
```

### Responsive Units

```css
/* Percentage units - relative to parent */
.container {
  width: 90%; /* 90% of parent width */
  max-width: 1200px;
}

/* Viewport units - relative to viewport */
.hero {
  height: 100vh; /* Full viewport height */
  width: 100vw;   /* Full viewport width */
}

.section {
  padding: 5vh 5vw; /* 5% of viewport height/width */
}

/* EM units - relative to parent font-size */
.card {
  padding: 1em;     /* 16px if parent is 16px */
  margin: 0.5em 0;  /* 8px if parent is 16px */
}

/* REM units - relative to root font-size */
.title {
  font-size: 2rem;    /* 32px if root is 16px */
  margin-bottom: 1rem; /* 16px if root is 16px */
}

/* vmin/vmax - responsive to smallest/largest viewport dimension */
.square {
  width: 50vmin;  /* 50% of smaller viewport dimension */
  height: 50vmin; /* Always creates a square */
}
```

## Mobile-First Design Pattern

```css
/* 1. Start with mobile styles (no media query needed) */
.navigation {
  position: fixed;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100vh;
  background: #fff;
  transition: left 0.3s ease;
}

.navigation.open {
  left: 0;
}

.nav-toggle {
  display: block;
  position: fixed;
  top: 1rem;
  right: 1rem;
}

/* 2. Enhance for larger screens */
@media (min-width: 768px) {
  .navigation {
    position: static;
    left: auto;
    width: auto;
    height: auto;
    background: transparent;
    display: flex;
  }
  
  .nav-toggle {
    display: none;
  }
}
```

## Common Responsive Patterns

```css
/* Responsive grid */
.grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr; /* Mobile: single column */
}

@media (min-width: 600px) {
  .grid {
    grid-template-columns: repeat(2, 1fr); /* Tablet: 2 columns */
  }
}

@media (min-width: 900px) {
  .grid {
    grid-template-columns: repeat(3, 1fr); /* Desktop: 3 columns */
  }
}

/* Responsive typography */
.heading {
  font-size: clamp(1.5rem, 4vw, 3rem); /* Fluid typography */
}

/* Hide/show elements by screen size */
.mobile-only {
  display: block;
}

.desktop-only {
  display: none;
}

@media (min-width: 768px) {
  .mobile-only {
    display: none;
  }
  
  .desktop-only {
    display: block;
  }
}

/* Responsive images */
.responsive-img {
  width: 100%;
  height: auto;
  max-width: 100%;
}

/* Container queries (modern browsers) */
@container (min-width: 400px) {
  .card {
    display: flex;
    flex-direction: row;
  }
}
```

## Advanced Media Queries

```css
/* High-resolution displays */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .logo {
    background-image: url('logo@2x.png');
    background-size: 100px 50px;
  }
}

/* Dark mode preference */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-color: #1a1a1a;
    --text-color: #ffffff;
  }
}

/* Reduced motion preference */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

/* Hover capability */
@media (hover: hover) {
  .button:hover {
    background-color: #0056b3;
  }
}

/* Print styles */
@media print {
  .no-print {
    display: none;
  }
  
  body {
    font-size: 12pt;
    color: black;
    background: white;
  }
}
```

## Do's & Don'ts

| ✅ Do                                    | ❌ Don't                               |
| ---------------------------------------- | -------------------------------------- |
| Use mobile-first approach               | Design desktop-first then scale down  |
| Test on real devices                    | Rely only on browser dev tools        |
| Use relative units (rem, em, %)        | Use fixed pixel values everywhere     |
| Set max-width on containers             | Let content stretch infinitely wide    |
| Use meaningful breakpoints              | Add breakpoints for every device size |

## Quick Fixes

- **Layout breaks at certain widths** → Add intermediate breakpoints or use more flexible units
- **Text too small on mobile** → Use `rem` units and set appropriate base font-size
- **Images overflow containers** → Add `max-width: 100%; height: auto;`
- **Horizontal scrolling on mobile** → Check for fixed widths, use `box-sizing: border-box`
- **Media query not working** → Check syntax and ensure screen width matches breakpoint

## Gotchas

⚠️ **Warning**: `em` units in media queries are based on browser default (16px), not parent element
⚠️ **Warning**: Viewport units (vh/vw) can cause issues with mobile browser UI (use `dvh`/`svh` when supported)
⚠️ **Warning**: `orientation` changes don't always trigger on some mobile devices
⚠️ **Warning**: Nested media queries are not supported - combine conditions instead

## Breakpoint Strategy

```css
/* Standard breakpoint system */
:root {
  --bp-mobile: 480px;
  --bp-tablet: 768px;
  --bp-desktop: 1024px;
  --bp-wide: 1200px;
}

/* Usage with custom properties */
@media (min-width: 768px) { /* Use actual values in media queries */
  .container {
    max-width: var(--bp-tablet);
  }
}

/* Common device targets */
/* iPhone SE: 375px */
/* iPhone 12/13: 390px */
/* iPad: 768px */
/* iPad Pro: 1024px */
/* Desktop: 1200px+ */
```

## Modern Responsive Techniques

```css
/* Container queries (cutting-edge) */
.card-container {
  container-type: inline-size;
}

@container (min-width: 300px) {
  .card {
    display: flex;
  }
}

/* Intrinsic web design */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* Fluid typography */
h1 {
  font-size: clamp(1.5rem, 2.5vw + 1rem, 4rem);
}

/* Aspect ratio (modern) */
.video-container {
  aspect-ratio: 16 / 9;
  width: 100%;
}
```

## Checklist

- [ ] Implemented mobile-first design approach
- [ ] Used relative units (rem, em, %) instead of fixed pixels
- [ ] Tested breakpoints on actual devices, not just browser resize
- [ ] Added appropriate meta viewport tag: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- [ ] Ensured images are responsive with max-width: 100%
- [ ] Considered orientation changes for mobile users
- [ ] Used meaningful breakpoints based on content, not specific devices