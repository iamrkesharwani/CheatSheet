# CSS Transitions & Animations `v CSS3+`

## Essentials

```css
/* Smooth transitions */
.button {
  transition: background-color 0.3s ease;
}
.button:hover {
  background-color: #007bff;
}

/* Transform properties */
.card:hover {
  transform: scale(1.05) translateY(-5px);
}

/* Keyframe animations */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
.modal {
  animation: fadeIn 0.5s ease-out;
}
```

**Key Concepts**: Smooth Transitions | Transform Effects | Keyframe Animations
**When to use**: Enhance user experience with smooth state changes and engaging visual effects

## Syntax Reference

### Basic Transition Structure

```css
/* Full transition syntax */
.element {
  transition: property duration timing-function delay;
}

/* Multiple properties */
.element {
  transition: 
    background-color 0.3s ease,
    transform 0.2s ease-out 0.1s,
    opacity 0.4s linear;
}

/* Shorthand for all properties */
.element {
  transition: all 0.3s ease;
}
```

### Transform Properties

```css
/* Scale transformations */
.grow:hover { transform: scale(1.1); }
.shrink:hover { transform: scale(0.9); }
.stretch:hover { transform: scaleX(1.2) scaleY(0.8); }

/* Translation (movement) */
.slide-right:hover { transform: translateX(20px); }
.slide-up:hover { transform: translateY(-10px); }
.slide-diagonal:hover { transform: translate(15px, -10px); }

/* Rotation */
.rotate:hover { transform: rotate(45deg); }
.spin:hover { transform: rotate(360deg); }

/* Combining transforms */
.fancy:hover { 
  transform: scale(1.1) rotate(5deg) translateY(-5px); 
}
```

### Animation Structure

```css
/* Define keyframes */
@keyframes slideIn {
  0% { 
    transform: translateX(-100%); 
    opacity: 0; 
  }
  50% { 
    transform: translateX(10px); 
  }
  100% { 
    transform: translateX(0); 
    opacity: 1; 
  }
}

/* Apply animation */
.animated-element {
  animation: slideIn 0.6s ease-out;
}
```

### Animation Properties

```css
.element {
  animation-name: bounceIn;
  animation-duration: 1s;
  animation-timing-function: ease-out;
  animation-delay: 0.2s;
  animation-iteration-count: 2;
  animation-direction: alternate;
  animation-fill-mode: forwards;
  animation-play-state: running;
}

/* Shorthand */
.element {
  animation: bounceIn 1s ease-out 0.2s 2 alternate forwards;
}
```

## Common Patterns

```css
/* Hover button effect */
.btn {
  transition: all 0.3s ease;
  transform: translateY(0);
}
.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

/* Loading spinner */
@keyframes spin {
  to { transform: rotate(360deg); }
}
.spinner {
  animation: spin 1s linear infinite;
}

/* Fade in on scroll */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.fade-in {
  animation: fadeInUp 0.6s ease-out;
}

/* Pulse effect */
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}
.notification {
  animation: pulse 2s ease-in-out infinite;
}

/* Slide navigation */
.nav-menu {
  transform: translateX(-100%);
  transition: transform 0.3s ease-in-out;
}
.nav-menu.open {
  transform: translateX(0);
}
```

## Timing Functions

```css
/* Built-in easing functions */
.linear { transition-timing-function: linear; }
.ease { transition-timing-function: ease; }
.ease-in { transition-timing-function: ease-in; }
.ease-out { transition-timing-function: ease-out; }
.ease-in-out { transition-timing-function: ease-in-out; }

/* Custom cubic-bezier */
.custom { 
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.265, 1.55); 
}

/* Steps for sprite animations */
.sprite {
  animation-timing-function: steps(8, end);
}
```

## Do's & Don'ts

| ✅ Do                                    | ❌ Don't                               |
| ---------------------------------------- | -------------------------------------- |
| Use 0.2-0.5s for most UI transitions    | Make transitions too slow (>1s)       |
| Test on lower-end devices               | Animate too many elements at once     |
| Use transform for position changes      | Animate layout properties (width/height) |
| Add will-change for complex animations  | Leave will-change on after animation  |
| Respect prefers-reduced-motion          | Ignore accessibility preferences       |

## Quick Fixes

- **Animations not smooth** → Use `transform` instead of changing `left/top/width/height`
- **Animation jumps at start** → Set initial state in CSS, not just keyframes
- **Transition not working** → Check if property is animatable (not all CSS properties can transition)
- **Performance issues** → Add `will-change: transform` before animation, remove after
- **Animation stops abruptly** → Use `animation-fill-mode: forwards` to maintain end state

## Gotchas

⚠️ **Warning**: Animating `width`, `height`, `top`, `left` causes layout reflow - use `transform` instead
⚠️ **Warning**: `will-change` improves performance but uses more memory - remove when animation ends
⚠️ **Warning**: Some users prefer reduced motion - use `@media (prefers-reduced-motion: reduce)`
⚠️ **Warning**: Transforms create new stacking context - can affect z-index behavior

## Performance Optimization

```css
/* Respect user preferences */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* Optimize for performance */
.optimized {
  will-change: transform; /* Before animation */
  transform: translateZ(0); /* Force hardware acceleration */
}

/* Remove will-change after animation */
.optimized.animation-complete {
  will-change: auto;
}
```

## Advanced Patterns

```css
/* Staggered animations */
.list-item:nth-child(1) { animation-delay: 0.1s; }
.list-item:nth-child(2) { animation-delay: 0.2s; }
.list-item:nth-child(3) { animation-delay: 0.3s; }

/* Chained animations */
@keyframes slideAndFade {
  0% { 
    transform: translateX(-100%); 
    opacity: 0; 
  }
  50% { 
    transform: translateX(0); 
    opacity: 0.5; 
  }
  100% { 
    transform: translateX(0); 
    opacity: 1; 
  }
}

/* Hover group effects */
.card-group:hover .card:not(:hover) {
  transform: scale(0.95);
  opacity: 0.5;
}
```

## Checklist

- [ ] Used transform properties for position/scale changes instead of layout properties
- [ ] Added appropriate transition durations (0.2-0.5s for UI elements)
- [ ] Tested animations on slower devices for performance
- [ ] Included prefers-reduced-motion media query for accessibility
- [ ] Set initial states for animated elements to prevent jumps
- [ ] Used will-change judiciously and removed after animations complete