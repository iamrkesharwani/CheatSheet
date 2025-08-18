# JavaScript Security `v 0.07.25.01`

## Essentials
```javascript
// Input sanitization
const sanitize = (input) => input.replace(/[<>&"']/g, (c) => ({
  '<': '&lt;', '>': '&gt;', '&': '&amp;', '"': '&quot;', "'": '&#x27;'
})[c]);

// Safe DOM manipulation
element.textContent = userInput; // Not innerHTML
element.setAttribute('data-value', sanitize(userInput));

// CSRF protection
fetch('/api/data', {
  headers: { 'X-CSRF-Token': sessionStorage.getItem('csrfToken') }
});
```

**Key Concepts**: XSS Prevention | Input Validation | CSRF Protection | CSP | Secure Headers
**When to use**: All user input handling, API calls, dynamic content rendering

## Syntax Reference

### XSS Prevention
```javascript
// HTML encoding function
function htmlEncode(str) {
  return str.replace(/&/g, '&amp;')
            .replace(/</g, '&lt;')
            .replace(/>/g, '&gt;')
            .replace(/"/g, '&quot;')
            .replace(/'/g, '&#x27;')
            .replace(/\//g, '&#x2F;');
}

// Safe content insertion
function safeAddContent(element, content) {
  element.textContent = content; // Instead of innerHTML
}

// URL parameter sanitization
function getSecureUrlParam(param) {
  const urlParams = new URLSearchParams(window.location.search);
  const value = urlParams.get(param);
  return value ? htmlEncode(value) : null;
}

// Using DOMPurify (recommended library)
// const clean = DOMPurify.sanitize(userInput);

// When innerHTML is necessary
function safeInnerHTML(element, html) {
  const clean = DOMPurify.sanitize(html);
  element.innerHTML = clean;
}
```

### Input Validation  
```javascript
// Validation patterns
const validators = {
  email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  phone: /^\+?[\d\s-()]+$/,
  alphanumeric: /^[a-zA-Z0-9]+$/,
  url: /^https?:\/\/.+/,
  strongPassword: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/
};

// Validation function
function validateInput(input, type) {
  if (!input || typeof input !== 'string') return false;
  return validators[type]?.test(input) || false;
}

// Length validation
function validateLength(input, min = 0, max = Infinity) {
  return input.length >= min && input.length <= max;
}

// Whitelist validation
function validateWhitelist(input, allowedValues) {
  return allowedValues.includes(input);
}

// File upload validation
function validateFileUpload(file) {
  const allowedTypes = ['image/jpeg', 'image/png', 'image/gif', 'application/pdf'];
  const maxSize = 5 * 1024 * 1024; // 5MB
  
  return allowedTypes.includes(file.type) && file.size <= maxSize;
}
```

### CSRF Protection
```javascript
// CSRF token generation
function generateCSRFToken() {
  return btoa(Math.random().toString()).substring(0, 32);
}

// Add CSRF token to forms
function addCSRFTokenToForm(form) {
  const tokenInput = document.createElement('input');
  tokenInput.type = 'hidden';
  tokenInput.name = 'csrfToken';
  tokenInput.value = sessionStorage.getItem('csrfToken');
  form.appendChild(tokenInput);
}

// Secure AJAX requests
function secureAjaxRequest(url, data, method = 'POST') {
  const csrfToken = sessionStorage.getItem('csrfToken');
  
  return fetch(url, {
    method: method,
    headers: {
      'Content-Type': 'application/json',
      'X-CSRF-Token': csrfToken,
      'X-Requested-With': 'XMLHttpRequest'
    },
    body: JSON.stringify(data),
    credentials: 'same-origin'
  });
}

// Double Submit Cookie Pattern
function doubleSubmitCookieCSRF() {
  const token = generateCSRFToken();
  document.cookie = `csrfToken=${token}; SameSite=Strict; Secure`;
  return token;
}
```

