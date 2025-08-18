# CSS Print & Screen `v 0.07.25.01`

## Essentials

```css
/* Basic print styles */
@media print {
  body { 
    font-size: 12pt;
    color: black;
    background: white;
  }
  
  /* Hide non-essential elements */
  nav, .sidebar, .ads, button {
    display: none !important;
  }
  
  /* Control page breaks */
  h1, h2 { page-break-after: avoid; }
  .content { page-break-inside: avoid; }
}
```

**Key Concepts**: Print Optimization | Page Break Control | Element Visibility | Typography Units | URL Display
**When to use**: Documents, articles, reports, invoices, or any content users might print

## Syntax Reference

### Media Queries for Print

```css
/* Print-specific styles */
@media print {
  /* Typography optimization */
  body {
    font-family: "Times New Roman", serif;
    font-size: 12pt;
    line-height: 1.4;
    color: #000;
    background: #fff;
  }
  
  /* Remove unnecessary spacing */
  * {
    box-shadow: none !important;
    text-shadow: none !important;
  }
  
  /* Ensure black text on white background */
  h1, h2, h3, h4, h5, h6 {
    color: #000;
    background: transparent;
  }
}

/* Screen-only styles */
@media screen {
  .print-only {
    display: none;
  }
}

/* Combined conditions */
@media screen and (min-width: 768px) {
  /* Desktop screen styles */
}

@media print and (orientation: landscape) {
  /* Landscape print styles */
}
```

### Page Break Controls

```css
/* Page break properties */
.chapter {
  page-break-before: always;  /* always, auto, avoid, left, right */
  page-break-after: avoid;    /* avoid starting new page after */
  page-break-inside: avoid;   /* prevent breaking element across pages */
}

/* Modern alternatives (better browser support) */
.section {
  break-before: page;         /* auto, avoid, page, left, right */
  break-after: avoid;         /* avoid page break after element */
  break-inside: avoid;        /* keep element together */
}

/* Orphans and widows control */
p {
  orphans: 3;                 /* min lines at bottom of page */
  widows: 3;                  /* min lines at top of page */
}

/* Page margins and size */
@page {
  margin: 1in;
  size: A4;                   /* A4, letter, legal, landscape */
}

@page :first {
  margin-top: 2in;            /* Special first page margins */
}
```

### Element Visibility Control

```css
/* Hide elements in print */
@media print {
  /* Navigation and UI elements */
  nav, .navigation,
  .sidebar, .ads,
  .social-share,
  button:not(.print-button),
  .back-to-top {
    display: none !important;
  }
  
  /* Form elements (usually not needed in print) */
  input[type="search"],
  input[type="email"],
  textarea,
  select {
    display: none;
  }
  
  /* Interactive elements */
  .tooltip, .dropdown,
  .modal, .popup {
    display: none !important;
  }
}

/* Show elements only in print */
.print-only {
  display: none;
}

@media print {
  .print-only {
    display: block;
  }
  
  .screen-only {
    display: none !important;
  }
}

/* Print version of hidden content */
@media print {
  .content-summary {
    display: block;
  }
  
  .content-full {
    display: none;
  }
}
```

## Common Patterns

