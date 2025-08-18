# JavaScript Events `v 0.07.25.01`

## Essentials
```javascript
// Add event listener
button.addEventListener('click', function(event) {
  console.log('Button clicked!');
  event.preventDefault(); // Stop default behavior
});

// Event delegation (handle dynamic content)
container.addEventListener('click', function(event) {
  if (event.target.matches('.delete-btn')) {
    event.target.closest('.item').remove();
  }
});

// Remove event listener
button.removeEventListener('click', handleClick);

// Custom events
const customEvent = new CustomEvent('userAction', {
  detail: { userId: 123, action: 'login' }
});
element.dispatchEvent(customEvent);
```

**Key Concepts**: Event Listeners | Event Object | Bubbling/Capturing | Event Delegation | Custom Events

**When to use**: User interactions, form handling, dynamic content updates, component communication

## Syntax Reference

### Event Listener Basics
```javascript
// Basic syntax
element.addEventListener(event, handler, options);

// Function reference (for removal)
function handleClick(event) {
  console.log('Clicked!');
}
button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick);

// Event options
element.addEventListener('click', handler, {
  once: true,      // Run only once
  passive: true,   // Never calls preventDefault()
  capture: true    // Capture phase
});

// Legacy boolean for capture
element.addEventListener('click', handler, true);
```

### Common Event Types
```javascript
// Mouse events
element.addEventListener('click', handleClick);
element.addEventListener('dblclick', handleDoubleClick);
element.addEventListener('mousedown', handleMouseDown);
element.addEventListener('mouseup', handleMouseUp);
element.addEventListener('mouseover', handleMouseOver);
element.addEventListener('mouseout', handleMouseOut);
element.addEventListener('mouseenter', handleMouseEnter);
element.addEventListener('mouseleave', handleMouseLeave);

// Keyboard events
input.addEventListener('keydown', handleKeyDown);
input.addEventListener('keyup', handleKeyUp);
input.addEventListener('keypress', handleKeyPress); // Deprecated

// Form events
form.addEventListener('submit', handleSubmit);
input.addEventListener('input', handleInput);      // Real-time changes
input.addEventListener('change', handleChange);    // After focus lost
input.addEventListener('focus', handleFocus);
input.addEventListener('blur', handleBlur);

// Window events
window.addEventListener('load', handleLoad);
window.addEventListener('resize', handleResize);
window.addEventListener('scroll', handleScroll);
```

### Event Object Properties
```javascript
function handleEvent(event) {
  // Target information
  console.log('Target:', event.target);           // Element that triggered event
  console.log('Current target:', event.currentTarget); // Element with listener
  console.log('Type:', event.type);               // Event type ('click', 'keydown', etc.)
  
  // Mouse properties
  console.log('Mouse X/Y:', event.clientX, event.clientY);
  console.log('Page X/Y:', event.pageX, event.pageY);
  console.log('Screen X/Y:', event.screenX, event.screenY);
  
  // Keyboard properties
  console.log('Key:', event.key);                 // 'Enter', 'a', 'ArrowUp'
  console.log('Key code:', event.code);           // 'KeyA', 'Enter', 'ArrowUp'
  console.log('Modifiers:', event.ctrlKey, event.shiftKey, event.altKey);
  
  // Event control
  event.preventDefault();                         // Stop default action
  event.stopPropagation();                       // Stop bubbling
  event.stopImmediatePropagation();              // Stop other listeners
}
```

### Event Bubbling & Capturing
```javascript
// Event phases: Capture → Target → Bubble
const outer = document.getElementById('outer');
const inner = document.getElementById('inner');

// Bubbling (default)
outer.addEventListener('click', () => console.log('Outer bubbling'));
inner.addEventListener('click', () => console.log('Inner bubbling'));

// Capturing
outer.addEventListener('click', () => console.log('Outer capturing'), true);
inner.addEventListener('click', () => console.log('Inner capturing'), true);

// Click inner element output:
// 1. Outer capturing
// 2. Inner capturing  
// 3. Inner bubbling
// 4. Outer bubbling

// Stop propagation
inner.addEventListener('click', function(event) {
  event.stopPropagation(); // Outer handlers won't run
});
```

## Common Patterns