### Content Security Policy (CSP)
```javascript
// CSP violation handling
function handleCSPViolation(event) {
  const violation = event.originalEvent || event;
  console.warn('CSP Violation:', violation.violatedDirective);
  
  // Report to server
  fetch('/csp-violation-report', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({
      directive: violation.violatedDirective,
      blockedURI: violation.blockedURI,
      sourceFile: violation.sourceFile,
      lineNumber: violation.lineNumber
    })
  });
}

// Add CSP violation listener
document.addEventListener('securitypolicyviolation', handleCSPViolation);

// Generate CSP nonce
function generateCSPNonce() {
  return btoa(Math.random().toString()).substring(0, 16);
}

// CSP header example (server-side)
// Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-{random}'
```

### Secure Storage & Cookies
```javascript
// Secure cookie settings
function setSecureCookie(name, value, days = 7) {
  const expires = new Date(Date.now() + days * 24 * 60 * 60 * 1000);
  document.cookie = `${name}=${value}; expires=${expires.toUTCString()}; Secure; HttpOnly; SameSite=Strict; Path=/`;
}

// Secure localStorage wrapper
const SecureStorage = {
  set(key, value) {
    try {
      const encrypted = btoa(JSON.stringify(value));
      localStorage.setItem(key, encrypted);
    } catch (e) {
      console.error('Storage error:', e);
    }
  },
  
  get(key) {
    try {
      const encrypted = localStorage.getItem(key);
      return encrypted ? JSON.parse(atob(encrypted)) : null;
    } catch (e) {
      console.error('Storage error:', e);
      return null;
    }
  },
  
  remove(key) {
    localStorage.removeItem(key);
  }
};
```

## Common Patterns

### Password Security
```javascript
// Password strength checker
function checkPasswordStrength(password) {
  const checks = {
    length: password.length >= 8,
    uppercase: /[A-Z]/.test(password),
    lowercase: /[a-z]/.test(password),
    numbers: /\d/.test(password),
    symbols: /[!@#$%^&*(),.?":{}|<>]/.test(password),
    noSequential: !/123|abc|qwe/i.test(password)
  };
  
  const score = Object.values(checks).filter(Boolean).length;
  const strength = score < 3 ? 'weak' : score < 5 ? 'medium' : 'strong';
  
  return { score, strength, checks };
}
```

### Rate Limiting
```javascript
// Client-side rate limiter
class RateLimiter {
  constructor(maxRequests, timeWindow) {
    this.maxRequests = maxRequests;
    this.timeWindow = timeWindow;
    this.requests = [];
  }
  
  isAllowed() {
    const now = Date.now();
    this.requests = this.requests.filter(time => now - time < this.timeWindow);
    
    if (this.requests.length >= this.maxRequests) {
      return false;
    }
    
    this.requests.push(now);
    return true;
  }
}

// Usage: const limiter = new RateLimiter(5, 60000); // 5 requests per minute
```

### Secure File Upload
```javascript
// Secure file upload handler
function handleSecureFileUpload(fileInput, callback) {
  const file = fileInput.files[0];
  if (!file) return;
  
  // Validate file
  if (!validateFileUpload(file)) {
    callback({ type: 'error', message: 'Invalid file type or size' });
    return;
  }
  
  // Create secure form data
  const formData = new FormData();
  formData.append('file', file);
  formData.append('csrfToken', sessionStorage.getItem('csrfToken'));
  
  // Upload with progress tracking
  const xhr = new XMLHttpRequest();
  xhr.upload.onprogress = (e) => {
    if (e.lengthComputable) {
      const percentComplete = (e.loaded / e.total) * 100;
      callback({ type: 'progress', percent: percentComplete });
    }
  };
  
  xhr.onload = () => {
    if (xhr.status === 200) {
      callback({ type: 'success', response: xhr.responseText });
    } else {
      callback({ type: 'error', message: 'Upload failed' });
    }
  };
  
  xhr.open('POST', '/secure-upload');
  xhr.send(formData);
}
```

