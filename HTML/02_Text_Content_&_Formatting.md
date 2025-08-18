# HTML5 Complete Reference `v 5.2024`

## Essentials
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
</head>
<body>
    <h1>Main Heading</h1>
    <p>Content goes here</p>
</body>
</html>
```

**Key Concepts**: Semantic Elements | Accessibility | SEO Optimization | Forms | Media
**When to use**: Building modern, accessible, and SEO-friendly web pages and applications

## Document Structure

### Basic HTML Template
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Essential meta tags -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Page description under 160 characters">
    <title>Page Title - Site Name</title>
    
    <!-- External resources -->
    <link rel="stylesheet" href="styles.css">
    <link rel="icon" href="favicon.ico">
</head>
<body>
    <!-- Page content -->
</body>
</html>
```

### Semantic Structure
```html
<header>
    <nav><!-- Navigation --></nav>
</header>

<main>
    <article>
        <header>
            <h1>Article Title</h1>
            <time datetime="2024-01-01">January 1, 2024</time>
        </header>
        <p>Article content...</p>
    </article>
</main>

<aside>
    <!-- Sidebar content -->
</aside>

<footer>
    <!-- Footer content -->
</footer>
```

## Text Content and Structure

### Heading Hierarchy
```html
<!-- Proper heading structure (only one h1 per page) -->
<h1>Main Page Title</h1>
<h2>Section Heading</h2>
<h3>Subsection Heading</h3>
<h4>Sub-subsection</h4>
<h5>Minor Heading</h5>
<h6>Smallest Heading</h6>

<!-- Article structure example -->
<article>
    <h1>Web Development Guide</h1>
    <h2>Frontend Technologies</h2>
    <h3>HTML Basics</h3>
    <h3>CSS Styling</h3>
</article>
```

### Text Formatting
```html
<!-- Paragraphs and line breaks -->
<p>This is a paragraph of text.</p>
<p>Another paragraph with a<br>line break inside.</p>
<hr> <!-- Horizontal divider -->

<!-- Semantic formatting (meaningful) -->
<strong>Important text (bold + semantic meaning)</strong>
<em>Emphasized text (italic + semantic meaning)</em>

<!-- Visual formatting (appearance only) -->
<b>Bold text (visual only)</b>
<i>Italic text (visual only)</i>

<!-- Other text formatting -->
<mark>Highlighted text</mark>
<small>Fine print or side comments</small>
<del>Deleted text</del>
<ins>Inserted text</ins>
<u>Underlined text</u>
<s>Strikethrough text</s>
<sub>Subscript: H<sub>2</sub>O</sub>
<sup>Superscript: E=mc<sup>2</sup></sup>
```

### Quotations and Citations
```html
<!-- Block quotations -->
<blockquote cite="https://example.com">
    <p>To be or not to be, that is the question.</p>
    <footer>— William Shakespeare, <cite>Hamlet</cite></footer>
</blockquote>

<!-- Inline quotes -->
<p>Shakespeare wrote <q>All the world's a stage</q> in As You Like It.</p>

<!-- Citations (for work titles, not authors) -->
<p>More details in <cite>The HTML Specification</cite>.</p>
```

### Code and Technical Text
```html
<!-- Inline code -->
<p>Use the <code>console.log()</code> function to debug.</p>

<!-- Code blocks -->
<pre><code>
function greet(name) {
    return "Hello, " + name + "!";
}
</code></pre>

<!-- User input and system output -->
<p>Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to copy.</p>
<p>System response: <samp>File not found</samp></p>
<p>The <var>username</var> variable stores user data.</p>
```

## Lists and Hierarchical Content

### Unordered Lists
```html
<!-- Basic unordered list -->
<ul>
    <li>HTML basics</li>
    <li>CSS styling</li>
    <li>JavaScript fundamentals</li>
</ul>

<!-- Nested unordered lists -->
<ul>
    <li>Web Technologies
        <ul>
            <li>HTML</li>
            <li>CSS</li>
            <li>JavaScript</li>
        </ul>
    </li>
    <li>Backend Technologies
        <ul>
            <li>Node.js</li>
            <li>Python</li>
        </ul>
    </li>
</ul>
```

### Ordered Lists
```html
<!-- Basic ordered list -->
<ol>
    <li>Plan project</li>
    <li>Create wireframes</li>
    <li>Develop prototype</li>
    <li>Test and deploy</li>
</ol>

<!-- Advanced ordered list options -->
<ol start="5">
    <li>Fifth item</li>
    <li>Sixth item</li>
</ol>

<ol type="A">
    <li>Uppercase letters</li>
    <li>Second item</li>
</ol>

<ol type="I" start="3">
    <li>Roman numerals starting at III</li>
    <li>Then IV</li>
</ol>

<ol reversed>
    <li>Countdown list</li>
    <li>Second to last</li>
    <li>Last item</li>
</ol>
```

### Description Lists
```html
<!-- Term-definition pairs -->
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
    <dd>Used for styling web pages</dd>
    
    <dt>Frontend</dt>
    <dt>Client-side</dt>
    <dd>User-facing part of web applications</dd>
</dl>
```

