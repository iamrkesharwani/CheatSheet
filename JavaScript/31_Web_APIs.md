# JavaScript Web APIs `v 0.07.25.01`

## Essentials
```javascript
// Web Storage - persistent data
localStorage.setItem('key', 'value');
const data = localStorage.getItem('key');

// History - navigation control
history.pushState({page: 1}, 'Title', '/page1');
history.back();

// URL - parse and manipulate URLs
const url = new URL('https://example.com/path?param=value');
console.log(url.pathname, url.searchParams.get('param'));

// Fetch - HTTP requests
const response = await fetch('/api/data', {
  method: 'POST',
  body: JSON.stringify(data),
  headers: {'Content-Type': 'application/json'}
});
```

**Key Concepts**: Storage | Navigation | HTTP | Observation | Location | Notifications
**When to use**: Client-side data persistence, SPA navigation, API calls, DOM monitoring, user location, push notifications

---

## Web Storage API

### Basic Structure
```javascript
// localStorage - persistent across sessions
localStorage.setItem('user', JSON.stringify({name: 'John', id: 123}));
const user = JSON.parse(localStorage.getItem('user'));
localStorage.removeItem('user');
localStorage.clear(); // Remove all items

// sessionStorage - cleared when tab closes
sessionStorage.setItem('tempData', 'value');
const temp = sessionStorage.getItem('tempData');

// Storage events - listen for changes
window.addEventListener('storage', (e) => {
  console.log(`Key: ${e.key}, Old: ${e.oldValue}, New: ${e.newValue}`);
});
```

### Common Patterns
```javascript
// Pattern 1: Safe JSON storage with error handling
function setStorageItem(key, value) {
  try {
    localStorage.setItem(key, JSON.stringify(value));
    return true;
  } catch (e) {
    console.error('Storage failed:', e);
    return false;
  }
}

// Pattern 2: Storage with expiration
function setWithExpiry(key, value, ttl) {
  const item = {
    value: value,
    expiry: Date.now() + ttl
  };
  localStorage.setItem(key, JSON.stringify(item));
}

function getWithExpiry(key) {
  const item = JSON.parse(localStorage.getItem(key));
  if (!item || Date.now() > item.expiry) {
    localStorage.removeItem(key);
    return null;
  }
  return item.value;
}
```

---

## History API

### Basic Structure
```javascript
// Push new state without page reload
history.pushState({page: 'home'}, 'Home Page', '/home');

// Replace current state
history.replaceState({page: 'updated'}, 'Updated', '/updated');

// Navigate programmatically
history.back();    // Go back one page
history.forward(); // Go forward one page
history.go(-2);    // Go back 2 pages

// Listen for navigation
window.addEventListener('popstate', (event) => {
  console.log('State:', event.state);
  // Handle navigation
});
```

### Common Patterns
```javascript
// Pattern 1: SPA routing
function navigateTo(path, data = {}) {
  history.pushState(data, '', path);
  renderPage(path, data);
}

// Pattern 2: Prevent accidental navigation
window.addEventListener('beforeunload', (e) => {
  if (hasUnsavedChanges) {
    e.preventDefault();
    e.returnValue = ''; // Required for Chrome
  }
});
```

---

## URL API

### Basic Structure
```javascript
// Create URL object
const url = new URL('https://example.com:8080/path?name=john&age=25#section');

// Access components
console.log(url.protocol);   // 'https:'
console.log(url.hostname);   // 'example.com'
console.log(url.port);       // '8080'
console.log(url.pathname);   // '/path'
console.log(url.search);     // '?name=john&age=25'
console.log(url.hash);       // '#section'

// Work with search parameters
const params = url.searchParams;
params.get('name');      // 'john'
params.set('city', 'NY'); // Add parameter
params.delete('age');    // Remove parameter
params.toString();       // Get query string
```

### Common Patterns
```javascript
// Pattern 1: Parse current page URL
const currentUrl = new URL(window.location.href);
const userId = currentUrl.searchParams.get('userId');

// Pattern 2: Build API URLs dynamically
function buildApiUrl(endpoint, params = {}) {
  const url = new URL(endpoint, 'https://api.example.com');
  Object.entries(params).forEach(([key, value]) => {
    url.searchParams.set(key, value);
  });
  return url.toString();
}

// Usage: buildApiUrl('/users', {page: 1, limit: 10})
```

---

