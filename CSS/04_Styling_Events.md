# Styling Elements `v 0.07.25.01`

## Essentials

```css
/* Essential element styling patterns */
a {
  color: #0066cc;
  text-decoration: none;
  transition: color 0.2s ease;
}
a:hover { color: #004499; }

button {
  background: #007bff;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 4px;
  cursor: pointer;
}

input[type="text"] {
  border: 1px solid #ddd;
  padding: 8px 12px;
  border-radius: 4px;
  font-size: 16px;
}
```

**Key Concepts**: Pseudo-classes | Form Styling | Interactive States | Custom Controls
**When to use**: Creating consistent, accessible UI components and interactive elements

## Syntax Reference

### Link Styling

```css
/* Link States (order matters: LVHA) */
a:link {    /* unvisited link */
  color: #0066cc;
  text-decoration: none;
}

a:visited { /* visited link */
  color: #551a8b;
}

a:hover {   /* mouse over */
  color: #004499;
  text-decoration: underline;
}

a:active {  /* clicked/pressed */
  color: #cc0000;
}

/* Modern link patterns */
.nav-link {
  color: #333;
  text-decoration: none;
  padding: 8px 16px;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.nav-link:hover {
  background-color: #f5f5f5;
  color: #007bff;
}

/* Link with underline animation */
.animated-link {
  position: relative;
  text-decoration: none;
}

.animated-link::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background: #007bff;
  transition: width 0.3s ease;
}

.animated-link:hover::after {
  width: 100%;
}
```

### Button Styling

```css
/* Base button styles */
.btn {
  display: inline-block;
  padding: 12px 24px;
  font-size: 16px;
  font-weight: 500;
  text-align: center;
  text-decoration: none;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s ease;
  user-select: none;
}

/* Button variants */
.btn-primary {
  background-color: #007bff;
  color: white;
}

.btn-primary:hover {
  background-color: #0056b3;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0,123,255,0.3);
}

.btn-secondary {
  background-color: transparent;
  color: #007bff;
  border: 2px solid #007bff;
}

.btn-secondary:hover {
  background-color: #007bff;
  color: white;
}

/* Button states */
.btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
  opacity: 0.6;
}

.btn:active {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}

/* Button sizes */
.btn-sm { padding: 8px 16px; font-size: 14px; }
.btn-lg { padding: 16px 32px; font-size: 18px; }

/* Icon buttons */
.btn-icon {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}
```

### Form Element Styling

```css
/* Input fields */
input[type="text"],
input[type="email"],
input[type="password"],
textarea {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  font-family: inherit;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

/* Focus states */
input:focus,
textarea:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 3px rgba(0,123,255,0.1);
}

/* Invalid states */
input:invalid {
  border-color: #dc3545;
}

input:invalid:focus {
  box-shadow: 0 0 0 3px rgba(220,53,69,0.1);
}

/* Select dropdown */
select {
  padding: 12px 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
  font-size: 16px;
  cursor: pointer;
}

/* Textarea specific */
textarea {
  resize: vertical;
  min-height: 100px;
}

/* Form labels */
label {
  display: block;
  margin-bottom: 4px;
  font-weight: 500;
  color: #333;
}

/* Input groups */
.form-group {
  margin-bottom: 20px;
}

.input-group {
  display: flex;
  align-items: stretch;
}

.input-group input {
  border-radius: 4px 0 0 4px;
}

.input-group .btn {
  border-radius: 0 4px 4px 0;
}
```

### Custom Checkboxes and Radios

