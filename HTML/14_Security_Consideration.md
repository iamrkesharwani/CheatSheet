# Web Security Considerations Cheatsheet

> **Purpose**: Essential security practices to protect web applications from common vulnerabilities
> **Version**: v1.0 | **Updated**: August 2025

---

## 🚀 Quick Start

```html
<!-- Minimal secure setup -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'">
    <title>Secure App</title>
</head>
<body>
    <!-- Always sanitize user input -->
    <div id="content"></div>
    <script>
        // Safe way to display user content
        document.getElementById('content').textContent = userInput;
    </script>
</body>
</html>
```

**Key concepts**: XSS prevention through proper encoding, CSP headers for attack mitigation, input validation at all entry points.

---

## 📋 Syntax Essentials

### Basic Structure
```javascript
// Secure coding foundation
const sanitizeInput = (input) => {
    return input.replace(/[<>"'&]/g, (match) => {
        const escapeMap = {
            '<': '&lt;',
            '>': '&gt;',
            '"': '&quot;',
            "'": '&#39;',
            '&': '&amp;'
        };
        return escapeMap[match];
    });
};

// Safe DOM manipulation
element.textContent = userInput;  // Safe
element.innerHTML = sanitizeInput(userInput);  // Better
```

### Security Headers
| Header | Purpose | Example | Notes |
|--------|---------|---------|-------|
| CSP | Prevent XSS | `Content-Security-Policy: default-src 'self'` | Blocks inline scripts |
| HTTPS | Encrypt traffic | `Strict-Transport-Security: max-age=31536000` | Force HTTPS |
| X-Frame-Options | Prevent clickjacking | `X-Frame-Options: DENY` | Blocks iframe embedding |

### Input Validation
```javascript
// Always validate on both client AND server
function validateEmail(email) {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email) && email.length < 255;
}

// Sanitize before processing
function processUserData(input) {
    if (!input || typeof input !== 'string') return '';
    return input.trim().slice(0, 1000); // Length limit
}
```

---

## 🔧 Core Features

### XSS Prevention
```javascript
// DOM-based XSS prevention
// Bad - vulnerable to XSS
element.innerHTML = userInput;

// Good - safe content insertion
element.textContent = userInput;
element.setAttribute('data-value', userInput);

// Template literals - be careful
const template = `<div>${escapeHtml(userInput)}</div>`;
```
- **When to use**: Every time you handle user input
- **Gotchas**: innerHTML, eval(), document.write() are dangerous

### Content Security Policy
```html
<!-- Restrictive CSP header -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               script-src 'self' 'unsafe-inline'; 
               style-src 'self' 'unsafe-inline';
               img-src 'self' data: https:">
```
- **Pattern**: Start restrictive, then allow specific sources
- **Returns**: Blocks unauthorized script execution

### Input Sanitization
```javascript
// Server-side validation (Node.js example)
const validator = require('validator');

function sanitizeInput(input) {
    if (!input) return '';
    
    // Escape HTML entities
    let clean = validator.escape(input);
    
    // Remove potential script tags
    clean = clean.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');
    
    return clean.trim();
}
```

---

## 📖 Common Patterns

### Safe User Content Display
```javascript
// Before (vulnerable)
function displayComment(comment) {
    document.getElementById('comments').innerHTML += `
        <div class="comment">${comment.text}</div>
    `;
}

// After (secure)
function displayComment(comment) {
    const div = document.createElement('div');
    div.className = 'comment';
    div.textContent = comment.text; // Automatically escapes
    document.getElementById('comments').appendChild(div);
}
```
**Use case**: When displaying user-generated content

### Form Validation Pattern
```javascript
// Complete form security pattern
function secureFormSubmit(formData) {
    // 1. Client-side validation (UX)
    if (!validateInput(formData)) {
        showError('Invalid input');
        return false;
    }
    
    // 2. Sanitize before sending
    const cleanData = sanitizeFormData(formData);
    
    // 3. Send over HTTPS
    fetch('/api/submit', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'X-CSRF-Token': getCsrfToken()
        },
        body: JSON.stringify(cleanData)
    });
}
```
**Pro tip**: Never trust client-side validation alone - always validate on server

---

## 🎯 Best Practices

| ✅ Do | ❌ Don't | Why |
|-------|----------|-----|
| `element.textContent = input` | `element.innerHTML = input` | Prevents XSS injection |
| Validate on server | Trust client validation | Client can be bypassed |
| Use HTTPS everywhere | HTTP for "non-sensitive" data | All data needs protection |
| Implement CSP headers | Rely on input filtering alone | Defense in depth |

