# HTML Code Quality `v 0.07.25.01`

## Essentials
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Required Page Title</title>
</head>
<body>
  <main>
    <h1>Main Heading</h1>
    <p>Content with proper semantic structure</p>
  </main>
</body>
</html>
```

**Key Concepts**: W3C Validation | Semantic HTML | Clean Structure | Proper Nesting | Cross-Browser Support
**When to use**: Every HTML document to ensure quality, accessibility, and maintainability

## HTML Validation

### Document Structure Requirements
```html
<!-- Essential document foundation -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title (Required)</title>
</head>
<body>
  <!-- Content here -->
</body>
</html>
```

### Required Attributes Checklist
```html
<!-- Images require alt attributes -->
<img src="photo.jpg" alt="Description of image">

<!-- Form inputs need type and name -->
<input type="email" name="email" id="email">

<!-- Labels must reference inputs -->
<label for="email">Email Address</label>

<!-- Forms should have action -->
<form action="/submit" method="post">
  <!-- Form controls -->
</form>

<!-- HTML element needs lang -->
<html lang="en">
```

### Common Validation Errors
```html
<!-- ❌ Missing DOCTYPE -->
<html>  <!-- Should be preceded by <!DOCTYPE html> -->

<!-- ❌ Unclosed tags -->
<p>Text without closing tag
<div>Another unclosed element

<!-- ❌ Improper nesting -->
<p><div>Block inside inline</div></p>  <!-- Invalid -->

<!-- ❌ Missing required attributes -->
<img src="photo.jpg">  <!-- Missing alt attribute -->
<input type="text">    <!-- Missing name attribute -->
```

## Semantic HTML Structure

### Document Layout Elements
```html
<header>     <!-- Page/section header -->
  <nav>      <!-- Primary navigation -->
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>

<main>       <!-- Primary content (one per page) -->
  <section>  <!-- Thematic content grouping -->
    <article><!-- Self-contained content -->
      <h1>Article Title</h1>
      <p>Article content...</p>
    </article>
  </section>
  
  <aside>    <!-- Sidebar/related content -->
    <h2>Related Links</h2>
  </aside>
</main>

<footer>     <!-- Page/section footer -->
  <p>&copy; 2024 Company Name</p>
</footer>
```

### Content Semantics
```html
<!-- Headings: Don't skip levels -->
<h1>Main Title</h1>
  <h2>Section Title</h2>
    <h3>Subsection Title</h3>

<!-- Text meaning -->
<p>Regular paragraph with <strong>important</strong> and <em>emphasized</em> text.</p>
<blockquote cite="https://source.com">
  <p>Long quotation content</p>
</blockquote>
<p>He said, <q>This is a short quote</q> during the meeting.</p>

<!-- Time and references -->
<time datetime="2024-01-15">January 15, 2024</time>
<cite>Book Title</cite>
<abbr title="HyperText Markup Language">HTML</abbr>
```

### Form Semantics
```html
<form action="/submit" method="post">
  <fieldset>
    <legend>Personal Information</legend>
    
    <div class="form-group">
      <label for="name">Full Name</label>
      <input type="text" id="name" name="name" required>
    </div>
    
    <div class="form-group">
      <label for="email">Email</label>
      <input type="email" id="email" name="email" required>
    </div>
  </fieldset>
  
  <button type="submit">Submit Form</button>
</form>
```

### List Semantics
```html
<!-- Unordered list for related items -->
<ul>
  <li>First item</li>
  <li>Second item</li>
</ul>

<!-- Ordered list for sequences -->
<ol>
  <li>Step one</li>
  <li>Step two</li>
</ol>

<!-- Description list for term-definition pairs -->
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language</dd>
  
  <dt>CSS</dt>
  <dd>Cascading Style Sheets</dd>
</dl>
```

## Clean Code Structure

### Naming Conventions
```html
<!-- ✅ Good: Descriptive and consistent -->
<div id="user-profile" class="profile-container">
  <h2 class="profile-title">User Information</h2>
  <input id="email-address" name="email" class="form-input">
  <button id="submit-form" class="btn btn-primary">Save</button>
</div>

<!-- ❌ Bad: Vague and inconsistent -->
<div id="d1" class="red-box">
  <h2 class="big">Info</h2>
  <input id="txt1" name="e" class="field">
  <button id="btn" class="button1">OK</button>