```css
/* Custom checkbox */
.custom-checkbox {
  position: relative;
  display: inline-block;
  cursor: pointer;
  user-select: none;
}

.custom-checkbox input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.custom-checkbox .checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  background-color: white;
  border: 2px solid #ddd;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.custom-checkbox:hover input ~ .checkmark {
  border-color: #007bff;
}

.custom-checkbox input:checked ~ .checkmark {
  background-color: #007bff;
  border-color: #007bff;
}

.custom-checkbox input:checked ~ .checkmark::after {
  content: '';
  position: absolute;
  left: 6px;
  top: 2px;
  width: 6px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

/* Custom radio button */
.custom-radio {
  position: relative;
  display: inline-block;
  cursor: pointer;
  user-select: none;
}

.custom-radio input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.custom-radio .radiomark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  background-color: white;
  border: 2px solid #ddd;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.custom-radio input:checked ~ .radiomark {
  background-color: #007bff;
  border-color: #007bff;
}

.custom-radio input:checked ~ .radiomark::after {
  content: '';
  position: absolute;
  top: 4px;
  left: 4px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}

/* Toggle switch */
.toggle-switch {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 24px;
}

.toggle-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.toggle-slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  border-radius: 24px;
  transition: 0.4s;
}

.toggle-slider::before {
  position: absolute;
  content: '';
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  border-radius: 50%;
  transition: 0.4s;
}

.toggle-switch input:checked + .toggle-slider {
  background-color: #007bff;
}

.toggle-switch input:checked + .toggle-slider::before {
  transform: translateX(26px);
}
```

### Form Layouts

```css
/* Inline forms */
.form-inline {
  display: flex;
  align-items: center;
  gap: 12px;
}

.form-inline .form-group {
  margin-bottom: 0;
}

/* Grid form layout */
.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

/* Full-width form */
.form-full {
  max-width: 600px;
  margin: 0 auto;
}

/* Floating labels */
.floating-label {
  position: relative;
}

.floating-label input {
  padding-top: 20px;
}

.floating-label label {
  position: absolute;
  left: 16px;
  top: 12px;
  color: #999;
  transition: all 0.2s ease;
  pointer-events: none;
}

.floating-label input:focus + label,
.floating-label input:not(:placeholder-shown) + label {
  top: 4px;
  font-size: 12px;
  color: #007bff;
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Follow LVHA order for link states | Style visited links the same as unvisited |
| Use `font-size: 16px` on mobile inputs | Use smaller fonts that trigger zoom |
| Provide visual feedback for all states | Leave buttons/links without hover states |
| Make custom controls keyboard accessible | Hide default controls without alternatives |
| Use consistent spacing and sizing | Mix different button/input sizes randomly |
| Test with real content and long text | Design only for placeholder text |

## Quick Fixes

- **Mobile input zoom** → Set `font-size: 16px` or larger on inputs
- **Button text selection** → Add `user-select: none`
- **Checkbox not clickable** → Wrap in `<label>` or use `for` attribute
- **Focus outline missing** → Never use `outline: none` without custom focus styles
- **Form validation not working** → Check input `name` and `required` attributes
- **Custom radio/checkbox not accessible** → Include hidden native input for screen readers

## Gotchas

⚠️ **Link State Order**: Must follow LVHA (Link, Visited, Hover, Active) order or styles won't work correctly
⚠️ **iOS Safari Input Styling**: Some properties like `border-radius` are ignored unless `-webkit-appearance: none` is set
⚠️ **Checkbox Label Association**: Custom checkboxes need proper label association for accessibility
⚠️ **Button vs Input**: Use `<button>` for actions and `<input type="submit">` for form submission
⚠️ **Focus Indicators**: Never remove focus outlines without providing custom focus styles
⚠️ **Mobile Form Validation**: Browser validation popups may be poorly positioned on mobile

## Checklist

- [ ] All interactive elements have hover and focus states
- [ ] Custom form controls maintain keyboard accessibility
- [ ] Form inputs have proper labels and validation feedback
- [ ] Button states (disabled, loading) are clearly indicated
- [ ] Mobile input font-size is 16px or larger
- [ ] Color contrast meets WCAG guidelines (4.5:1 minimum)
- [ ] Forms work without JavaScript for basic functionality
- [ ] Custom controls are tested with screen readers