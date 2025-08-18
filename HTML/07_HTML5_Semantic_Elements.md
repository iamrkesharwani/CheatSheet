# HTML5 Semantic Elements `v 5.0`

## Essentials
```html
<header>Site/section header with nav</header>
<nav>Navigation links</nav>
<main>Primary content (one per page)</main>
<article>Independent, reusable content</article>
<section>Thematic content grouping</section>
<aside>Sidebar/tangential content</aside>
<footer>Site/section footer</footer>
```

**Key Concepts**: Meaning over appearance | Document structure | Accessibility | SEO optimization
**When to use**: When content has semantic meaning beyond visual styling

## Syntax Reference

### Page Structure Elements
```html
<!-- Complete semantic page structure -->
<header>
  <h1>Site Title</h1>
  <nav>
    <ul>
      <li><a href="/" aria-current="page">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>

<main>
  <h1>Page Title</h1>
  <!-- Only one main per page -->
  <section>
    <h2>Content Section</h2>
    <p>Main content goes here...</p>
  </section>
</main>

<aside>
  <h2>Sidebar</h2>
  <p>Related or supplementary content</p>
</aside>

<footer>
  <p>&copy; 2024 Company Name</p>
</footer>
```

### Content Organization
```html
<!-- Article: Standalone, syndicate-able content -->
<article>
  <header>
    <h2>Article Title</h2>
    <time datetime="2024-01-15">January 15, 2024</time>
    <address>By John Doe</address>
  </header>
  
  <section>
    <h3>Introduction</h3>
    <p>Article content...</p>
  </section>
  
  <footer>
    <p>Tags: HTML5, Semantics</p>
  </footer>
</article>

<!-- Section: Thematic grouping with heading -->
<section>
  <h2>Our Services</h2>
  <section>
    <h3>Web Development</h3>
    <p>Service details...</p>
  </section>
</section>
```

### Interactive & Media Elements
```html
<!-- Details/Summary: Collapsible content -->
<details>
  <summary>Click to expand FAQ</summary>
  <p>Hidden content revealed on click</p>
</details>

<!-- Dialog: Modal/popup content -->
<dialog id="modal">
  <h3>Modal Title</h3>
  <p>Modal content</p>
  <button onclick="this.closest('dialog').close()">Close</button>
</dialog>

<!-- Time: Machine-readable dates -->
<time datetime="2024-01-15T10:30:00">January 15, 2024 at 10:30 AM</time>
<time datetime="PT2H30M">2 hours 30 minutes</time>
```

### Navigation Patterns
```html
<!-- Primary navigation -->
<nav>
  <ul>
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/products">Products</a></li>
  </ul>
</nav>

<!-- Breadcrumb navigation -->
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/category">Category</a></li>
    <li aria-current="page">Current Page</li>
  </ol>
</nav>
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use `<article>` for standalone content (blog posts, news articles) | Use `<article>` just for styling containers |
| Use `<section>` when content has a natural heading | Use `<section>` as a generic wrapper (use `<div>`) |
| Use only one `<main>` per page | Use multiple `<main>` elements on same page |
| Use `<aside>` for tangentially related content | Use `<aside>` for primary navigation |
| Include `datetime` attribute in `<time>` elements | Use `<time>` for non-temporal content |
| Use `<address>` for contact info of article/page author | Use `<address>` for postal addresses in content |

## Quick Fixes
- **Screen reader confusion** → Add `aria-label` to multiple `<nav>` elements
- **SEO not recognizing content structure** → Replace `<div>` with appropriate semantic elements
- **Time not machine-readable** → Add `datetime="YYYY-MM-DD"` to `<time>` elements
- **Dialog not working** → Use `showModal()` method and `method="dialog"` on forms
- **Multiple main elements** → Use only one `<main>` per page, nest sections inside

## Gotchas
⚠️ **Article vs Section**: Use `<article>` for content that makes sense independently, `<section>` for thematic groupings within a page

⚠️ **Header/Footer scope**: These elements are scoped to their nearest sectioning element (article, aside, nav, section) or body

⚠️ **Main element placement**: `<main>` should contain the dominant content, exclude site navigation, headers, footers, and sidebars

⚠️ **Time element validation**: `datetime` must use valid ISO 8601 format (YYYY-MM-DD, YYYY-MM-DDTHH:MM:SS)

⚠️ **Details element styling**: The triangle marker can be hidden with CSS `summary { list-style: none; }`

## Checklist
- [ ] Used semantic elements based on meaning, not appearance
- [ ] Only one `<main>` element per page
- [ ] All `<section>` elements have logical headings
- [ ] Navigation elements have appropriate `aria-label` when multiple exist
- [ ] Time elements include machine-readable `datetime` attributes
- [ ] Address elements contain author/contact info, not postal addresses
- [ ] Article elements contain complete, standalone content
- [ ] Page has logical heading hierarchy (h1 → h2 → h3)
- [ ] Interactive elements (details, dialog) have proper event handling