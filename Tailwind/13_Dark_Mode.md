# Tailwind CSS Dark Mode `v3.4+`

## Essentials

```css
/* tailwind.config.js */
module.exports = {
  darkMode: 'class', // or 'media'
  // ... rest of config
}
```

```html
<!-- Basic dark mode implementation -->
<div class="bg-white dark:bg-gray-900 text-black dark:text-white">
  <h1 class="text-gray-900 dark:text-gray-100">Hello World</h1>
</div>
```

**Key Concepts**: Configuration | Class Strategy | Media Strategy | Variant Prefixes
**When to use**: Creating responsive themes that adapt to user preferences or system settings

## Syntax Reference

### Basic Configuration

```javascript
// tailwind.config.js - Class-based (recommended)
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        dark: {
          bg: '#1a1a1a',
          text: '#ffffff'
        }
      }
    }
  }
}

// Media query strategy (automatic)
module.exports = {
  darkMode: 'media', // Respects prefers-color-scheme
}
```

### Common Patterns

```html
<!-- Pattern 1: Basic background and text -->
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  Content here
</div>

<!-- Pattern 2: Borders and shadows -->
<div class="border border-gray-200 dark:border-gray-700 shadow-md dark:shadow-xl">
  Card content
</div>

<!-- Pattern 3: Interactive elements -->
<button class="bg-blue-500 hover:bg-blue-600 dark:bg-blue-600 dark:hover:bg-blue-700 text-white">
  Click me
</button>

<!-- Pattern 4: Complex component -->
<nav class="bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700">
  <a class="text-gray-700 hover:text-gray-900 dark:text-gray-300 dark:hover:text-white">
    Navigation Link
  </a>
</nav>
```

## JavaScript Toggle Implementation

```javascript
// Simple toggle function
function toggleDarkMode() {
  document.documentElement.classList.toggle('dark');
  localStorage.setItem('darkMode', 
    document.documentElement.classList.contains('dark')
  );
}

// Initialize on page load
if (localStorage.getItem('darkMode') === 'true' || 
    (!localStorage.getItem('darkMode') && 
     window.matchMedia('(prefers-color-scheme: dark)').matches)) {
  document.documentElement.classList.add('dark');
}

// React implementation
const [darkMode, setDarkMode] = useState(false);

useEffect(() => {
  if (darkMode) {
    document.documentElement.classList.add('dark');
  } else {
    document.documentElement.classList.remove('dark');
  }
}, [darkMode]);
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use consistent color schemes across components | Mix different gray scales randomly |
| Test both light and dark modes thoroughly | Assume dark variants work without testing |
| Provide manual toggle for class-based strategy | Force dark mode without user control |
| Use semantic color names in config | Hardcode colors in HTML classes |
| Consider accessibility and contrast ratios | Ignore color contrast requirements |

## Quick Fixes

- **Dark mode not working** → Check `darkMode: 'class'` in config and `.dark` class on html/body
- **Colors look wrong** → Verify you're using `dark:` prefix consistently
- **Toggle not persisting** → Implement localStorage to save user preference
- **Media query conflicts** → Switch from `media` to `class` strategy for manual control
- **Build not including dark variants** → Ensure Tailwind processes your dark: classes

## Gotchas

⚠️ **Class Strategy Requires Manual Implementation**: Unlike `media` strategy, `class` needs JavaScript to add/remove the `dark` class

⚠️ **Specificity Issues**: Dark variants have same specificity as regular classes - order matters in your HTML

⚠️ **Inheritance Problems**: Child elements don't automatically inherit dark variants from parents

⚠️ **Build Optimization**: Unused dark variants might be purged if not detected in your code

## Advanced Patterns

```html
<!-- Group hover with dark mode -->
<div class="group hover:bg-gray-100 dark:hover:bg-gray-800">
  <p class="group-hover:text-blue-600 dark:group-hover:text-blue-400">Hover effect</p>
</div>

<!-- Responsive + dark mode -->
<div class="bg-white md:bg-gray-100 dark:bg-gray-900 dark:md:bg-gray-800">
  Responsive dark mode
</div>

<!-- Custom dark mode colors -->
<div class="bg-slate-50 dark:bg-slate-900 text-slate-900 dark:text-slate-100">
  Better contrast
</div>
```

## Theme Extension Example

```javascript
// tailwind.config.js
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a',
        }
      }
    }
  }
}
```

```html
<!-- Usage with custom colors -->
<div class="bg-primary-50 dark:bg-primary-900 text-primary-900 dark:text-primary-50">
  Custom themed content
</div>
```

## Checklist

- [ ] Configure `darkMode` strategy in tailwind.config.js
- [ ] Add `dark:` variants to all background and text colors
- [ ] Implement JavaScript toggle for class-based strategy
- [ ] Store user preference in localStorage
- [ ] Test color contrast in both modes
- [ ] Handle system preference detection
- [ ] Add dark variants to borders, shadows, and interactive states
- [ ] Verify all components work in both modes