### Complex List Structures
```html
<!-- Lists with rich content -->
<ul>
    <li>
        <strong>HTML Fundamentals</strong>
        <p>Learn the structure of web pages</p>
        <ol>
            <li>Elements and tags</li>
            <li>Attributes</li>
            <li>Document structure</li>
        </ol>
    </li>
    <li>
        <strong>CSS Basics</strong>
        <p>Style your web pages</p>
        <ul>
            <li>Selectors</li>
            <li>Properties</li>
            <li>Layout</li>
        </ul>
    </li>
</ul>
```

### Links and Navigation
```html
<!-- External link (safe) -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    External Link
</a>

<!-- Internal link -->
<a href="/page.html">Internal Page</a>

<!-- Email and phone -->
<a href="mailto:user@example.com">Email</a>
<a href="tel:+1234567890">Phone</a>

<!-- Skip navigation for accessibility -->
<a href="#main-content" class="skip-link">Skip to main content</a>
```

## Forms and Input

### Form Structure
```html
<form action="/submit" method="post" novalidate>
    <fieldset>
        <legend>Personal Information</legend>
        
        <label for="name">Name (required)</label>
        <input type="text" id="name" name="name" required>
        
        <label for="email">Email</label>
        <input type="email" id="email" name="email" required>
        
        <button type="submit">Submit</button>
    </fieldset>
</form>
```

### Input Types
```html
<!-- Text inputs -->
<input type="text" placeholder="Username">
<input type="password" placeholder="Password">
<input type="email" placeholder="user@example.com">
<input type="url" placeholder="https://example.com">
<input type="tel" placeholder="123-456-7890">

<!-- Numbers and dates -->
<input type="number" min="1" max="100" step="1">
<input type="range" min="0" max="100" value="50">
<input type="date">
<input type="time">

<!-- File and color -->
<input type="file" accept=".jpg,.png,.pdf" multiple>
<input type="color" value="#ff0000">
```

### Form Controls
```html
<!-- Radio buttons -->
<input type="radio" id="male" name="gender" value="male">
<label for="male">Male</label>

<!-- Checkboxes -->
<input type="checkbox" id="newsletter" name="newsletter" checked>
<label for="newsletter">Subscribe</label>

<!-- Select dropdown -->
<select name="country" required>
    <option value="">Choose country</option>
    <option value="us">United States</option>
    <option value="ca">Canada</option>
</select>

<!-- Textarea -->
<textarea name="message" rows="5" cols="50" 
          placeholder="Your message" required></textarea>
```

## Media Elements

### Images
```html
<!-- Basic image -->
<img src="image.jpg" alt="Descriptive text" loading="lazy">

<!-- Responsive images -->
<img src="image.jpg" alt="Description" 
     srcset="image-320w.jpg 320w,
             image-640w.jpg 640w,
             image-1024w.jpg 1024w"
     sizes="(max-width: 320px) 280px,
            (max-width: 640px) 600px,
            1024px">

<!-- Picture element -->
<picture>
    <source media="(min-width: 800px)" srcset="large.jpg">
    <source media="(min-width: 400px)" srcset="medium.jpg">
    <img src="small.jpg" alt="Description">
</picture>

<!-- Figure with caption -->
<figure>
    <img src="chart.png" alt="Sales data">
    <figcaption>Q4 Sales Performance</figcaption>
</figure>
```

### Audio and Video
```html
<!-- Audio element -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    Your browser doesn't support audio.
</audio>

<!-- Video element -->
<video controls width="640" height="360" poster="thumbnail.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <track src="subtitles.vtt" kind="subtitles" srclang="en" label="English">
    Video not supported.
</video>
```

## Tables

### Basic Table
```html
<table>
    <caption>Monthly Sales Data</caption>
    <thead>
        <tr>
            <th scope="col">Month</th>
            <th scope="col">Sales</th>
            <th scope="col">Growth</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">January</th>
            <td>$10,000</td>
            <td>5%</td>
        </tr>
        <tr>
            <th scope="row">February</th>
            <td>$12,000</td>
            <td>20%</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <th scope="row">Total</th>
            <td>$22,000</td>
            <td>12.5%</td>
        </tr>
    </tfoot>
</table>
```

### Advanced Table Features
```html
<!-- Spanning cells -->
<th colspan="2">Sales Data</th>
<td rowspan="3">Q1 Total</td>

<!-- Column groups -->
<colgroup>
    <col style="background-color: #f0f0f0">
    <col span="2" style="background-color: #e0e0e0">
</colgroup>
```

## SEO and Meta Tags

### Essential Meta Tags
```html
<head>
    <!-- Required -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title - Brand Name</title>
    <meta name="description" content="Page description under 160 chars">
    
    <!-- SEO -->
    <meta name="robots" content="index, follow">
    <link rel="canonical" href="https://example.com/page">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Page Title">
    <meta property="og:description" content="Page description">
    <meta property="og:image" content="https://example.com/image.jpg">
    <meta property="og:url" content="https://example.com/page">
    
    <!-- Twitter Cards -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="Page Title">
    <meta name="twitter:description" content="Page description">
</head>
```