## Fetch API Advanced Features

### Basic Structure
```javascript
// GET request with headers
const response = await fetch('/api/data', {
  headers: {
    'Authorization': 'Bearer ' + token,
    'Content-Type': 'application/json'
  }
});

// POST with JSON body
const postResponse = await fetch('/api/users', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({name: 'John', email: 'john@example.com'})
});

// Handle response
if (response.ok) {
  const data = await response.json();
} else {
  throw new Error(`HTTP error! status: ${response.status}`);
}
```

### Common Patterns
```javascript
// Pattern 1: Request with timeout and retry
async function fetchWithTimeout(url, options = {}, timeout = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeout);
  
  try {
    const response = await fetch(url, {
      ...options,
      signal: controller.signal
    });
    clearTimeout(timeoutId);
    return response;
  } catch (error) {
    clearTimeout(timeoutId);
    throw error;
  }
}

// Pattern 2: Upload files with progress
async function uploadFile(file, onProgress) {
  const formData = new FormData();
  formData.append('file', file);
  
  return fetch('/upload', {
    method: 'POST',
    body: formData,
    // Don't set Content-Type - browser sets it with boundary
  });
}

// Pattern 3: Stream processing
async function processStream(url) {
  const response = await fetch(url);
  const reader = response.body.getReader();
  
  while (true) {
    const {done, value} = await reader.read();
    if (done) break;
    // Process chunk
    console.log(new TextDecoder().decode(value));
  }
}
```

---

## IntersectionObserver

### Basic Structure
```javascript
// Create observer
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('Element is visible:', entry.target);
      // Trigger action when element comes into view
    }
  });
}, {
  threshold: 0.5,    // Trigger when 50% visible
  rootMargin: '10px' // Extend root area by 10px
});

// Observe elements
const target = document.querySelector('.lazy-image');
observer.observe(target);

// Stop observing
observer.unobserve(target);
observer.disconnect(); // Stop all observations
```

### Common Patterns
```javascript
// Pattern 1: Lazy loading images
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src; // Load actual image
      img.classList.add('loaded');
      imageObserver.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  imageObserver.observe(img);
});

// Pattern 2: Infinite scroll
const loadMoreObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      loadMoreContent();
    }
  });
}, { threshold: 1.0 });

loadMoreObserver.observe(document.querySelector('.load-trigger'));
```

---

## MutationObserver

### Basic Structure
```javascript
// Create observer
const observer = new MutationObserver((mutations) => {
  mutations.forEach(mutation => {
    console.log('Mutation type:', mutation.type);
    console.log('Added nodes:', mutation.addedNodes);
    console.log('Removed nodes:', mutation.removedNodes);
  });
});

// Start observing
observer.observe(document.body, {
  childList: true,     // Watch for child additions/removals
  attributes: true,    // Watch for attribute changes
  subtree: true,       // Watch entire subtree
  attributeOldValue: true,
  characterData: true  // Watch text content changes
});

// Stop observing
observer.disconnect();
```

### Common Patterns
```javascript
// Pattern 1: Watch for specific class changes
const classObserver = new MutationObserver((mutations) => {
  mutations.forEach(mutation => {
    if (mutation.type === 'attributes' && mutation.attributeName === 'class') {
      const target = mutation.target;
      if (target.classList.contains('active')) {
        console.log('Element became active');
      }
    }
  });
});

classObserver.observe(element, { attributes: true });

// Pattern 2: Auto-process dynamically added content
const contentObserver = new MutationObserver((mutations) => {
  mutations.forEach(mutation => {
    mutation.addedNodes.forEach(node => {
      if (node.nodeType === Node.ELEMENT_NODE) {
        // Process new elements (e.g., bind events, apply styles)
        processNewElement(node);
      }
    });
  });
});

contentObserver.observe(document.body, { childList: true, subtree: true });
```

---

## Geolocation API

### Basic Structure
```javascript
// Get current position
navigator.geolocation.getCurrentPosition(
  (position) => {
    const {latitude, longitude} = position.coords;
    console.log(`Location: ${latitude}, ${longitude}`);
    console.log(`Accuracy: ${position.coords.accuracy} meters`);
  },
  (error) => {
    console.error('Geolocation error:', error.message);
  },
  {
    enableHighAccuracy: true,
    timeout: 10000,
    maximumAge: 60000
  }
);

// Watch position changes
const watchId = navigator.geolocation.watchPosition(
  (position) => {
    updateUserLocation(position.coords);
  },
  (error) => console.error(error),
  { enableHighAccuracy: true }
);

// Stop watching
navigator.geolocation.clearWatch(watchId);
```