```css
/* Pattern 1: Complete print stylesheet */
@media print {
  /* Reset and base styles */
  * {
    background: transparent !important;
    color: #000 !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  
  /* Typography */
  body {
    font-family: "Times New Roman", serif;
    font-size: 12pt;
    line-height: 1.4;
    margin: 0;
  }
  
  /* Headings */
  h1, h2, h3, h4, h5, h6 {
    color: #000;
    page-break-after: avoid;
    break-after: avoid;
  }
  
  /* Links */
  a, a:visited {
    text-decoration: underline;
    color: #000;
  }
  
  /* Show URLs after links */
  a[href]:after {
    content: " (" attr(href) ")";
    font-size: 10pt;
    color: #666;
  }
  
  /* Don't show URLs for internal links */
  a[href^="#"]:after,
  a[href^="javascript:"]:after {
    content: "";
  }
  
  /* Images */
  img {
    max-width: 100% !important;
    height: auto !important;
    page-break-inside: avoid;
  }
  
  /* Tables */
  table {
    border-collapse: collapse;
    width: 100%;
  }
  
  th, td {
    border: 1px solid #000;
    padding: 4pt 8pt;
  }
  
  thead {
    display: table-header-group;
  }
  
  tr {
    page-break-inside: avoid;
  }
}

/* Pattern 2: Article/blog post print optimization */
@media print {
  /* Header/footer for printed pages */
  @page {
    margin: 1in;
    @top-center {
      content: attr(title);
    }
    @bottom-right {
      content: "Page " counter(page);
    }
  }
  
  /* Article structure */
  .article-header {
    border-bottom: 2pt solid #000;
    padding-bottom: 12pt;
    margin-bottom: 24pt;
  }
  
  .article-title {
    font-size: 18pt;
    font-weight: bold;
    margin-bottom: 6pt;
  }
  
  .article-meta {
    font-size: 10pt;
    color: #666;
  }
  
  .article-content p {
    margin-bottom: 12pt;
    text-align: justify;
  }
  
  /* Code blocks */
  pre, code {
    background: #f5f5f5 !important;
    border: 1pt solid #ccc;
    font-family: "Courier New", monospace;
    font-size: 9pt;
    page-break-inside: avoid;
  }
  
  /* Blockquotes */
  blockquote {
    border-left: 3pt solid #000;
    padding-left: 12pt;
    margin: 12pt 0;
    font-style: italic;
    page-break-inside: avoid;
  }
}

/* Pattern 3: Invoice/document print layout */
@media print {
  .invoice {
    font-family: Arial, sans-serif;
    font-size: 11pt;
  }
  
  .invoice-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 24pt;
    padding-bottom: 12pt;
    border-bottom: 1pt solid #000;
  }
  
  .invoice-table {
    width: 100%;
    border-collapse: collapse;
    margin: 12pt 0;
  }
  
  .invoice-table th {
    background: #f0f0f0 !important;
    border: 1pt solid #000;
    padding: 6pt;
    text-align: left;
  }
  
  .invoice-table td {
    border: 1pt solid #000;
    padding: 6pt;
  }
  
  .invoice-total {
    text-align: right;
    margin-top: 12pt;
    font-weight: bold;
    font-size: 12pt;
  }
  
  /* Force page break before footer */
  .invoice-footer {
    break-before: page;
    margin-top: 24pt;
    font-size: 9pt;
    color: #666;
  }
}

/* Pattern 4: Resume/CV print styles */
@media print {
  .resume {
    font-family: "Times New Roman", serif;
    font-size: 11pt;
    line-height: 1.3;
  }
  
  .resume-section {
    margin-bottom: 18pt;
    break-inside: avoid;
  }
  
  .resume-section h2 {
    font-size: 14pt;
    font-weight: bold;
    border-bottom: 1pt solid #000;
    padding-bottom: 3pt;
    margin-bottom: 9pt;
  }
  
  .job-entry {
    margin-bottom: 12pt;
    break-inside: avoid;
  }
  
  .job-title {
    font-weight: bold;
    font-size: 12pt;
  }
  
  .job-company {
    font-style: italic;
    margin-bottom: 3pt;
  }
  
  .job-dates {
    font-size: 10pt;
    color: #666;
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use point (pt) units for print typography | Use pixels or viewport units in print styles |
| Hide non-essential UI elements (nav, ads, buttons) | Print decorative backgrounds that waste ink |
| Control page breaks for better readability | Let important content split awkwardly across pages |
| Show URLs after links for reference | Show URLs for internal or JavaScript links |
| Use high contrast (black text on white) | Rely on color alone to convey information |

## Quick Fixes

- **Text too small in print** → Use pt units instead of px (12pt = readable size)
- **Unwanted page breaks** → Add `page-break-inside: avoid` or `break-inside: avoid`
- **Hidden content in print** → Check for `display: none` in print media queries
- **Wasted ink on backgrounds** → Set `background: transparent !important` in print styles
- **Links without context** → Use `a[href]:after { content: " (" attr(href) ")"; }`

## Gotchas

⚠️ **Browser Differences**: Print rendering varies between browsers - test in multiple browsers
⚠️ **Page Break Support**: Older browsers may not support `break-*` properties - use `page-break-*` fallbacks
⚠️ **Font Availability**: Web fonts may not print correctly - provide serif/sans-serif fallbacks
⚠️ **Color Printing**: Not all users have color printers - ensure content works in black and white
⚠️ **Print Preview**: Always test with browser print preview - actual printing may differ

## Checklist

- [ ] Test print preview in Chrome, Firefox, and Safari
- [ ] Use point (pt) units for all print typography
- [ ] Hide non-essential elements (navigation, ads, buttons)
- [ ] Control page breaks for headings and content blocks
- [ ] Show URLs after external links for reference
- [ ] Ensure sufficient contrast (black text on white background)
- [ ] Test content works without color (grayscale printing)
- [ ] Set appropriate page margins and orientation
- [ ] Verify tables and images scale properly
- [ ] Include print-only elements like page numbers or dates