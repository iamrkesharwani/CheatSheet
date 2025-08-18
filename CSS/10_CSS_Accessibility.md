# CSS Accessibility `v 0.07.25.01`

## Essentials

```css
/* Visible focus indicators */
button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Multi-sensory feedback (not color-only) */
.error {
  color: #d73502;
  border-left: 4px solid #d73502;
  background: url('error-icon.svg') no-repeat left center;
}

/* Respect motion preferences */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

**Key Concepts**: Focus Management | Multi-Sensory Design | Motion Sensitivity | Color Independence | Keyboard Navigation
**When to use**: Every website - accessibility is not optional, it's fundamental to inclusive design

## Syntax Reference

### Focus Styles

```css
/* Modern focus-visible (keyboard only) */
button:focus-visible {
  outline: 2px solid #005fcc;
  outline-offset: 2px;
  border-radius: 4px;
}

/* Hide focus ring for mouse users */
button:focus:not(:focus-visible) {
  outline: none;
}

/* Custom focus styles */
.card:focus-visible {
  box-shadow: 0 0 0 3px rgba(0, 95, 204, 0.5);
  outline: none;
}

/* Skip links */
.skip-link {
  position: absolute;
  top: -40px;
  left: 6px;
  background: #000;
  color: #fff;
  padding: 8px;
  text-decoration: none;
  border-radius: 0 0 4px 4px;
}
.skip-link:focus {
  top: 0;
}
```

### Color-Independent Design

```css
/* Error state with multiple indicators */
.field-error {
  color: #d73502;                    /* Color cue */
  border: 2px solid #d73502;         /* Visual boundary */
  background: url('icon-error.svg') no-repeat right 8px center; /* Icon */
}
.field-error::before {
  content: "⚠️ ";                    /* Symbol */
}

/* Status indicators */
.status-success {
  color: #16a34a;
  background-color: #dcfce7;
  border-left: 4px solid #16a34a;
}
.status-success::before {
  content: "✓ ";
  font-weight: bold;
}

/* Interactive states beyond color */
.button-primary {
  background: #0066cc;
  border: 2px solid #0066cc;
}
.button-primary:hover {
  background: #0052a3;
  transform: translateY(-1px);       /* Subtle movement */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.button-primary:disabled {
  background: #94a3b8;
  border-color: #94a3b8;
  cursor: not-allowed;
  opacity: 0.6;                      /* Visual dimming */
}
```

### Reduced Motion

```css
/* Global motion reset */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* Selective motion reduction */
@media (prefers-reduced-motion: reduce) {
  .parallax {
    transform: none !important;
  }
  
  .auto-carousel {
    animation-play-state: paused;
  }
  
  .floating-animation {
    animation: none;
  }
}

/* Motion-safe animations */
@media (prefers-reduced-motion: no-preference) {
  .fade-in {
    animation: fadeIn 0.3s ease-in;
  }
  
  .slide-up {
    animation: slideUp 0.4s ease-out;
  }
}

/* Essential motion only */
.loading-spinner {
  animation: spin 1s linear infinite;
}
@media (prefers-reduced-motion: reduce) {
  .loading-spinner {
    animation: pulse 2s ease-in-out infinite;
  }
}
```

## Common Patterns

```css
/* Pattern 1: Accessible form validation */
.form-field {
  position: relative;
  margin-bottom: 1rem;
}
.form-field.error input {
  border: 2px solid #dc2626;
  background: #fef2f2;
}
.form-field.error::after {
  content: attr(data-error);
  position: absolute;
  bottom: -1.5rem;
  left: 0;
  color: #dc2626;
  font-size: 0.875rem;
  display: flex;
  align-items: center;
  gap: 0.25rem;
}
.form-field.error::before {
  content: "⚠️";
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
}

/* Pattern 2: Inclusive button states */
.btn {
  padding: 0.75rem 1.5rem;
  border: 2px solid transparent;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  position: relative;
}
.btn:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}
.btn:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
.btn:active:not(:disabled) {
  transform: translateY(0);
}
.btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  text-decoration: line-through;
}
@media (prefers-reduced-motion: reduce) {
  .btn {
    transition: none;
  }
  .btn:hover:not(:disabled) {
    transform: none;
  }
}

/* Pattern 3: Accessible data visualization */
.chart-bar {
  background: linear-gradient(45deg, #0066cc 25%, transparent 25%);
  border: 1px solid #0066cc;
  min-height: 20px;
  position: relative;
}
.chart-bar::after {
  content: attr(data-value);
  position: absolute;
  right: 4px;
  top: 50%;
  transform: translateY(-50%);
  font-weight: bold;
  color: #000;
}

/* Pattern 4: Motion-aware page transitions */
.page-transition {
  opacity: 1;
  transform: translateX(0);
}
@media (prefers-reduced-motion: no-preference) {
  .page-transition {
    transition: opacity 0.3s ease, transform 0.3s ease;
  }
  .page-transition.entering {
    opacity: 0;
    transform: translateX(20px);
  }
}
@media (prefers-reduced-motion: reduce) {
  .page-transition.entering {
    opacity: 0;
    transform: none;
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use focus-visible for keyboard-only focus indicators | Remove all focus indicators for aesthetics |
| Combine color with icons, text, or patterns | Rely solely on color to convey information |
| Test with prefers-reduced-motion enabled | Ignore motion sensitivity preferences |
| Provide multiple ways to identify states | Use only hover states for interactive feedback |
| Test with keyboard navigation only | Assume all users interact with a mouse |

## Quick Fixes

- **Focus ring invisible** → Check contrast ratio and outline-offset
- **Color-only error states** → Add icons, borders, or text indicators
- **Animations too fast** → Implement prefers-reduced-motion queries
- **Poor keyboard navigation** → Add visible focus styles and logical tab order
- **Low contrast focus** → Use high contrast colors (4.5:1 minimum ratio)

## Gotchas

⚠️ **Focus-visible Support**: Use focus polyfill for older browsers or provide focus fallback
⚠️ **Motion Preferences**: Users may have vestibular disorders - respect prefers-reduced-motion
⚠️ **Color Contrast**: Red/green combinations are problematic for colorblind users
⚠️ **Focus Traps**: Modal dialogs need proper focus management with JavaScript
⚠️ **Screen Readers**: CSS-generated content may not be announced - use aria-labels when needed

## Checklist

- [ ] All interactive elements have visible focus indicators
- [ ] Critical information doesn't rely on color alone
- [ ] Animations respect prefers-reduced-motion preferences
- [ ] Focus indicators meet WCAG contrast requirements (3:1 minimum)
- [ ] Error states include text descriptions, not just color changes
- [ ] Test keyboard navigation through entire interface
- [ ] Verify color combinations work for colorblind users
- [ ] Check that essential animations still function with reduced motion
- [ ] Ensure focus order follows logical reading sequence