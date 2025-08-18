# HTML Performance Optimization `v 0.07.25.01`

## Essentials
```html
<!-- Critical CSS inline -->
<style>
  .header { display: flex; background: #fff; }
  .hero { min-height: 100vh; }
</style>

<!-- Lazy load images -->
<img src="image.jpg" loading="lazy" alt="Description">

<!-- Async non-critical CSS -->
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">

<!-- Defer JavaScript -->
<script src="app.js" defer></script>
```

**Key Concepts**: Critical Rendering Path | Resource Optimization | Lazy Loading | CDN Usage
**When to use**: Every production website to improve load times and user experience

## Syntax Reference

### Critical CSS Inlining
```html
<head>
  <!-- Inline critical styles for above-the-fold content -->
  <style>
    body { margin: 0; font-family: system-ui; }
    .header { background: #fff; padding: 1rem; }
    .hero { min-height: 100vh; display: flex; }
  </style>
  
  <!-- Load non-critical CSS asynchronously -->
  <link rel="preload" href="non-critical.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="non-critical.css"></noscript>
</head>
```

### Image Optimization
```html
<!-- Responsive images with modern formats -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Optimized image" loading="lazy" width="800" height="600">
</picture>

<!-- Multi-resolution support -->
<img src="image-400.jpg" 
     srcset="image-400.jpg 400w, image-800.jpg 800w, image-1200.jpg 1200w"
     sizes="(max-width: 400px) 400px, (max-width: 800px) 800px, 1200px"
     alt="Responsive image" loading="lazy">
```

### Resource Loading Optimization
```html
<head>
  <!-- DNS prefetch for external domains -->
  <link rel="dns-prefetch" href="//cdnjs.cloudflare.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  
  <!-- Preload critical resources -->
  <link rel="preload" href="critical-font.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="hero-image.jpg" as="image">
  
  <!-- Async/defer JavaScript -->
  <script src="critical.js"></script>
  <script src="analytics.js" defer></script>
  <script src="tracking.js" async></script>
</head>
```

### CDN Implementation
```html
<!-- CSS from CDN with fallback -->
<link rel="stylesheet" 
      href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.0/css/bootstrap.min.css"
      integrity="sha512-..."
      crossorigin="anonymous"
      onerror="this.onerror=null;this.href='css/bootstrap.min.css';">

<!-- JavaScript from CDN with fallback -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.0/jquery.min.js"></script>
<script>
  window.jQuery || document.write('<script src="js/jquery-3.7.0.min.js"><\/script>')
</script>
```

## Common Patterns

### Font Loading Optimization
```html
<!-- Optimal font loading strategy -->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>

<style>
  @font-face {
    font-family: 'CustomFont';
    font-display: swap; /* Show fallback immediately */
    src: url('font.woff2') format('woff2');
  }
</style>
```

### Lazy Loading with Fallback
```html
<!-- Native lazy loading with Intersection Observer fallback -->
<img src="placeholder.jpg"
     data-src="actual-image.jpg"
     loading="lazy"
     class="lazy-image"
     alt="Lazy loaded image">

<script>
// Fallback for browsers without native lazy loading
if ('IntersectionObserver' in window) {
  const imageObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const img = entry.target;
        img.src = img.dataset.src;
        imageObserver.unobserve(img);
      }
    });
  });
  
  document.querySelectorAll('.lazy-image').forEach(img => {
    imageObserver.observe(img);
  });
}
</script>
```

### Resource Bundling
```html
<!-- Combine multiple CSS files -->
<!-- ❌ Multiple requests -->
<link rel="stylesheet" href="reset.css">
<link rel="stylesheet" href="layout.css">
<link rel="stylesheet" href="theme.css">

<!-- ✅ Single bundled file -->
<link rel="stylesheet" href="styles.min.css">

<!-- Bundle JavaScript modules -->
<script type="module" src="modern-bundle.js"></script>
<script nomodule src="legacy-bundle.js"></script>
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Inline critical CSS for above-the-fold content | Load all CSS synchronously |
| Use `loading="lazy"` for below-fold images | Load all images eagerly |
| Specify image dimensions to prevent layout shift | Leave images without width/height |
| Use modern image formats (WebP, AVIF) with fallbacks | Serve only JPEG/PNG images |
| Defer non-critical JavaScript | Block rendering with synchronous scripts |
| Use CDNs for popular libraries | Host all resources locally |
| Minify HTML, CSS, and JavaScript | Serve uncompressed files |
| Implement resource hints (preload, prefetch) | Let browser discover resources naturally |

## Quick Fixes
- **Slow initial render** → Inline critical CSS, defer non-critical resources
- **Layout shift on image load** → Add width/height attributes to images
- **Large bundle sizes** → Split into critical and non-critical chunks
- **Slow font rendering** → Use `font-display: swap` and preload fonts
- **Too many HTTP requests** → Bundle resources, use image sprites
- **Slow CDN resources** → Add fallbacks to local resources
- **Poor mobile performance** → Implement responsive images with `srcset`

## Gotchas
⚠️ **Critical CSS**: Only inline styles needed for above-the-fold content (typically <14KB)

⚠️ **Lazy Loading**: Don't lazy load above-the-fold images - use `loading="eager"` instead

⚠️ **Resource Hints**: `preload` for critical resources, `prefetch` for likely future resources

⚠️ **Image Formats**: Always provide fallbacks when using modern formats (WebP, AVIF)

⚠️ **JavaScript Loading**: `defer` maintains execution order, `async` doesn't

⚠️ **Font Loading**: Missing `crossorigin` attribute can block font preloading

## Performance Budget Targets
- **First Contentful Paint**: < 1.5s
- **Largest Contentful Paint**: < 2.5s  
- **First Input Delay**: < 100ms
- **Cumulative Layout Shift**: < 0.1
- **Total Page Size**: < 500KB
- **JavaScript Bundle**: < 150KB
- **CSS Bundle**: < 50KB

## Checklist
- [ ] Critical CSS inlined (< 14KB)
- [ ] Non-critical CSS loaded asynchronously
- [ ] Images optimized with modern formats and lazy loading
- [ ] JavaScript properly deferred or made async
- [ ] Resource hints implemented (dns-prefetch, preconnect, preload)
- [ ] CDN configured for static resources
- [ ] HTML/CSS/JS minified and compressed
- [ ] Font loading optimized with font-display: swap
- [ ] Image dimensions specified to prevent layout shift
- [ ] Performance monitoring implemented
- [ ] Fallbacks provided for CDN resources
- [ ] Browser caching headers configured