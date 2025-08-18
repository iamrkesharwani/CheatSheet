# Working with APIs `v ES2022`

## Essentials

```javascript
// Basic fetch - GET request
const response = await fetch('https://api.example.com/data');
const data = await response.json();

// Check response status
if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`);
}

// POST request with JSON data
const response = await fetch('/api/posts', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ title: 'New Post', content: 'Content' })
});

// Cancel requests with AbortController
const controller = new AbortController();
fetch('/api/data', { signal: controller.signal });
controller.abort(); // Cancel the request
```

**Key Concepts**: HTTP Methods | Status Codes | JSON Parsing | Error Handling | CORS

**When to use**: Fetching data from servers, submitting forms, building web applications that communicate with APIs

## Syntax Reference

### Basic Fetch Structure

```javascript
// GET request with error handling
async function fetchData(url) {
  try {
    const response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch error:', error);
    throw error;
  }
}

// POST request with data
async function createResource(url, data) {
  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer token'
    },
    body: JSON.stringify(data)
  });
  
  return await response.json();
}
```

### HTTP Methods

```javascript
// CRUD operations
const api = {
  // CREATE - POST
  async create(data) {
    return await fetch('/api/posts', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
  },
  
  // READ - GET
  async get(id) {
    return await fetch(`/api/posts/${id}`);
  },
  
  // UPDATE - PUT (full replacement)
  async update(id, data) {
    return await fetch(`/api/posts/${id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
  },
  
  // UPDATE - PATCH (partial update)
  async patch(id, data) {
    return await fetch(`/api/posts/${id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
  },
  
  // DELETE
  async delete(id) {
    return await fetch(`/api/posts/${id}`, {
      method: 'DELETE'
    });
  }
};
```

### Response Handling

```javascript
// Different response types
const response = await fetch('/api/data');

// JSON data
const jsonData = await response.json();

// Plain text
const textData = await response.text();

// Binary data
const blobData = await response.blob();

// Check content type
const contentType = response.headers.get('content-type');
if (contentType && contentType.includes('application/json')) {
  const data = await response.json();
}

// Response status checking
if (response.ok) {
  // Status 200-299
} else if (response.status === 404) {
  // Not found
} else if (response.status >= 500) {
  // Server error
}
```

## Common Patterns

```javascript
// Pattern 1: Request manager with cancellation
class RequestManager {
  constructor() {
    this.activeRequests = new Map();
  }
  
  async fetch(key, url, options = {}) {
    // Cancel existing request
    this.cancel(key);
    
    const controller = new AbortController();
    this.activeRequests.set(key, controller);
    
    try {
      const response = await fetch(url, {
        ...options,
        signal: controller.signal
      });
      
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      
      const data = await response.json();
      this.activeRequests.delete(key);
      return data;
    } catch (error) {
      this.activeRequests.delete(key);
      if (error.name !== 'AbortError') throw error;
    }
  }
  
  cancel(key) {
    const controller = this.activeRequests.get(key);
    if (controller) {
      controller.abort();
      this.activeRequests.delete(key);
    }
  }
}

// Pattern 2: Retry mechanism
async function fetchWithRetry(url, maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(url);
      if (response.ok) return await response.json();
      
      if (response.status >= 500 && attempt < maxRetries) {
        await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
        continue;
      }
      throw new Error(`HTTP ${response.status}`);
    } catch (error) {
      if (attempt === maxRetries) throw error;
      await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
    }
  }
}

// Pattern 3: Parallel requests
async function fetchMultiple(urls) {
  const promises = urls.map(url => fetch(url).then(r => r.json()));
  return await Promise.all(promises);
}

// Pattern 4: Loading and error states
class APIState {
  constructor() {
    this.loading = false;
    this.error = null;
    this.data = null;
  }
  
  async fetchData(url) {
    this.loading = true;
    this.error = null;
    
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      
      this.data = await response.json();
    } catch (error) {
      this.error = error.message;
    } finally {
      this.loading = false;
    }
  }
}
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Always check response.ok before parsing | Assume fetch will throw on HTTP errors |
| Handle network errors with try/catch | Ignore CORS errors in development |
| Use AbortController for cancellable requests | Parse response body multiple times |
| Include proper Content-Type headers | Send sensitive data in GET query params |
| Implement loading and error states in UI | Block UI thread with synchronous operations |

## Quick Fixes

- **CORS blocked request** → Check server CORS headers or use proxy in development
- **fetch doesn't throw on 404/500** → Check `response.ok` before parsing
- **Can't read response body twice** → Clone response with `response.clone()`
- **Request hangs indefinitely** → Add timeout with AbortController
- **Memory leaks with requests** → Cancel requests when component unmounts
- **Form submission not working** → Check Content-Type header for JSON data

## Gotchas

⚠️ **fetch doesn't reject on HTTP errors**: Status 404 or 500 won't trigger catch block. Always check `response.ok`

⚠️ **Response body can only be read once**: Use `response.clone()` if you need to read it multiple times

⚠️ **CORS in development**: Browsers block cross-origin requests. Use proxy or CORS headers

⚠️ **FormData Content-Type**: Don't set Content-Type header when sending FormData, let browser set it

⚠️ **Credentials with CORS**: Use `credentials: 'include'` to send cookies cross-origin

## Checklist

- [ ] Check response.ok before parsing JSON
- [ ] Handle both network errors and HTTP errors
- [ ] Include proper headers for POST/PUT requests
- [ ] Implement request cancellation for user interactions
- [ ] Show loading states during API calls
- [ ] Handle CORS properly for cross-origin requests
- [ ] Use HTTPS in production for security
- [ ] Implement retry logic for failed requests
- [ ] Validate and sanitize data from APIs
- [ ] Cancel pending requests on page navigation