</div>
```

### Indentation Standards
```html
<!-- Consistent 2-space indentation -->
<section class="hero">
  <div class="container">
    <h1 class="hero-title">Welcome</h1>
    <p class="hero-text">
      Description with proper indentation
    </p>
    <div class="hero-actions">
      <button class="btn btn-primary">Get Started</button>
      <button class="btn btn-secondary">Learn More</button>
    </div>
  </div>
</section>

<!-- Multi-line attributes aligned -->
<input
  type="email"
  id="user-email"
  name="email"
  placeholder="Enter your email"
  class="form-input"
  required>
```

### Content Organization
```html
<!-- Group related content logically -->
<section class="contact-info">
  <h2>Contact Information</h2>
  
  <address>
    <p>123 Main Street<br>
       City, State 12345</p>
    <p>Phone: <a href="tel:+1234567890">(123) 456-7890</a></p>
    <p>Email: <a href="mailto:info@example.com">info@example.com</a></p>
  </address>
  
  <div class="business-hours">
    <h3>Hours</h3>
    <dl>
      <dt>Monday - Friday</dt>
      <dd>9:00 AM - 5:00 PM</dd>
      
      <dt>Saturday</dt>
      <dd>10:00 AM - 3:00 PM</dd>
    </dl>
  </div>
</section>
```

## Proper Nesting Rules

### Block vs Inline Nesting
```html
<!-- ✅ Correct: Block elements can contain inline -->
<div class="content">
  <p>Text with <strong>bold</strong> and <em>italic</em> formatting.</p>
  <blockquote>
    <p>Quote with <cite>citation</cite></p>
  </blockquote>
</div>

<!-- ❌ Wrong: Inline cannot contain block -->
<p>
  <div>Block inside paragraph</div>  <!-- Invalid -->
</p>

<span>
  <h2>Heading inside span</h2>      <!-- Invalid -->
</span>
```

### Interactive Element Nesting
```html
<!-- ✅ Correct button structure -->
<button type="button" class="icon-button">
  <span class="icon" aria-hidden="true">🔍</span>
  <span class="button-text">Search</span>
</button>

<!-- ❌ Wrong: Interactive inside interactive -->
<button type="button">
  <a href="/link">Link inside button</a>     <!-- Invalid -->
</button>

<a href="/page">
  <button type="button">Button in link</button>  <!-- Invalid -->
</a>
```

### Form Element Nesting
```html
<!-- ✅ Proper form structure -->
<form action="/submit" method="post">
  <fieldset>
    <legend>User Registration</legend>
    
    <div class="form-row">
      <div class="form-group">
        <label for="first-name">First Name</label>
        <input type="text" id="first-name" name="firstName" required>
      </div>
      
      <div class="form-group">
        <label for="last-name">Last Name</label>
        <input type="text" id="last-name" name="lastName" required>
      </div>
    </div>
  </fieldset>
  
  <button type="submit">Register</button>
</form>
```

## Effective Comments

### Section Comments
```html
<!-- ===== HEADER SECTION ===== -->
<header class="site-header">
  <div class="container">
    <!-- Logo and main navigation -->
    <nav class="main-nav">
      <!-- Navigation items -->
    </nav>
  </div>
</header>
<!-- ===== END HEADER ===== -->

<!-- ===== MAIN CONTENT ===== -->
<main class="main-content">
  <!-- Page content -->
</main>
```

### Component Comments
```html
<!-- 
  PRODUCT CARD COMPONENT
  - Displays product information in grid layout
  - Requires: product-card.css, product-card.js
  - Updated: 2024-01-15
  - Dependencies: None
-->
<article class="product-card" data-product-id="123">
  <img src="product.jpg" alt="Product name" class="product-image">
  <div class="product-info">
    <h3 class="product-title">Product Name</h3>
    <p class="product-price">$99.99</p>
  </div>
</article>
```

### Functional Comments
```html
<!-- TODO: Replace with dynamic content from API -->
<section class="news-feed">
  <h2>Latest News</h2>
  <!-- Static content - will be replaced with dynamic loading -->
  <p>Loading news articles...</p>
</section>

<!-- NOTE: Only visible to authenticated users -->
<div class="user-dashboard" data-auth-required="true">
  <!-- Dashboard content -->
</div>

<!-- FIXME: Add proper ARIA labels for screen readers -->
<div class="image-carousel" role="region">
  <!-- Carousel slides -->