### Code Style
```javascript
// Security-first approach
function handleUserInput(input) {
    // 1. Validate first
    if (!isValid(input)) {
        throw new Error('Invalid input');
    }
    
    // 2. Sanitize
    const clean = sanitize(input);
    
    // 3. Use safely
    return processCleanInput(clean);
}
```

---

## 🔍 Debugging & Testing

### Common Errors
| Error Message | Cause | Fix |
|---------------|-------|-----|
| `CSP violation` | Inline script blocked | Move scripts to external files or add nonce |
| `Mixed content` | HTTP resources on HTTPS | Update all URLs to HTTPS |
| `XSS detected` | Unescaped user input | Use textContent or proper escaping |

### Security Testing
```javascript
// Test for XSS vulnerabilities
const xssPayloads = [
    '<script>alert("XSS")</script>',
    'javascript:alert("XSS")',
    '<img src=x onerror=alert("XSS")>'
];

// Test each input field
xssPayloads.forEach(payload => {
    console.log('Testing:', payload);
    // Verify payload is properly escaped/blocked
});
```

---

## 📚 Advanced Features

### CSRF Protection
```javascript
// Generate CSRF token
function generateCsrfToken() {
    return crypto.randomBytes(32).toString('hex');
}

// Include in forms
function addCsrfToken(form) {
    const token = getCsrfToken();
    const input = document.createElement('input');
    input.type = 'hidden';
    input.name = '_csrf';
    input.value = token;
    form.appendChild(input);
}
```

### Content Security Policy Reporting
```html
<!-- CSP with violation reporting -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; report-uri /csp-violation-report">
```

---

## 🛠️ Tools & Setup

### HTTPS Setup
```bash
# Generate SSL certificate (development)
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

### Security Headers Configuration
```nginx
# Nginx security headers
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Content-Security-Policy "default-src 'self'" always;
```

### Security Testing Tools
```bash
# Check HTTPS configuration
curl -I https://yoursite.com

# Test security headers
curl -I -s https://yoursite.com | grep -i security

# CSP validator
csp-evaluator --url https://yoursite.com
```

---

## 📖 Reference

### Essential Security Functions
| Function | Purpose | Example | Returns |
|----------|---------|---------|---------|
| `textContent` | Safe text insertion | `el.textContent = input` | Escaped string |
| `setAttribute()` | Safe attribute setting | `el.setAttribute('data', input)` | Escaped attribute |
| `validator.escape()` | HTML entity encoding | `validator.escape('<script>')` | `&lt;script&gt;` |

### Security Headers Reference
| Header | Purpose | Example Value |
|--------|---------|---------------|
| `Content-Security-Policy` | XSS prevention | `default-src 'self'` |
| `X-Frame-Options` | Clickjacking prevention | `DENY` |
| `Strict-Transport-Security` | Force HTTPS | `max-age=31536000` |

---

## 🔗 Quick Links

- [OWASP Top 10](https://owasp.org/Top10/) - Most critical security risks
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/) - Test CSP policies
- [Security Headers](https://securityheaders.com/) - Analyze security headers
- [Mozilla Observatory](https://observatory.mozilla.org/) - Security assessment tool

---

## 💡 Tips & Tricks

- **XSS Prevention**: Use `textContent` instead of `innerHTML` whenever possible
- **CSP Strategy**: Start with `default-src 'self'` and add exceptions as needed
- **Input Validation**: Validate on both client (UX) and server (security)
- **HTTPS Everywhere**: Even "non-sensitive" data needs encryption in transit

---

## 🚨 Security Checklist

- [ ] **XSS Prevention**: All user input properly escaped/sanitized
- [ ] **CSP Headers**: Content Security Policy implemented and tested
- [ ] **Input Validation**: Server-side validation for all inputs
- [ ] **HTTPS**: All traffic encrypted, HSTS headers set
- [ ] **Security Headers**: X-Frame-Options, X-Content-Type-Options configured  
- [ ] **CSRF Protection**: Anti-CSRF tokens on state-changing operations
- [ ] **Error Handling**: No sensitive info leaked in error messages
- [ ] **Dependencies**: All packages updated and vulnerability-scanned

---

**Remember**: 
- Security is layered - no single measure is enough
- Always validate and sanitize user input
- Use HTTPS everywhere, not just for "sensitive" pages
- Test your CSP policy thoroughly before deploying