# DOM Manipulation `v 0.07.25.01`

## Essentials
```javascript
// Select elements
const element = document.getElementById('myId');
const elements = document.querySelectorAll('.my-class');

// Change content
element.textContent = 'New text';
element.innerHTML = '<strong>Bold text</strong>';

// Modify styles
element.style.color = 'red';
element.classList.add('active');

// Create and append
const newDiv = document.createElement('div');
newDiv.textContent = 'Hello World';
document.body.appendChild(newDiv);
```

**Key Concepts**: Element Selection | Content Manipulation | Event Handling | Style Management | DOM Traversal

**When to use**: Dynamic web interfaces, form handling, interactive user experiences, real-time content updates

## Syntax Reference

### Element Selection
```javascript
// By ID (returns single element or null)
const element = document.getElementById('myId');

// CSS selectors (first match)
const first = document.querySelector('.class-name');
const button = document.querySelector('#submit-btn');

// CSS selectors (all matches - returns NodeList)
const all = document.querySelectorAll('button');
const inputs = document.querySelectorAll('input[type="text"]');

// Legacy methods (returns HTMLCollection)
const byClass = document.getElementsByClassName('my-class');
const byTag = document.getElementsByTagName('div');
```

### Content Manipulation
```javascript
// Text content (safe from XSS)
element.textContent = 'Plain text';
console.log(element.textContent);

// HTML content (can execute scripts)
element.innerHTML = '<strong>Bold</strong>';

// Form values
input.value = 'new value';
select.value = 'option2';
checkbox.checked = true;
```

### Style Management
```javascript
// Inline styles
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.fontSize = '16px';

// CSS classes (recommended)
element.classList.add('active');
element.classList.remove('hidden');
element.classList.toggle('highlight');
element.classList.contains('active'); // returns boolean

// Get computed styles
const styles = getComputedStyle(element);
console.log(styles.color);
```

### Creating Elements
```javascript
// Create and configure
const div = document.createElement('div');
div.textContent = 'Hello';
div.className = 'my-class';
div.id = 'unique-id';

// Append to DOM
parent.appendChild(div);
parent.insertBefore(div, existingChild);

// Modern methods
parent.append(div); // Can append multiple
parent.prepend(div); // Add at beginning
element.remove(); // Remove element
```

### Event Handling
```javascript
// Add event listener
button.addEventListener('click', function(event) {
  console.log('Clicked!');
  event.preventDefault(); // Stop default action
});

// Remove event listener (same function reference needed)
button.removeEventListener('click', handleClick);

// Event delegation
container.addEventListener('click', function(event) {
  if (event.target.matches('.delete-btn')) {
    // Handle delete button clicks
  }
});
```

### DOM Traversal
```javascript
// Parent navigation
const parent = element.parentElement;
const form = element.closest('form'); // Find ancestor

// Child navigation
const children = parent.children; // HTMLCollection
const firstChild = parent.firstElementChild;
const lastChild = parent.lastElementChild;

// Sibling navigation
const next = element.nextElementSibling;
const prev = element.previousElementSibling;
```

### Attributes & Data
```javascript
// Attributes
element.getAttribute('data-id');
element.setAttribute('data-id', '123');
element.removeAttribute('disabled');
element.hasAttribute('required');

// Data attributes
element.dataset.userId = '123'; // Creates data-user-id
console.log(element.dataset.userId);

// Properties vs Attributes
element.id = 'newId'; // Property
element.setAttribute('id', 'newId'); // Attribute
```

## Common Patterns

```javascript
// Pattern 1: Batch DOM updates with DocumentFragment
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
  const div = document.createElement('div');
  div.textContent = `Item ${i}`;
  fragment.appendChild(div);
}
container.appendChild(fragment); // Single reflow

// Pattern 2: Safe content insertion
function safeInsert(element, userInput) {
  element.textContent = userInput; // Safe
  // element.innerHTML = userInput; // Dangerous!
}

// Pattern 3: Form data collection
function getFormData(form) {
  const formData = new FormData(form);
  const data = {};
  for (const [key, value] of formData) {
    data[key] = value;
  }
  return data;
}

// Pattern 4: Element creation utility
function createElement(tag, props = {}, children = []) {
  const element = document.createElement(tag);
  
  Object.keys(props).forEach(key => {
    if (key === 'textContent') {
      element.textContent = props[key];
    } else if (key === 'className') {
      element.className = props[key];
    } else {
      element.setAttribute(key, props[key]);
    }
  });
  
  children.forEach(child => {
    element.appendChild(child);
  });
  
  return element;
}

// Pattern 5: Event delegation for dynamic content
function setupEventDelegation(container) {
  container.addEventListener('click', function(event) {
    if (event.target.matches('.delete-btn')) {
      event.target.closest('.item').remove();
    } else if (event.target.matches('.edit-btn')) {
      editItem(event.target.closest('.item'));
    }
  });
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `textContent` for user input | Use `innerHTML` for untrusted content |
| Use `classList` for class manipulation | Manipulate `className` string directly |
| Use event delegation for dynamic content | Add listeners to every dynamic element |
| Batch DOM updates with DocumentFragment | Make individual DOM changes in loops |
| Cache element references | Query DOM repeatedly in loops |
| Use semantic HTML with ARIA attributes | Rely only on visual styling for meaning |
| Remove event listeners when cleaning up | Leave orphaned event listeners |
| Use `closest()` for ancestor traversal | Walk up the DOM tree manually |

## Quick Fixes

- **Element not found** → Check if DOM is loaded, use `DOMContentLoaded` event
- **Event listener not working** → Ensure element exists when adding listener
- **CSS changes not applying** → Use `!important` sparingly, check specificity
- **Memory leaks with events** → Remove event listeners when elements are removed
- **Slow DOM updates** → Use DocumentFragment for batch operations
- **XSS vulnerabilities** → Use `textContent` instead of `innerHTML` for user data
- **Form not submitting** → Check for `preventDefault()` in event handlers
- **Styles not updating** → Use `getComputedStyle()` to read actual values

## Gotchas

⚠️ **NodeList vs Array**: `querySelectorAll()` returns NodeList, not Array. Use `Array.from()` or spread operator for array methods

⚠️ **Live vs Static Collections**: `getElementsByClassName()` returns live HTMLCollection that updates automatically, `querySelectorAll()` returns static NodeList

⚠️ **Event Listener Memory**: Event listeners on removed elements can cause memory leaks. Always remove listeners or use weak references

⚠️ **innerHTML Security**: Using `innerHTML` with user input can lead to XSS attacks. Always sanitize or use `textContent`

⚠️ **Style Property Names**: CSS properties become camelCase in JavaScript (`background-color` → `backgroundColor`)

⚠️ **Form Element Values**: Reading `value` before user interaction may return empty string even if placeholder exists

## Checklist

- [ ] Elements exist before manipulation (check with `if` statements)
- [ ] Event listeners removed when elements are destroyed
- [ ] User input sanitized before insertion into DOM
- [ ] Batch DOM operations for performance (use DocumentFragment)
- [ ] CSS classes used instead of inline styles when possible
- [ ] Form validation implemented before submission
- [ ] Accessibility attributes (ARIA) included for dynamic content
- [ ] Error handling for DOM operations that might fail
- [ ] Memory leaks prevented by proper cleanup
- [ ] Event delegation used for dynamic content