</div>

<!-- WARNING: This section uses deprecated HTML attributes -->
<table border="1" cellpadding="5">
  <!-- Table content - needs modernization -->
</table>
```

## Cross-Browser Compatibility

### Essential Meta Tags
```html
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cross-Browser Compatible Page</title>
</head>
```

### HTML5 Elements with Roles
```html
<!-- Add role attributes for older browser support -->
<header role="banner">
<nav role="navigation">
<main role="main">
<section role="region" aria-labelledby="section-title">
<article role="article">
<aside role="complementary">
<footer role="contentinfo">
```

### Form Input Fallbacks
```html
<!-- Email with pattern fallback -->
<input 
  type="email" 
  name="email"
  pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$"
  title="Please enter a valid email address"
  placeholder="user@example.com">

<!-- Date with placeholder -->
<input 
  type="date" 
  name="birthdate"
  placeholder="MM/DD/YYYY"
  pattern="\d{2}/\d{2}/\d{4}">

<!-- Number with validation -->
<input 
  type="number" 
  name="age"
  min="1" 
  max="120"
  pattern="[0-9]+"
  title="Please enter a valid age">
```

### Media Elements with Fallbacks
```html
<!-- Video with multiple formats -->
<video controls>
  <source src="video.webm" type="video/webm">
  <source src="video.mp4" type="video/mp4">
  <source src="video.ogv" type="video/ogg">
  <p>Your browser doesn't support video. 
     <a href="video.mp4">Download the video</a>.</p>
</video>

<!-- Picture with format fallbacks -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <source srcset="image.jpg" type="image/jpeg">
  <img src="image.jpg" alt="Fallback image description">
</picture>
```

### Progressive Enhancement
```html
<!-- Details/summary with JavaScript fallback -->
<details class="expandable-section">
  <summary>Click to expand</summary>
  <div class="details-content">
    <p>Expandable content that works without JavaScript</p>
  </div>
</details>

<!-- Noscript fallback -->
<noscript>
  <div class="no-js-message">
    <p>This site works best with JavaScript enabled.</p>
  </div>
</noscript>
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use semantic HTML elements for their intended purpose | Use divs for everything |
| Include alt attributes on all images | Leave alt attributes empty without reason |
| Validate HTML with W3C validator | Skip validation testing |
| Use consistent indentation (2 or 4 spaces) | Mix tabs and spaces |
| Write descriptive comments for complex sections | Over-comment obvious code |
| Test in multiple browsers | Only test in one browser |

## Quick Fixes
- **HTML not validating** → Check for unclosed tags and missing required attributes
- **Poor accessibility** → Add semantic elements and proper ARIA labels  
- **Code hard to read** → Implement consistent indentation and naming
- **Browser compatibility issues** → Add fallbacks and test in target browsers
- **Form not working** → Ensure proper label-input associations
- **Images not loading** → Check alt attributes and file paths

## Gotchas
⚠️ **Skipping heading levels**: Always follow h1→h2→h3 hierarchy, don't skip from h1 to h3  
⚠️ **Interactive nesting**: Never put buttons inside links or links inside buttons  
⚠️ **Block in inline**: Inline elements like `<span>` cannot contain block elements like `<div>`  
⚠️ **Missing lang attribute**: Search engines and screen readers need `<html lang="en">`  
⚠️ **Form labels**: Every form input needs an associated label for accessibility  
⚠️ **Multiple main elements**: Only one `<main>` element per page is allowed

## Validation Tools
- **W3C Markup Validator**: https://validator.w3.org/
- **HTML5 Validator**: https://html5.validator.nu/
- **Browser Dev Tools**: Built-in validation in most browsers
- **VS Code Extensions**: HTMLHint, W3C Web Validator
- **Command Line**: html-validate, htmllint

## Checklist
- [ ] Document starts with `<!DOCTYPE html>`
- [ ] HTML element has `lang` attribute
- [ ] All images have meaningful `alt` attributes
- [ ] Forms have proper `label` elements
- [ ] Heading hierarchy follows logical order (h1→h2→h3)
- [ ] HTML validates with W3C validator
- [ ] Semantic elements used appropriately
- [ ] Consistent indentation throughout
- [ ] Comments added for complex sections
- [ ] Tested in target browsers
- [ ] No interactive elements nested inside each other
- [ ] Required attributes present on all elements