### Common Patterns
```javascript
// Pattern 1: Promise-based geolocation
function getCurrentLocation(options = {}) {
  return new Promise((resolve, reject) => {
    if (!navigator.geolocation) {
      reject(new Error('Geolocation not supported'));
    }
    
    navigator.geolocation.getCurrentPosition(
      resolve,
      reject,
      {
        enableHighAccuracy: false,
        timeout: 10000,
        maximumAge: 300000, // 5 minutes
        ...options
      }
    );
  });
}

// Usage
try {
  const position = await getCurrentLocation();
  console.log(position.coords.latitude, position.coords.longitude);
} catch (error) {
  console.error('Location error:', error.message);
}
```

---

## Notification API

### Basic Structure
```javascript
// Request permission
const permission = await Notification.requestPermission();
if (permission === 'granted') {
  // Create notification
  const notification = new Notification('Hello!', {
    body: 'This is a notification message',
    icon: '/icon.png',
    badge: '/badge.png',
    tag: 'unique-id', // Prevents duplicates
    requireInteraction: true,
    actions: [
      {action: 'view', title: 'View'},
      {action: 'dismiss', title: 'Dismiss'}
    ]
  });
  
  // Handle events
  notification.onclick = () => {
    window.focus();
    notification.close();
  };
}
```

### Common Patterns
```javascript
// Pattern 1: Smart notification function
async function showNotification(title, options = {}) {
  if (!('Notification' in window)) {
    console.warn('Notifications not supported');
    return null;
  }
  
  let permission = Notification.permission;
  if (permission === 'default') {
    permission = await Notification.requestPermission();
  }
  
  if (permission === 'granted') {
    return new Notification(title, {
      icon: '/favicon.ico',
      badge: '/badge.png',
      ...options
    });
  }
  
  return null;
}

// Pattern 2: Service Worker notifications (more reliable)
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js').then(registration => {
    registration.showNotification('Title', {
      body: 'Message from service worker',
      icon: '/icon.png'
    });
  });
}
```

---

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Check API support before using | Assume all browsers support all APIs |
| Handle storage quota exceeded errors | Store large objects without compression |
| Use HTTPS for geolocation and notifications | Use location APIs on insecure connections |
| Implement proper error handling for fetch | Ignore network failures and timeouts |
| Clean up observers when done | Leave observers running indefinitely |
| Request notification permission contextually | Spam users with permission requests |

---

## Quick Fixes

- **Storage quota exceeded** → Clear old data, compress values, or use IndexedDB for large data
- **Geolocation timeout** → Increase timeout, set maximumAge, or provide fallback location
- **Fetch CORS error** → Configure server CORS headers or use a proxy
- **Notification permission denied** → Provide alternative UI notifications or email
- **Observer not triggering** → Check threshold values and rootMargin settings
- **History state lost** → Always pass state object, don't rely on undefined

---

## Gotchas

⚠️ **Storage**: Data is domain-specific and has size limits (~5-10MB). localStorage is synchronous and can block the main thread

⚠️ **History**: popstate doesn't fire on initial page load. Use replaceState for updating current entry without adding history

⚠️ **Fetch**: Doesn't reject on HTTP error status (404, 500). Check response.ok explicitly

⚠️ **Observers**: Can cause memory leaks if not properly disconnected. Always clean up in component unmount

⚠️ **Geolocation**: Requires HTTPS in production. Can be slow and drain battery on mobile devices

⚠️ **Notifications**: Permission is persistent per origin. Once denied, must be reset manually in browser settings

---

## Checklist

- [ ] Check browser support for target APIs using feature detection
- [ ] Implement proper error handling for all async operations  
- [ ] Clean up observers, event listeners, and watchers when components unmount
- [ ] Use HTTPS for secure context APIs (geolocation, notifications)
- [ ] Handle storage quota limits and implement fallback strategies
- [ ] Test notification permissions and provide graceful degradation
- [ ] Validate and sanitize data before storing or transmitting
- [ ] Consider performance impact of frequent observer callbacks
- [ ] Implement request timeouts and abort controllers for fetch requests
- [ ] Test APIs across different browsers and devices