### Clickjacking Prevention
```javascript
// Prevent clickjacking
function preventClickjacking() {
  if (window.top !== window.self) {
    window.top.location = window.self.location;
  }
}

// Call on page load
preventClickjacking();
```

### Secure WebSocket
```javascript
// Secure WebSocket connection
function createSecureWebSocket(url) {
  const wsUrl = url.replace(/^http/, 'ws');
  const socket = new WebSocket(wsUrl);
  
  socket.onopen = (event) => {
    // Send authentication token
    socket.send(JSON.stringify({
      type: 'auth',
      token: sessionStorage.getItem('authToken')
    }));
  };
  
  socket.onmessage = (event) => {
    try {
      const data = JSON.parse(event.data);
      // Validate message structure
      if (data.type && data.payload) {
        handleMessage(data);
      }
    } catch (e) {
      console.error('Invalid WebSocket message:', e);
    }
  };
  
  return socket;
}
```

### Secure Random Generation
```javascript
// Secure random number generation
function secureRandom() {
  const array = new Uint32Array(1);
  window.crypto.getRandomValues(array);
  return array[0];
}

// Generate secure tokens
function generateSecureToken(length = 32) {
  const array = new Uint8Array(length);
  window.crypto.getRandomValues(array);
  return Array.from(array, byte => byte.toString(16).padStart(2, '0')).join('');
}
```

## Security Headers
```javascript
// Security headers (server-side configuration)
const SECURITY_HEADERS = {
  'X-Frame-Options': 'DENY',
  'X-Content-Type-Options': 'nosniff', 
  'X-XSS-Protection': '1; mode=block',
  'Strict-Transport-Security': 'max-age=31536000; includeSubDomains',
  'Referrer-Policy': 'strict-origin-when-cross-origin',
  'Content-Security-Policy': "default-src 'self'; script-src 'self' 'unsafe-inline'"
};
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Sanitize all user input before display | Trust any user input without validation |
| Use textContent instead of innerHTML when possible | Use innerHTML with unsanitized data |
| Implement CSRF tokens for state-changing requests | Rely on cookies alone for authentication |
| Set secure cookie flags (Secure, HttpOnly, SameSite) | Store sensitive data in localStorage without encryption |
| Validate file uploads (type, size, content) | Allow arbitrary file uploads |
| Use HTTPS for all communications | Send sensitive data over HTTP |
| Implement proper error handling | Expose detailed error messages to users |

## Quick Fixes
- **XSS vulnerability** → Replace innerHTML with textContent, use DOMPurify
- **Missing CSRF protection** → Add X-CSRF-Token header to requests
- **Insecure cookies** → Set Secure, HttpOnly, SameSite=Strict flags
- **CSP violations** → Review and update Content-Security-Policy header
- **Weak passwords** → Implement password strength requirements
- **File upload risks** → Validate file type, size, and scan content
- **Clickjacking attacks** → Set X-Frame-Options: DENY header

## Gotchas
⚠️ **Client-side validation is not security** - Always validate server-side too
⚠️ **Base64 encoding is not encryption** - Use proper encryption for sensitive data
⚠️ **HTTPS doesn't prevent all attacks** - Still need input validation and other protections
⚠️ **CSP can break existing functionality** - Test thoroughly when implementing
⚠️ **SameSite=Strict may break legitimate cross-site requests** - Consider SameSite=Lax
⚠️ **innerHTML with sanitized content may still be risky** - Prefer textContent when possible

## Checklist
- [ ] All user input is validated and sanitized
- [ ] HTTPS is used for all communications
- [ ] CSRF protection is implemented for state-changing requests
- [ ] Content Security Policy (CSP) is configured
- [ ] Security headers are set (X-Frame-Options, X-XSS-Protection, etc.)
- [ ] Cookies use Secure, HttpOnly, and SameSite flags
- [ ] File uploads are properly validated
- [ ] Error messages don't expose sensitive information
- [ ] Dependencies are regularly updated for security patches
- [ ] Rate limiting is implemented to prevent abuse
- [ ] Authentication tokens are stored securely
- [ ] Clickjacking protection is in place