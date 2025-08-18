# HTML Attributes `v 5.0`

## Essentials
```html
<!-- Core attributes every element can use -->
<div id="unique-id" class="style-group" title="Tooltip text">
  <p data-user="123" lang="en" hidden>Custom data storage</p>
</div>
```

**Key Concepts**: Global attributes | Form validation | Accessibility | Media control | Custom data
**When to use**: To modify element behavior, store data, improve accessibility, and enhance user experience

## Syntax Reference

### Global Attributes
```html
<!-- Identification and styling -->
<div id="header" class="container large">Content</div>

<!-- Custom data storage -->
<button data-user-id="123" data-action="delete">Delete</button>

<!-- Accessibility and interaction -->
<input type="text" title="Enter your name" tabindex="1" 
       aria-label="Full name" contenteditable="true">

<!-- Language and direction -->
<p lang="fr" dir="rtl">Texte français</p>

<!-- Visibility control -->
<div hidden draggable="true">Hidden draggable content</div>
```

### Form Attributes
```html
<!-- Validation -->
<input type="email" required 
       pattern="[^@]+@[^@]+\.[^@]+" 
       minlength="5" maxlength="50"
       title="Enter valid email">

<!-- State control -->
<input type="text" disabled readonly 
       placeholder="Cannot edit"
       autocomplete="off">

<!-- Range and limits -->
<input type="number" min="1" max="100" step="5">
<input type="date" min="2024-01-01" max="2024-12-31">

<!-- Multiple selections -->
<input type="file" multiple accept=".pdf,.doc">
<select multiple size="5">
  <option value="1">Option 1</option>
</select>
```

### Media Attributes
```html
<!-- Images -->
<img src="photo.jpg" alt="Description" 
     width="300" height="200"
     loading="lazy" decoding="async"
     srcset="small.jpg 400w, large.jpg 800w"
     sizes="(max-width: 600px) 400px, 800px">

<!-- Video -->
<video controls autoplay muted loop
       width="640" height="480"
       poster="preview.jpg" preload="metadata">
  <source src="video.mp4" type="video/mp4">
</video>

<!-- Audio -->
<audio controls preload="none" loop>
  <source src="audio.mp3" type="audio/mpeg">
</audio>
```

### Link Attributes
```html
<!-- External links -->
<a href="https://example.com" target="_blank" 
   rel="noopener noreferrer" title="Visit example">Link</a>

<!-- Download links -->
<a href="file.pdf" download="report.pdf">Download</a>

<!-- Communication -->
<a href="mailto:user@example.com?subject=Hello">Email</a>
<a href="tel:+1234567890">Call</a>
<a href="sms:+1234567890">SMS</a>
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `alt` attribute for all meaningful images | Use empty `alt=""` for decorative images without reason |
| Include `title` for additional context/tooltips | Duplicate visible text in `title` attribute |
| Use `data-*` attributes for custom data | Store sensitive data in `data-*` attributes |
| Add `rel="noopener noreferrer"` to external links | Open external links in same window without warning |
| Use semantic `type` attributes on inputs | Use generic `type="text"` when specific types exist |
| Include `lang` attribute for non-English content | Forget to specify document language in `<html>` |

## Quick Fixes
- **Form not validating** → Add `required`, `pattern`, or `type` attributes
- **Images not accessible** → Add descriptive `alt` text
- **External links insecure** → Add `rel="noopener noreferrer"`
- **Poor mobile experience** → Use `viewport` meta and responsive attributes
- **Screen reader confusion** → Add `aria-label`, `aria-describedby`, `role` attributes
- **Tooltips not showing** → Check `title` attribute spelling and placement

## Gotchas
⚠️ **ID uniqueness**: Each `id` must be unique on the page - duplicates break JavaScript and CSS targeting

⚠️ **Data attribute naming**: Use lowercase with hyphens (`data-user-id`), not camelCase (`data-userId`)

⚠️ **Boolean attributes**: Attributes like `disabled`, `checked`, `hidden` are true if present, false if absent

⚠️ **Validation attributes**: HTML validation is client-side only - always validate server-side for security

⚠️ **ARIA overrides**: ARIA attributes override semantic HTML - use sparingly and test with screen readers

## Interactive Elements
```html
<!-- Button types -->
<button type="submit">Submit Form</button>
<button type="reset">Reset Form</button>
<button type="button" onclick="action()">Custom Action</button>

<!-- Progress indicators -->
<progress value="70" max="100">70%</progress>
<meter value="85" min="0" max="100" high="90">85%</meter>

<!-- Expandable content -->
<details open>
  <summary>Click to expand</summary>
  <p>Hidden content revealed</p>
</details>

<!-- Input suggestions -->
<input list="browsers" name="browser">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
</datalist>
```

## ARIA Accessibility
```html
<!-- Labels and descriptions -->
<button aria-label="Close dialog">×</button>
<input type="password" aria-describedby="pwd-help">
<div id="pwd-help">Must be 8+ characters</div>

<!-- Dynamic content -->
<div aria-live="polite" id="status">Status updates</div>
<div aria-live="assertive" id="errors">Error messages</div>

<!-- Interactive states -->
<button aria-expanded="false" aria-controls="menu">Menu</button>
<div id="menu" role="menu" hidden>Menu items</div>

<!-- Hide decorative elements -->
<span aria-hidden="true">👍</span>
```

## Table Structure
```html
<table>
  <caption>Sales Report Q1 2024</caption>
  <thead>
    <tr>
      <th scope="col" id="month">Month</th>
      <th scope="col" id="sales">Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">January</th>
      <td headers="month sales">$10,000</td>
    </tr>
    <tr>
      <td colspan="2">Total: $22,000</td>
    </tr>
  </tbody>
</table>
```

## Checklist
- [ ] All images have descriptive `alt` attributes
- [ ] Forms use appropriate `type` attributes for inputs
- [ ] Required form fields have `required` attribute
- [ ] External links include security attributes (`rel="noopener noreferrer"`)
- [ ] Interactive elements have proper ARIA labels
- [ ] Custom data uses `data-*` attributes appropriately
- [ ] Language is specified with `lang` attribute
- [ ] Unique `id` values throughout the document
- [ ] Tables have proper header associations (`scope`, `headers`)
- [ ] Media elements include fallback content
- [ ] Keyboard navigation works with `tabindex`
- [ ] Dynamic content uses `aria-live` regions