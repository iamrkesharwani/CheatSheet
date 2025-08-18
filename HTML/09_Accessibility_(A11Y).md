# HTML Accessibility (A11Y) & ARIA `v 2.1`

## Essentials
```html
<!-- Core accessibility principles -->
<main role="main" id="main-content">
  <h1>Page Title</h1>
  <button aria-label="Close dialog">×</button>
  <img src="chart.png" alt="Sales increased 25% in Q4">
  <div aria-live="polite" id="status"></div>
</main>
```

**Key Concepts**: WCAG 2.1 AA compliance | Semantic HTML first | Screen reader support | Keyboard navigation
**When to use**: Every website and web application to ensure equal access for all users

## Syntax Reference

### ARIA Labels and Descriptions
```html
<!-- aria-label: Provides accessible name -->
<button aria-label="Close dialog">×</button>
<input type="search" aria-label="Search products" placeholder="Keywords">

<!-- aria-labelledby: References labeling elements -->
<h2 id="settings-title">Account Settings</h2>
<div role="group" aria-labelledby="settings-title">Settings content</div>

<!-- aria-describedby: Additional description -->
<input type="password" aria-describedby="pwd-help">
<div id="pwd-help">Password must be 8+ characters</div>

<!-- Multiple references -->
<input type="email" 
       aria-labelledby="email-label" 
       aria-describedby="email-help email-error">
```

### ARIA States and Properties
```html
<!-- Dynamic states -->
<button aria-expanded="false" aria-controls="menu">Menu</button>
<ul id="menu" hidden>
  <li><a href="/home">Home</a></li>
</ul>

<!-- Form states -->
<input type="email" aria-required="true" aria-invalid="false">
<div role="checkbox" aria-checked="mixed" tabindex="0">Mixed state</div>
<button aria-pressed="false">Toggle Button</button>

<!-- Selection states -->
<div role="tab" aria-selected="true" tabindex="0">Active Tab</div>
<nav><a href="/products" aria-current="page">Products</a></nav>

<!-- Live regions -->
<div aria-live="polite" id="status">Status updates</div>
<div aria-live="assertive" id="errors">Urgent alerts</div>
```

### ARIA Roles
```html
<!-- Landmark roles -->
<header role="banner">Site header</header>
<nav role="navigation" aria-label="Main">Navigation</nav>
<main role="main">Primary content</main>
<aside role="complementary">Sidebar</aside>
<footer role="contentinfo">Site footer</footer>

<!-- Interactive roles -->
<div role="button" tabindex="0">Custom Button</div>
<div role="dialog" aria-modal="true">Modal Dialog</div>
<div role="tab" aria-selected="false">Tab Item</div>
<div role="tabpanel" aria-labelledby="tab1">Tab Content</div>

<!-- Widget roles -->
<div role="combobox" aria-expanded="false">Autocomplete</div>
<ul role="listbox">
  <li role="option" aria-selected="false">Option 1</li>
</ul>
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use semantic HTML before adding ARIA | Use ARIA to fix bad HTML structure |
| Include meaningful alt text for images | Use empty alt for informational images |
| Provide visible focus indicators | Remove focus outlines without replacement |
| Test with keyboard navigation | Assume mouse-only interaction |
| Use proper heading hierarchy (h1→h2→h3) | Skip heading levels or use multiple h1s |
| Include skip links for navigation | Force users to tab through entire nav |

## Quick Fixes
- **Screen reader can't understand element** → Add appropriate `role` attribute
- **Button/link not announced properly** → Add `aria-label` or improve visible text
- **Form field missing label** → Connect with `<label for="id">` or `aria-labelledby`
- **Dynamic content not announced** → Add `aria-live` region
- **Modal traps keyboard users** → Implement focus management and trap
- **Images not described** → Add descriptive `alt` text
- **Low contrast text** → Ensure 4.5:1 ratio (AA) or 3:1 for large text

## Gotchas
⚠️ **ARIA overrides semantics**: ARIA attributes override native HTML semantics - use sparingly and test thoroughly

⚠️ **aria-hidden on focusable elements**: Never use `aria-hidden="true"` on elements that can receive focus

⚠️ **Missing keyboard support**: Custom interactive elements need `tabindex` and keyboard event handlers

⚠️ **Incorrect live region usage**: `aria-live="assertive"` interrupts users - use only for critical alerts

⚠️ **Focus management**: Dynamic content changes require proper focus management to avoid disorienting users

## Image Accessibility
```html
<!-- Descriptive alt text -->
<img src="sales-chart.png" 
     alt="Sales increased 25% from Q3 to Q4, reaching $2.5M">

<!-- Functional alt text -->
<a href="/contact">
  <img src="phone-icon.png" alt="Contact us by phone">
</a>

<!-- Decorative images -->
<img src="decoration.png" alt="" role="presentation">
<span aria-hidden="true">👍</span>

<!-- Complex images -->
<img src="complex-chart.png" 
     alt="Revenue chart" 
     aria-describedby="chart-desc">