```javascript
// Pattern 1: Event delegation for dynamic content
function setupEventDelegation(container) {
  container.addEventListener('click', function(event) {
    const target = event.target;
    
    if (target.matches('.delete-btn')) {
      target.closest('.item').remove();
    } else if (target.matches('.edit-btn')) {
      editItem(target.closest('.item'));
    } else if (target.matches('.toggle-btn')) {
      target.classList.toggle('active');
    }
  });
}

// Pattern 2: Debouncing for performance
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

const debouncedSearch = debounce(function(event) {
  performSearch(event.target.value);
}, 300);

searchInput.addEventListener('input', debouncedSearch);

// Pattern 3: Throttling for scroll/resize
function throttle(func, delay) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, delay);
    }
  };
}

const throttledScroll = throttle(function() {
  updateScrollPosition();
}, 100);

window.addEventListener('scroll', throttledScroll);

// Pattern 4: Form validation with events
function setupFormValidation(form) {
  // Real-time validation
  form.addEventListener('input', function(event) {
    if (event.target.matches('input[required]')) {
      validateField(event.target);
    }
  });
  
  // Submit validation
  form.addEventListener('submit', function(event) {
    if (!validateForm(form)) {
      event.preventDefault();
      showErrors();
    }
  });
}

// Pattern 5: Keyboard shortcuts
document.addEventListener('keydown', function(event) {
  // Ctrl+S to save
  if (event.ctrlKey && event.key === 's') {
    event.preventDefault();
    saveDocument();
  }
  
  // Escape to close modal
  if (event.key === 'Escape') {
    closeModal();
  }
  
  // Enter to submit form
  if (event.key === 'Enter' && event.target.matches('input')) {
    event.target.form.submit();
  }
});

// Pattern 6: Custom events for component communication
class Component {
  notify(eventName, data) {
    const event = new CustomEvent(eventName, {
      detail: data,
      bubbles: true,
      cancelable: true
    });
    this.element.dispatchEvent(event);
  }
  
  listen(eventName, handler) {
    this.element.addEventListener(eventName, handler);
  }
}

// Usage
const component = new Component();
component.listen('dataChanged', function(event) {
  console.log('Data:', event.detail);
});
component.notify('dataChanged', { id: 1, value: 'new' });
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use event delegation for dynamic content | Add listeners to every dynamic element |
| Remove event listeners when cleaning up | Leave orphaned event listeners |
| Use named functions for removable listeners | Use anonymous functions you need to remove |
| Debounce expensive operations | Run heavy code on every input event |
| Use `passive: true` for scroll/touch events | Block default behavior unnecessarily |
| Check `event.target` in delegated handlers | Assume event target is the listener element |
| Use `preventDefault()` for form validation | Allow invalid forms to submit |
| Handle keyboard events for accessibility | Rely only on mouse interactions |

## Quick Fixes

- **Event listener not working** → Check if element exists when adding listener
- **Can't remove event listener** → Use same function reference, not anonymous function
- **Memory leaks** → Remove listeners when elements are destroyed
- **Form submits despite validation** → Use `preventDefault()` in submit handler
- **Scroll performance issues** → Throttle scroll handlers with `passive: true`
- **Click events not working on dynamic content** → Use event delegation
- **Keyboard shortcuts not working** → Check for `preventDefault()` and modifier keys
- **Custom events not bubbling** → Set `bubbles: true` in CustomEvent options

## Gotchas

⚠️ **Anonymous Functions**: Can't be removed with `removeEventListener()`. Always use named functions or store references

⚠️ **Event Target vs Current Target**: `event.target` is the clicked element, `event.currentTarget` is the element with the listener

⚠️ **Passive Listeners**: When `passive: true` is set, `preventDefault()` is ignored and may show console warnings

⚠️ **Form Submit Events**: Default form submission will reload the page unless prevented with `preventDefault()`

⚠️ **Key Events**: `keypress` is deprecated. Use `keydown` for most cases, `keyup` for after-action handling

⚠️ **Memory Leaks**: Event listeners on removed DOM elements prevent garbage collection

⚠️ **This Binding**: Arrow functions don't bind `this` to the element. Use regular functions when you need element context

## Checklist

- [ ] Event listeners removed when elements are destroyed
- [ ] Named functions used for listeners that need removal
- [ ] Event delegation implemented for dynamic content
- [ ] Performance optimized with debouncing/throttling
- [ ] Form validation prevents submission of invalid data
- [ ] Keyboard accessibility implemented alongside mouse events
- [ ] Custom events use appropriate bubbling and cancelable options
- [ ] Passive listeners used for scroll/touch events when appropriate
- [ ] Memory usage monitored for event listener leaks