## Accessibility Features

### ARIA Attributes
```html
<!-- Labels and descriptions -->
<button aria-label="Close dialog">×</button>
<div aria-describedby="help-text">Input field</div>
<div id="help-text">Helper text</div>

<!-- Live regions -->
<div aria-live="polite">Status updates</div>
<div aria-live="assertive">Error messages</div>

<!-- Navigation -->
<nav aria-label="Main navigation">
    <ul role="menubar">
        <li><a href="/" role="menuitem">Home</a></li>
    </ul>
</nav>
```

### Form Accessibility
```html
<label for="email">Email (required)</label>
<input type="email" id="email" name="email" required 
       aria-describedby="email-error">
<div id="email-error" role="alert" aria-live="polite">
    Please enter a valid email
</div>
```

## Interactive Elements

### Details and Dialog
```html
<!-- Expandable content -->
<details>
    <summary>Click to expand</summary>
    <p>Hidden content revealed on click</p>
</details>

<!-- Modal dialog -->
<dialog id="myDialog">
    <h2>Dialog Title</h2>
    <p>Dialog content</p>
    <button onclick="document.getElementById('myDialog').close()">
        Close
    </button>
</dialog>
```

### Progress and Meter
```html
<!-- Progress bar -->
<progress value="70" max="100">70%</progress>

<!-- Meter/gauge -->
<meter value="0.7" min="0" max="1">70%</meter>
<meter value="6" min="0" max="10" optimum="9">6 out of 10</meter>
```

## Performance Optimization

### Resource Loading
```html
<!-- Preload critical resources -->
<link rel="preload" href="critical.css" as="style">
<link rel="preload" href="hero-image.jpg" as="image">

<!-- DNS prefetch -->
<link rel="dns-prefetch" href="//fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<!-- Lazy loading -->
<img src="image.jpg" alt="Description" loading="lazy">

<!-- Script loading strategies -->
<script defer src="main.js"></script>
<script async src="analytics.js"></script>
<script type="module" src="modern.js"></script>
<script nomodule src="legacy.js"></script>
```

## Common HTML Entities

### Special Characters
```html
&lt;      <!-- < -->
&gt;      <!-- > -->
&amp;     <!-- & -->
&quot;    <!-- " -->
&apos;    <!-- ' -->
&nbsp;    <!-- non-breaking space -->
&copy;    <!-- © -->
&reg;     <!-- ® -->
&trade;   <!-- ™ -->
&mdash;   <!-- — -->
&hellip;  <!-- … -->
```

## Global Attributes

### Universal Attributes
```html
<div id="unique-id" 
     class="class1 class2"
     title="Tooltip text"
     lang="en"
     dir="ltr"
     tabindex="0"
     contenteditable="true"
     draggable="true"
     hidden
     data-custom="value">
    Content
</div>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use semantic HTML elements for meaning | Use divs for everything |
| Include alt text for all images | Leave alt attributes empty (unless decorative) |
| Use proper heading hierarchy (h1→h2→h3) | Skip heading levels or use multiple h1s |
| Always close container tags | Leave tags unclosed |
| Use lowercase for consistency | Mix uppercase and lowercase randomly |
| Include DOCTYPE declaration | Forget the DOCTYPE |
| Use HTTPS for external resources | Mix HTTP and HTTPS resources |
| Validate your HTML regularly | Ignore validation errors |

## Quick Fixes

- **Images not loading** → Check file path and alt attribute
- **Form not submitting** → Verify action URL and method attribute  
- **CSS not applying** → Check link tag href and file path
- **Page not responsive** → Add viewport meta tag
- **SEO issues** → Add title, description, and heading structure
- **Accessibility errors** → Add alt text, labels, and ARIA attributes
- **Slow loading** → Add lazy loading and optimize images
- **Validation errors** → Close all tags and check nesting

## Gotchas

⚠️ **Self-closing tags**: Some elements like `<img>`, `<br>`, `<input>` don't need closing tags
⚠️ **Block vs inline**: Block elements create new lines, inline elements don't
⚠️ **Case sensitivity**: HTML is not case-sensitive, but XHTML is
⚠️ **Nested forms**: You cannot nest `<form>` elements inside each other
⚠️ **Button behavior**: Buttons in forms default to `type="submit"`
⚠️ **Boolean attributes**: Attributes like `checked`, `disabled` don't need values
⚠️ **Encoding**: Always specify charset to avoid character display issues
⚠️ **External links**: Use `rel="noopener noreferrer"` with `target="_blank"`

## Validation Checklist

- [ ] DOCTYPE declaration included
- [ ] All required meta tags present (charset, viewport, title)
- [ ] Proper heading hierarchy (single h1, logical h2-h6)
- [ ] All images have meaningful alt attributes
- [ ] Forms have proper labels and validation
- [ ] Links have descriptive text (not "click here")
- [ ] Tables have proper headers and captions
- [ ] Page validates with W3C validator
- [ ] Content is accessible (WCAG compliant)
- [ ] Performance optimized (lazy loading, compressed images)
- [ ] SEO optimized (meta description, structured headings)
- [ ] Cross-browser tested