<div id="chart-desc">
  Monthly revenue from Jan-Dec 2024: Started at $100k, 
  peaked at $350k in July, ended at $280k.
</div>

<!-- Background images with content -->
<div style="background-image: url('hero.jpg')" 
     role="img" 
     aria-label="Mountain sunset with orange sky">
  <h1>Welcome</h1>
</div>
```

## Form Accessibility
```html
<!-- Proper labels -->
<label for="email">Email Address</label>
<input type="email" id="email" required aria-required="true">

<!-- Fieldsets for groups -->
<fieldset>
  <legend>Contact Preference</legend>
  <input type="radio" id="contact-email" name="contact" value="email">
  <label for="contact-email">Email</label>
  <input type="radio" id="contact-phone" name="contact" value="phone">
  <label for="contact-phone">Phone</label>
</fieldset>

<!-- Error messages -->
<input type="email" 
       aria-describedby="email-error" 
       aria-invalid="true">
<div id="email-error" role="alert">
  Please enter a valid email address
</div>

<!-- Required field indicators -->
<label for="name">
  Full Name <span aria-label="required">*</span>
</label>
<input type="text" id="name" required>
```

## Interactive Patterns
```html
<!-- Modal Dialog -->
<div role="dialog" 
     aria-modal="true" 
     aria-labelledby="dialog-title"
     aria-describedby="dialog-desc">
  <h2 id="dialog-title">Confirm Delete</h2>
  <p id="dialog-desc">This action cannot be undone</p>
  <button>Delete</button>
  <button>Cancel</button>
</div>

<!-- Tab Interface -->
<div role="tablist" aria-label="Settings">
  <button role="tab" aria-selected="true" aria-controls="panel1">
    Profile
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel2">
    Security
  </button>
</div>
<div role="tabpanel" id="panel1" aria-labelledby="tab1">
  Profile settings...
</div>

<!-- Combobox (Autocomplete) -->
<label for="combo">Choose fruit</label>
<input type="text" 
       role="combobox" 
       aria-expanded="false"
       aria-autocomplete="list" 
       aria-owns="listbox" 
       id="combo">
<ul role="listbox" id="listbox" hidden>
  <li role="option">Apple</li>
  <li role="option">Banana</li>
</ul>

<!-- Disclosure Widget -->
<button aria-expanded="false" aria-controls="details">
  Show Details
</button>
<div id="details" hidden>
  Additional information...
</div>
```

## Keyboard Navigation
```html
<!-- Custom button with keyboard support -->
<div role="button" 
     tabindex="0" 
     onclick="action()"
     onkeydown="handleKey(event)">
  Custom Button
</div>

<script>
function handleKey(event) {
  if (event.key === 'Enter' || event.key === ' ') {
    event.preventDefault();
    action();
  }
}
</script>

<!-- Skip links -->
<a href="#main" class="skip-link">Skip to main content</a>
<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 6px;
  background: #000;
  color: #fff;
  padding: 8px;
  z-index: 1000;
}
.skip-link:focus { top: 6px; }
</style>
```

## Heading Structure
```html
<!-- Proper hierarchy -->
<h1>Main Page Title</h1>
  <h2>Section Title</h2>
    <h3>Subsection Title</h3>
      <h4>Sub-subsection Title</h4>

<!-- Article structure -->
<article>
  <h1>Article Title</h1>
  <h2>Introduction</h2>
  <h2>Main Content</h2>
    <h3>Subtopic 1</h3>
    <h3>Subtopic 2</h3>
      <h4>Detail Point</h4>
  <h2>Conclusion</h2>
</article>
```

## Screen Reader Support
```html
<!-- Screen reader only content -->
<style>
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}
</style>

<h2>Products <span class="sr-only">(5 items)</span></h2>
<a href="report.pdf">
  Download Report <span class="sr-only">(PDF, 2.3MB)</span>
</a>

<!-- Table headers -->
<table>
  <caption>Quarterly Sales Data</caption>
  <thead>
    <tr>
      <th scope="col">Quarter</th>
      <th scope="col">Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Q1 2024</th>
      <td>$150,000</td>
    </tr>
  </tbody>
</table>
```

## Checklist
- [ ] All images have appropriate alt text or are marked decorative
- [ ] Form fields have associated labels
- [ ] Heading hierarchy is logical (h1→h2→h3, no skipping)
- [ ] Color contrast meets WCAG AA standards (4.5:1)
- [ ] All interactive elements are keyboard accessible
- [ ] Focus indicators are visible and clear
- [ ] Skip links are present for navigation
- [ ] ARIA attributes are used correctly and sparingly
- [ ] Live regions announce dynamic content changes
- [ ] Error messages are properly associated with form fields
- [ ] Modal dialogs trap focus and manage keyboard navigation
- [ ] Custom interactive elements have appropriate roles and states
- [ ] Page has proper landmark structure (header, nav, main, footer)
- [ ] Content is organized with semantic HTML elements
- [ ] Auto-playing media has controls and can be paused