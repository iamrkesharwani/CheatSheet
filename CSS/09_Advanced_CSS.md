# Advanced CSS `v 0.07.25.01`

## Essentials

```css
/* Responsive typography with clamp() */
font-size: clamp(1rem, 2.5vw, 2rem);

/* Scroll snapping for carousels */
scroll-snap-type: x mandatory;
scroll-snap-align: start;

/* Aspect ratio for responsive containers */
aspect-ratio: 16 / 9;

/* Parent-aware selectors */
.card:has(.featured) { border: 2px solid gold; }

/* Container queries for component-based responsive design */
@container (min-width: 400px) {
  .card { flex-direction: row; }
}
```

**Key Concepts**: Fluid Design | Component Queries | Parent Awareness | Scroll Control | Intrinsic Sizing
**When to use**: Modern responsive layouts, component libraries, and enhanced user experiences

## Syntax Reference

### Clamp() Function

```css
/* Basic structure: clamp(min, preferred, max) */
font-size: clamp(1rem, 4vw, 3rem);
padding: clamp(1rem, 5%, 2rem);
width: clamp(200px, 50%, 800px);

/* Multiple properties */
.responsive-card {
  padding: clamp(1rem, 3vw, 2rem);
  font-size: clamp(0.875rem, 2.5vw, 1.25rem);
  gap: clamp(0.5rem, 2vw, 1.5rem);
}
```

### Scroll Snapping

```css
/* Container setup */
.scroll-container {
  scroll-snap-type: x mandatory; /* x, y, or both */
  overflow-x: scroll;
  display: flex;
}

/* Item alignment */
.scroll-item {
  scroll-snap-align: start; /* start, center, end */
  flex: none;
  width: 300px;
}

/* Stop scrolling at specific points */
.gallery {
  scroll-snap-type: both mandatory;
  scroll-snap-stop: always; /* Force stop at each item */
}
```

### Aspect Ratio

```css
/* Basic ratios */
.video-container { aspect-ratio: 16 / 9; }
.square-image { aspect-ratio: 1; }
.portrait { aspect-ratio: 3 / 4; }

/* With object-fit for images */
.responsive-image {
  aspect-ratio: 16 / 9;
  width: 100%;
  object-fit: cover;
}

/* Fallback for older browsers */
.aspect-box {
  aspect-ratio: 16 / 9;
}
@supports not (aspect-ratio: 1) {
  .aspect-box::before {
    content: '';
    display: block;
    padding-top: 56.25%; /* 9/16 * 100% */
  }
}
```

### :has() Selector

```css
/* Style parent based on child content */
.card:has(.sale-badge) {
  border: 2px solid red;
  box-shadow: 0 4px 8px rgba(255, 0, 0, 0.2);
}

/* Multiple conditions */
.form:has(input:invalid) {
  border-left: 4px solid red;
}

.article:has(h2):has(img) {
  display: grid;
  grid-template-columns: 1fr 200px;
}

/* Sibling awareness */
.nav-item:has(+ .nav-item.active) {
  margin-right: 0.5rem;
}
```

### Container Queries

```css
/* Define container */
.card-container {
  container-type: inline-size; /* size, inline-size, or normal */
  container-name: card; /* Optional naming */
}

/* Query the container */
@container card (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 150px 1fr;
    gap: 1rem;
  }
}

/* Multiple breakpoints */
@container (min-width: 300px) {
  .card-title { font-size: 1.25rem; }
}

@container (min-width: 500px) {
  .card-title { font-size: 1.5rem; }
  .card { padding: 2rem; }
}
```

## Common Patterns

```css
/* Pattern 1: Fluid typography system */
.fluid-text {
  --min-size: 1rem;
  --max-size: 2rem;
  --preferred: 4vw;
  font-size: clamp(var(--min-size), var(--preferred), var(--max-size));
}

/* Pattern 2: Smooth scrolling carousel */
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scroll-behavior: smooth;
  gap: 1rem;
}
.carousel-item {
  flex: none;
  scroll-snap-align: start;
  width: clamp(250px, 30vw, 350px);
}

/* Pattern 3: Responsive media with consistent ratios */
.media-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}
.media-item {
  aspect-ratio: 4 / 3;
  background: #f0f0f0;
  border-radius: 8px;
  overflow: hidden;
}

/* Pattern 4: Smart form validation styling */
.form-group:has(input:invalid:not(:placeholder-shown)) {
  --error-color: #e53e3e;
  border-left: 3px solid var(--error-color);
  padding-left: 1rem;
}

/* Pattern 5: Component-aware layouts */
.sidebar {
  container-type: inline-size;
}
@container (min-width: 200px) {
  .widget { padding: 1rem; }
}
@container (min-width: 300px) {
  .widget {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 0.5rem;
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use clamp() for fluid typography and spacing | Hardcode font sizes with media queries |
| Test scroll-snap on mobile devices thoroughly | Use scroll-snap without overflow handling |
| Combine aspect-ratio with object-fit for images | Assume aspect-ratio works in all browsers |
| Use :has() progressively with feature detection | Rely on :has() as the only styling method |
| Name containers for complex query scenarios | Create unnecessary container contexts |

## Quick Fixes

- **Clamp() not working** → Check unit consistency (rem/em vs px vs %)
- **Scroll snap too aggressive** → Add `scroll-snap-stop: normal` or adjust alignment
- **Aspect ratio distorted** → Use `object-fit: cover` or `contain` on images
- **:has() not supported** → Provide fallback styles or use @supports
- **Container queries ignored** → Ensure `container-type` is set on parent

## Gotchas

⚠️ **Browser Support**: :has() has limited support - always provide fallbacks
⚠️ **Performance**: Container queries can be expensive - avoid deeply nested containers
⚠️ **Scroll Snap**: Can interfere with accessibility tools - test with screen readers
⚠️ **Clamp() Accessibility**: Very small minimum sizes can hurt readability
⚠️ **Aspect Ratio**: Doesn't work with `display: inline` - use block or inline-block

## Checklist

- [ ] Test clamp() values across different screen sizes
- [ ] Verify scroll-snap behavior on touch devices
- [ ] Add aspect-ratio fallbacks for older browsers
- [ ] Provide alternative styling when :has() isn't supported
- [ ] Test container queries with actual content, not placeholders
- [ ] Check accessibility with keyboard navigation and screen readers
- [ ] Validate performance impact of advanced selectors in production