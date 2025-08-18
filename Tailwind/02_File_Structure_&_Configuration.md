# Tailwind CSS File Structure & Configuration `v3.4`

## Essentials

```javascript
// tailwind.config.js - Core structure
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js,jsx,ts,tsx}"],
  darkMode: 'class', // 'media' or 'class'
  theme: {
    extend: {
      colors: {
        primary: '#3b82f6'
      }
    },
  },
  plugins: [],
}
```

**Key Concepts**: Theme Extension | Content Purging | Layer System
**When to use**: Custom design systems, brand colors, responsive breakpoints, dark mode

## Syntax Reference

### Basic Configuration Structure

```javascript
// Complete tailwind.config.js structure
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{html,js,jsx,ts,tsx,vue}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  darkMode: 'class', // false, 'media', or 'class'
  theme: {
    // Override defaults completely
    screens: {
      sm: '480px',
      md: '768px',
      lg: '976px',
      xl: '1440px',
    },
    // Extend existing defaults
    extend: {
      colors: {
        brand: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a',
        }
      },
      fontFamily: {
        sans: ['Inter', 'system-ui'],
        serif: ['Georgia', 'serif'],
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
      }
    },
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ],
}
```

### Theme Customization Patterns

```javascript
// Color system with semantic naming
theme: {
  extend: {
    colors: {
      primary: {
        50: '#eff6ff',
        100: '#dbeafe',
        500: '#3b82f6',
        600: '#2563eb',
        900: '#1e3a8a',
      },
      gray: {
        950: '#030712', // Custom dark shade
      }
    }
  }
}

// Typography scale
theme: {
  extend: {
    fontSize: {
      'xs': ['0.75rem', { lineHeight: '1rem' }],
      '5xl': ['3rem', { lineHeight: '3.5rem', letterSpacing: '-0.02em' }],
    },
    fontFamily: {
      display: ['Inter', 'system-ui'],
      body: ['Inter', 'system-ui'],
    }
  }
}

// Responsive breakpoints
theme: {
  screens: {
    'xs': '475px',
    'sm': '640px',
    'md': '768px',
    'lg': '1024px',
    'xl': '1280px',
    '2xl': '1536px',
    '3xl': '1920px',
  }
}
```

### Custom CSS with @layer

```css
/* src/styles/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  /* Global styles */
  html {
    font-family: Inter, system-ui, sans-serif;
  }
  
  h1, h2, h3, h4, h5, h6 {
    @apply font-semibold text-gray-900 dark:text-gray-100;
  }
}

@layer components {
  /* Reusable component styles */
  .btn {
    @apply inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-offset-2;
  }
  
  .btn-primary {
    @apply btn bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500;
  }
  
  .card {
    @apply bg-white dark:bg-gray-800 shadow-lg rounded-lg p-6 border border-gray-200 dark:border-gray-700;
  }
}

@layer utilities {
  /* Custom utility classes */
  .text-balance {
    text-wrap: balance;
  }
  
  .scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }
  .scrollbar-hide::-webkit-scrollbar {
    display: none;
  }
}
```

## Common Patterns

```javascript
// Pattern 1: Brand color system
theme: {
  extend: {
    colors: {
      brand: {
        50: '#f0f9ff',
        100: '#e0f2fe',
        200: '#bae6fd',
        300: '#7dd3fc',
        400: '#38bdf8',
        500: '#0ea5e9', // Primary brand color
        600: '#0284c7',
        700: '#0369a1',
        800: '#075985',
        900: '#0c4a6e',
        950: '#082f49',
      }
    }
  }
}

// Pattern 2: Container customization
theme: {
  container: {
    center: true,
    padding: {
      DEFAULT: '1rem',
      sm: '2rem',
      lg: '4rem',
      xl: '5rem',
      '2xl': '6rem',
    },
  },
}

// Pattern 3: Dark mode setup
darkMode: 'class', // Enable class-based dark mode
theme: {
  extend: {
    colors: {
      gray: {
        50: '#f9fafb',
        900: '#111827',
        950: '#030712', // Extra dark for dark mode
      }
    }
  }
}

// Pattern 4: Animation extensions
theme: {
  extend: {
    animation: {
      'fade-in': 'fadeIn 0.5s ease-in-out',
      'slide-up': 'slideUp 0.3s ease-out',
    },
    keyframes: {
      fadeIn: {
        '0%': { opacity: '0' },
        '100%': { opacity: '1' },
      },
      slideUp: {
        '0%': { transform: 'translateY(20px)', opacity: '0' },
        '100%': { transform: 'translateY(0)', opacity: '1' },
      }
    }
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use `extend` to add to existing theme | Override entire theme unless necessary |
| Include all file paths in `content` array | Forget to update content paths for new directories |
| Use semantic color names (primary, secondary) | Use generic names (blue, red) for brand colors |
| Test both light and dark modes | Assume dark mode works without testing |
| Use `@layer` to organize custom CSS | Write custom CSS without proper layers |
| Configure container padding for responsive design | Use fixed container widths without padding |

## Quick Fixes

- **Colors not working** → Check if color is defined in theme.extend.colors
- **Styles being purged** → Add file patterns to content array
- **Dark mode not switching** → Verify darkMode: 'class' and toggle implementation
- **Custom fonts not loading** → Add font files and update fontFamily config
- **Breakpoints not working** → Check screens configuration vs responsive prefixes
- **@layer styles not applying** → Ensure proper import order in CSS file

## Gotchas

⚠️ **Content Purging**: Dynamic class names need safelist or must be complete strings in code  
⚠️ **Theme Override vs Extend**: Using `theme.colors` replaces ALL colors, use `theme.extend.colors` instead  
⚠️ **@layer Order**: Base → Components → Utilities. Wrong order can cause specificity issues  
⚠️ **Dark Mode Strategy**: 'media' respects system preference, 'class' requires manual toggle  
⚠️ **Container Behavior**: Default container has no padding, must be configured explicitly

## Dark Mode Setup

```javascript
// Configuration
module.exports = {
  darkMode: 'class', // or 'media'
  // ... rest of config
}
```

```javascript
// Toggle implementation (React)
import { useState, useEffect } from 'react'

function DarkModeToggle() {
  const [darkMode, setDarkMode] = useState(false)

  useEffect(() => {
    if (darkMode) {
      document.documentElement.classList.add('dark')
    } else {
      document.documentElement.classList.remove('dark')
    }
  }, [darkMode])

  return (
    <button
      onClick={() => setDarkMode(!darkMode)}
      className="p-2 rounded-lg bg-gray-200 dark:bg-gray-800"
    >
      {darkMode ? '☀️' : '🌙'}
    </button>
  )
}
```

```html
<!-- Usage in templates -->
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  <h1 class="text-2xl font-bold">Dark mode ready!</h1>
</div>
```

## Checklist

- [ ] Configure content paths for all file types using Tailwind
- [ ] Set up theme extensions for brand colors and typography
- [ ] Choose and implement dark mode strategy (media vs class)
- [ ] Organize custom CSS using @layer directives
- [ ] Test responsive breakpoints and container behavior
- [ ] Verify purging works correctly in production build
- [ ] Add safelist for any dynamic class names