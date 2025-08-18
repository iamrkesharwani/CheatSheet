# HTML Tables `v5.0`

## Essentials

```html
<!-- Basic table structure -->
<table>
    <thead>
        <tr>
            <th>Product</th>
            <th>Price</th>
            <th>Stock</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Laptop</td>
            <td>$999</td>
            <td>15</td>
        </tr>
        <tr>
            <td>Mouse</td>
            <td>$25</td>
            <td>50</td>
        </tr>
    </tbody>
</table>

<!-- Table with caption and accessibility -->
<table>
    <caption>Monthly Sales Report - Q4 2024</caption>
    <thead>
        <tr>
            <th scope="col">Month</th>
            <th scope="col">Revenue</th>
            <th scope="col">Growth</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">October</th>
            <td>$45,000</td>
            <td>+8%</td>
        </tr>
    </tbody>
</table>
```

**Key Concepts**: Semantic Structure | Accessibility | Cell Spanning | Data Organization

**When to use**: Displaying tabular data, comparing information, financial reports, schedules, and data comparisons

## Syntax Reference

### Table Structure Elements

```html
<!-- Complete table with all sections -->
<table>
    <caption>Employee Performance Review</caption>
    <colgroup>
        <col>
        <col span="2">
        <col>
    </colgroup>
    <thead>
        <tr>
            <th scope="col">Employee</th>
            <th scope="colgroup" colspan="2">Scores</th>
            <th scope="col">Total</th>
        </tr>
        <tr>
            <th scope="col"></th>
            <th scope="col">Q1</th>
            <th scope="col">Q2</th>
            <th scope="col"></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">John Smith</th>
            <td>85</td>
            <td>92</td>
            <td>88.5</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="3">Average Score</td>
            <td>88.5</td>
        </tr>
    </tfoot>
</table>
```

### Cell Types and Headers

```html
<!-- Column headers -->
<tr>
    <th scope="col">Name</th>
    <th scope="col">Age</th>
    <th scope="col">City</th>
</tr>

<!-- Row headers -->
<tr>
    <th scope="row">John Doe</th>
    <td>30</td>
    <td>New York</td>
</tr>

<!-- Data cells with HTML content -->
<tr>
    <td><strong>$199.99</strong></td>
    <td><a href="/product/123">View Details</a></td>
    <td><span class="status available">In Stock</span></td>
</tr>
```

### Cell Spanning

```html
<!-- Colspan - horizontal spanning -->
<table>
    <tr>
        <th>Product</th>
        <th colspan="3">Quarterly Sales</th>
        <th>Total</th>
    </tr>
    <tr>
        <th></th>
        <th>Q1</th>
        <th>Q2</th>
        <th>Q3</th>
        <th></th>
    </tr>
    <tr>
        <td>Widget A</td>
        <td>$5,000</td>
        <td>$6,000</td>
        <td>$7,000</td>
        <td>$18,000</td>
    </tr>
</table>

<!-- Rowspan - vertical spanning -->
<table>
    <tr>
        <td rowspan="3">Engineering</td>
        <td>John Smith</td>
        <td>Senior Developer</td>
    </tr>
    <tr>
        <td>Jane Doe</td>
        <td>Frontend Developer</td>
    </tr>
    <tr>
        <td>Bob Wilson</td>
        <td>DevOps Engineer</td>
    </tr>
</table>

<!-- Complex spanning -->
<table>
    <tr>
        <th rowspan="2">Product</th>
        <th colspan="2">2023</th>
        <th colspan="2">2024</th>
    </tr>
    <tr>
        <th>Sales</th>
        <th>Profit</th>
        <th>Sales</th>
        <th>Profit</th>
    </tr>
    <tr>
        <td>Product A</td>
        <td>$10,000</td>
        <td>$2,000</td>
        <td>$12,000</td>
        <td>$2,500</td>
    </tr>
</table>
```

### Accessibility Features

```html
<!-- Scope attributes for screen readers -->
<table>
    <tr>
        <th scope="col">Student</th>
        <th scope="col">Math</th>
        <th scope="col">Science</th>
    </tr>
    <tr>
        <th scope="row">Alice Johnson</th>
        <td>95</td>
        <td>87</td>
    </tr>
</table>

<!-- Headers attribute for complex relationships -->
<table>
    <tr>
        <th id="name">Name</th>
        <th id="math">Math Score</th>
        <th id="science">Science Score</th>
    </tr>
    <tr>
        <td headers="name">John Doe</td>
        <td headers="math">95</td>
        <td headers="science">88</td>
    </tr>
</table>

<!-- Group scopes for complex tables -->
<table>
    <tr>
        <th scope="colgroup" colspan="4">2024 Quarters</th>
    </tr>
    <tr>
        <th scope="rowgroup" colspan="5">North America</th>
    </tr>
</table>
```

### Column Groups

```html
<!-- Basic column groups -->
<table>
    <colgroup>
        <col>              <!-- First column -->
        <col span="2">     <!-- Next two columns -->
        <col>              <!-- Last column -->
    </colgroup>
    <thead>
        <tr>
            <th>Product</th>
            <th>Q1</th>
            <th>Q2</th>
            <th>Total</th>
        </tr>
    </thead>
</table>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `<th>` for headers and `<td>` for data | Mix header and data cell types incorrectly |
| Include descriptive captions for context | Skip captions on complex data tables |
| Add scope attributes for accessibility | Forget screen reader navigation aids |
| Structure with thead/tbody/tfoot sections | Put all content directly in table element |
| Use tables for tabular data only | Use tables for page layout purposes |
| Test with screen readers and keyboard navigation | Assume visual users represent all users |

## Quick Fixes

- **Headers not announcing properly** → Add `scope="col"` or `scope="row"` attributes
- **Table too wide on mobile** → Implement responsive table techniques with CSS
- **Screen reader confusion** → Use `headers` attribute to link cells to multiple headers
- **Missing context** → Add `<caption>` element to describe table purpose
- **Spanning cells misaligned** → Check colspan/rowspan math adds up correctly
- **Styling inconsistencies** → Use `<colgroup>` and `<col>` for column-wide formatting

## Gotchas

⚠️ **Colspan/Rowspan Math**: Ensure spanning cells don't exceed table grid - total cells per row must match

⚠️ **Header Association**: Complex tables need `headers` attribute when scope isn't sufficient for screen readers

⚠️ **Caption Position**: `<caption>` must be first child of `<table>` element, before any other content

⚠️ **Empty Cells**: Use `&nbsp;` or CSS to handle empty cells properly for consistent rendering

⚠️ **Table Sections**: `<thead>`, `<tbody>`, `<tfoot>` must contain complete rows - can't span across sections

⚠️ **Responsive Behavior**: Tables don't automatically respond to small screens - requires CSS solutions

## Checklist

- [ ] Table has descriptive caption explaining its purpose and content
- [ ] Header cells use `<th>` with appropriate scope attributes
- [ ] Data cells use `<td>` with headers attribute when needed
- [ ] Table structure uses thead/tbody/tfoot for logical organization
- [ ] Complex relationships documented with id/headers attributes
- [ ] Colspan and rowspan calculations are mathematically correct
- [ ] Table tested with keyboard navigation and screen readers
- [ ] Responsive design considered for mobile devices
- [ ] Column groups implemented when column-wide styling needed
- [ ] Table semantics match data relationships (not used for layout)