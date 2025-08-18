# CSS & JavaScript Integration `v 3.0.25`

## Essentials
```html
<!-- Optimal loading order -->
<head>
    <link rel="stylesheet" href="styles.css">
    <script defer src="app.js"></script>
</head>
<body>
    <div id="app">Content</div>
    <!-- Non-critical scripts at bottom -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // DOM ready code here
        });
    </script>
</body>
```

**Key Concepts**: External Linking | DOM Ready States | Progressive Enhancement | Graceful Degradation
**When to use**: Every web project needs proper CSS/JS integration for performance and functionality

## Syntax Reference

### CSS Linking Methods
```html
<!-- External (preferred) -->
<link rel="stylesheet" href="styles.css">
<link rel="stylesheet" href="mobile.css" media="(max-width: 768px)">

<!-- Internal -->
<style>
    .header { background: #333; }
    @media (max-width: 768px) { .header { padding: 10px; } }
</style>

<!-- Inline (use sparingly) -->
<div style="color: red; margin: 10px;">Content</div>
```

### JavaScript Loading Patterns
```html
<!-- Basic loading -->
<script src="app.js"></script>
<script defer src="app.js"></script>     <!-- Execute after HTML parsing -->
<script async src="analytics.js"></script> <!-- Execute when loaded -->

<!-- Module loading -->
<script type="module" src="main.js"></script>

<!-- DOM ready patterns -->
<script>
    document.addEventListener('DOMContentLoaded', () => {
        // DOM elements accessible
    });
    
    window.addEventListener('load', () => {
        // All resources loaded
    });
</script>
```

## Common Patterns

### Pattern 1: Performance Optimization
```html
<!-- Before: Render blocking -->
<head>
    <script src="large-library.js"></script>
    <link rel="stylesheet" href="styles.css">
</head>

<!-- After: Optimized loading -->
<head>
    <link rel="preload" href="critical.css" as="style" onload="this.rel='stylesheet'">
    <script defer src="large-library.js"></script>
</head>
```

### Pattern 2: Progressive Enhancement
```html
<!-- Base functionality without JS -->
<form action="/submit" method="post">
    <input type="email" name="email" required>
    <button type="submit">Subscribe</button>
</form>

<script>
    // Enhance with AJAX
    if ('fetch' in window) {
        document.querySelector('form').addEventListener('submit', handleAjax);
    }
</script>
```

### Pattern 3: Feature Detection
```html
<script>
    // Test for features before using
    if ('localStorage' in window) {
        // Use localStorage
    } else {
        // Fallback to cookies
    }
    
    if (CSS.supports('display', 'grid')) {
        document.body.classList.add('has-grid');
    }
</script>
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `defer` for non-critical scripts | Put large scripts in `<head>` without defer |
| Load CSS before JavaScript | Use `@import` in CSS (performance killer) |
| Wait for DOM ready before manipulation | Access DOM elements immediately in `<head>` |
| Provide fallbacks for modern features | Assume all browsers support latest features |
| Use external files for reusable code | Inline large amounts of CSS/JS |

## Quick Fixes
- **Script not executing** → Check if DOM is ready: `document.readyState`
- **CSS not loading** → Verify path and check for CORS issues
- **Elements not found** → Wrap code in `DOMContentLoaded` event listener
- **Performance issues** → Move non-critical scripts to bottom, use `defer`/`async`
- **Mobile issues** → Add viewport meta tag and responsive CSS

## Gotchas
⚠️ **Script execution order** - `async` scripts execute when loaded, not in order
⚠️ **CSS blocking** - CSS in `<head>` blocks rendering until loaded
⚠️ **DOM timing** - Scripts in `<head>` run before DOM elements exist
⚠️ **Feature detection** - Always test before using modern APIs
⚠️ **CDN fallbacks** - External resources can fail, always have backups

## Checklist
- [ ] CSS loaded before JavaScript for styling priority
- [ ] Scripts use `defer` or placed at bottom for performance
- [ ] DOM ready event listeners for safe element access
- [ ] Feature detection implemented for modern APIs
- [ ] Fallback strategies in place for failed resources
- [ ] Media queries used for responsive CSS loading
- [ ] Error handling for script failures
- [ ] Progressive enhancement pattern followed