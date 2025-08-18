# Mobile-First Development `v 0.07.25.01`

## Essentials
```html
<!-- Essential viewport configuration -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- Touch-friendly button sizing -->
<button style="min-height: 48px; min-width: 48px; padding: 12px;">Click me</button>

<!-- Mobile-optimized form input -->
<input type="email" inputmode="email" autocomplete="email" style="font-size: 16px;">

<!-- Responsive image -->
<img src="image.jpg" srcset="image-400.jpg 400w, image-800.jpg 800w" 
     sizes="(max-width: 600px) 100vw, 50vw" alt="Description">
```

**Key Concepts**: Viewport Meta Tag | Touch Targets | Mobile Forms | Responsive Images | Mobile Navigation
**When to use**: Building any web application that needs to work seamlessly on mobile devices

## Viewport Configuration

### Essential Setup
```html
<!-- Standard responsive configuration -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- App-like experience (no zoom) -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">

<!-- Accessibility-friendly (allows zoom) -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
```

### Advanced Options
```html
<!-- PWA with notch support -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">

<!-- Prevent iOS shrinking -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">

<!-- Limited zoom range -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=0.5, maximum-scale=3.0">
```

## Touch-Friendly Design

### Touch Target Sizing
```css
/* Minimum touch targets */
.touch-target {
  min-width: 44px;   /* Apple HIG minimum */
  min-height: 44px;
  padding: 12px;
}

.material-touch {
  min-width: 48px;   /* Material Design minimum */
  min-height: 48px;
}

/* Button hierarchy */
.btn-primary { min-height: 48px; padding: 12px 24px; font-size: 16px; }
.btn-secondary { min-height: 44px; padding: 10px 20px; font-size: 14px; }
.btn-icon { width: 48px; height: 48px; padding: 12px; }
```

### Touch States & Feedback
```css
/* Touch feedback */
.touchable {
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0.1);
  transition: all 0.15s ease;
}

.touchable:active {
  transform: scale(0.98);
  opacity: 0.8;
}

/* Remove iOS tap highlight */
.no-highlight {
  -webkit-tap-highlight-color: transparent;
}

/* Smooth scrolling on iOS */
.scroll-area {
  -webkit-overflow-scrolling: touch;
  overflow-y: auto;
}
```

## Mobile Forms

### Input Types & Attributes
```html
<!-- Email with proper keyboard -->
<input type="email" inputmode="email" autocomplete="email" 
       autocapitalize="none" autocorrect="off" placeholder="Enter email">

<!-- Phone with numeric keypad -->
<input type="tel" inputmode="numeric" autocomplete="tel" placeholder="Phone">

<!-- Numbers with numeric keypad -->
<input type="number" inputmode="numeric" pattern="[0-9]*" placeholder="Amount">

<!-- Password with autocomplete -->
<input type="password" autocomplete="current-password" minlength="8">

<!-- Search with proper keyboard -->
<input type="search" inputmode="search" autocomplete="off" 
       autocorrect="off" autocapitalize="none" placeholder="Search...">
```

### Form Styling Best Practices
```css
/* Mobile-optimized inputs */
.form-input {
  width: 100%;
  min-height: 48px;
  padding: 12px 16px;
  font-size: 16px;        /* Prevents zoom on iOS */
  border: 2px solid #e1e1e1;
  border-radius: 8px;
}

.form-input:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
}

/* Submit button */
.form-submit {
  width: 100%;
  min-height: 48px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
}
```

## Responsive Images

### Srcset for Different Densities
```html
<!-- High DPI support -->
<img src="image-1x.jpg"
     srcset="image-1x.jpg 1x, image-2x.jpg 2x, image-3x.jpg 3x"
     alt="Description">

<!-- Width-based responsive -->
<img src="image-400.jpg"
     srcset="image-400.jpg 400w, image-800.jpg 800w, image-1200.jpg 1200w"
     sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 800px"
     alt="Description">
```

### Picture Element with Modern Formats
```html
<picture>
  <!-- Modern formats first -->
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  
  <!-- Fallback -->
  <img src="image.jpg" alt="Description">
</picture>
```

### CSS Image Optimization
```css
/* Basic responsive */
.responsive-img {
  max-width: 100%;
  height: auto;
  display: block;
}

/* Container with object-fit */
.image-container {
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.image-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}
```

## Mobile Navigation Patterns

### Hamburger Menu
```html
<nav class="navbar">
  <div class="nav-brand"><a href="/">Logo</a></div>
  
  <button class="nav-toggle" onclick="toggleMenu()">
    <span class="hamburger-line"></span>
    <span class="hamburger-line"></span>
    <span class="hamburger-line"></span>
  </button>
  
  <div class="nav-menu" id="nav-menu">
    <a href="/" class="nav-link">Home</a>
    <a href="/about" class="nav-link">About</a>
    <a href="/contact" class="nav-link">Contact</a>
  </div>
</nav>
```

```css
.nav-menu {
  position: fixed;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100vh;
  background: #fff;
  transition: 0.3s;
  z-index: 1000;
}

.nav-menu.active { left: 0; }
```

### Bottom Tab Navigation
```html
<nav class="tab-bar">
  <a href="/" class="tab-item active">
    <span class="tab-icon">🏠</span>
    <span class="tab-label">Home</span>
  </a>
  <a href="/search" class="tab-item">
    <span class="tab-icon">🔍</span>
    <span class="tab-label">Search</span>
  </a>
</nav>
```

```css
.tab-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  background: #fff;
  padding-bottom: env(safe-area-inset-bottom);
}

.tab-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 8px 4px;
  min-height: 60px;
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `font-size: 16px` on inputs to prevent zoom | Use smaller font sizes that trigger zoom |
| Set minimum 44px touch targets | Create tiny clickable areas |
| Include proper `inputmode` attributes | Rely only on `type` attribute |
| Use `loading="lazy"` for below-fold images | Load all images eagerly |
| Test on real devices | Only test in browser dev tools |
| Provide visual touch feedback | Leave buttons feeling unresponsive |

## Quick Fixes
- **iOS zoom on input focus** → Set `font-size: 16px` or larger on inputs
- **Horizontal scrolling** → Add `overflow-x: hidden` to body and check max-widths
- **Tiny touch targets** → Ensure minimum 44px x 44px clickable area
- **Wrong mobile keyboard** → Add proper `inputmode` and `type` attributes
- **Images not responsive** → Add `max-width: 100%; height: auto;`
- **Navigation not accessible** → Add proper ARIA labels and keyboard support

## Gotchas
⚠️ **Font size zoom**: iOS Safari zooms when input font-size is below 16px - always use 16px or larger  
⚠️ **Viewport units**: `vh` can be unreliable on mobile due to browser UI changes - use `dvh` when supported  
⚠️ **Touch delays**: iOS has 300ms click delay - use `touch-action: manipulation` to prevent  
⚠️ **Safe areas**: Account for notches and home indicators with `env(safe-area-inset-*)`  
⚠️ **Hover states**: Mobile doesn't have true hover - use `:active` or JavaScript for touch feedback

## Checklist
- [ ] Viewport meta tag configured with `width=device-width, initial-scale=1.0`
- [ ] All touch targets minimum 44px x 44px
- [ ] Form inputs use 16px+ font-size and proper input modes
- [ ] Images are responsive with proper srcset/sizes
- [ ] Navigation works with touch and provides visual feedback
- [ ] Tested on actual mobile devices, not just browser dev tools
- [ ] Safe area insets considered for modern devices with notches
- [ ] Loading performance optimized with lazy loading and modern image formats