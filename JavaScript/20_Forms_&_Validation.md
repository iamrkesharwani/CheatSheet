# JavaScript Forms & Validation `v 0.07.25.01`

## Essentials
```javascript
// Get form values
const form = document.getElementById('myForm');
const formData = new FormData(form);
const name = formData.get('name');

// Basic validation
if (!form.checkValidity()) {
    form.reportValidity();
    return;
}

// Custom error messages
input.setCustomValidity('Custom error message');
input.reportValidity();
```

**Key Concepts**: Form Data Extraction | Built-in Validation | Custom Validation | File Uploads
**When to use**: Any time you need to collect, validate, or submit user input in web applications

## Syntax Reference

### Getting Form Values
```javascript
// Method 1: Direct element access
const nameValue = document.getElementById('name').value;

// Method 2: Form elements collection
const form = document.getElementById('myForm');
const username = form.elements.username.value;

// Method 3: FormData API (recommended)
const formData = new FormData(form);
const email = formData.get('email');

// Special input types
const isChecked = checkbox.checked;
const selectedRadio = document.querySelector('input[name="gender"]:checked')?.value;
const selectedOptions = Array.from(multiSelect.selectedOptions).map(opt => opt.value);
```

### Built-in HTML5 Validation
```javascript
// HTML attributes for validation
<input type="email" name="email" required>
<input type="text" name="username" pattern="[a-zA-Z0-9]+" minlength="3" maxlength="20">
<input type="number" name="age" min="18" max="100" step="1">

// Check validation programmatically
const isValid = input.checkValidity();
const errorMessage = input.validationMessage;
```

### Custom Validation
```javascript
// Real-time validation
input.addEventListener('blur', function() {
    if (!this.value.includes('@')) {
        this.setCustomValidity('Please enter a valid email');
    } else {
        this.setCustomValidity(''); // Clear error
    }
});

// Form submission validation
form.addEventListener('submit', function(e) {
    e.preventDefault();
    
    const formData = new FormData(this);
    const errors = [];
    
    // Custom validation logic
    if (!formData.get('email').includes('@')) {
        errors.push('Invalid email format');
    }
    
    if (errors.length > 0) {
        alert('Errors: ' + errors.join(', '));
        return;
    }
    
    // Submit form
    submitForm(formData);
});
```

## Common Patterns

### Validation Rules System
```javascript
// Define validation rules
const validationRules = {
    username: {
        required: true,
        pattern: /^[a-zA-Z0-9_]+$/,
        minLength: 3,
        message: 'Username: 3+ chars, letters/numbers/underscore only'
    },
    email: {
        required: true,
        pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
        message: 'Please enter a valid email'
    }
};

// Apply validation
function validateField(fieldName, value) {
    const rule = validationRules[fieldName];
    if (!rule) return { valid: true };
    
    if (rule.required && !value) {
        return { valid: false, message: `${fieldName} is required` };
    }
    
    if (rule.pattern && !rule.pattern.test(value)) {
        return { valid: false, message: rule.message };
    }
    
    return { valid: true };
}
```

### File Upload Handling
```javascript
// Basic file upload
const fileInput = document.getElementById('fileInput');
fileInput.addEventListener('change', function(e) {
    const files = e.target.files;
    
    for (let file of files) {
        if (validateFile(file).valid) {
            uploadFile(file);
        }
    }
});

function validateFile(file) {
    const maxSize = 5 * 1024 * 1024; // 5MB
    const allowedTypes = ['image/jpeg', 'image/png'];
    
    if (file.size > maxSize) {
        return { valid: false, message: 'File too large' };
    }
    
    if (!allowedTypes.includes(file.type)) {
        return { valid: false, message: 'Invalid file type' };
    }
    
    return { valid: true };
}
```

### Form Submission with Fetch
```javascript
// Submit form data
async function submitForm(formData) {
    try {
        const response = await fetch('/submit-form', {
            method: 'POST',
            body: formData // Don't set Content-Type header with FormData
        });
        
        if (response.ok) {
            const result = await response.json();
            console.log('Success:', result);
        } else {
            throw new Error('Submission failed');
        }
    } catch (error) {
        console.error('Error:', error);
        alert('Submission failed. Please try again.');
    }
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use FormData for form submission | Manually serialize form data |
| Clear custom validity when input is corrected | Leave old error messages |
| Validate on both client and server | Rely only on client-side validation |
| Use built-in HTML5 validation attributes | Write custom validation for basic cases |
| Handle file upload errors gracefully | Assume file uploads always succeed |

## Quick Fixes
- **Form not submitting** → Check `e.preventDefault()` and validation logic
- **Custom validation not showing** → Call `input.reportValidity()` after `setCustomValidity()`
- **File upload failing** → Don't set `Content-Type` header with FormData
- **Validation triggering on page load** → Use 'blur' event instead of 'input' for initial validation
- **FormData returning null** → Ensure input has `name` attribute, not just `id`

## Gotchas
⚠️ **FormData with fetch**: Never set Content-Type header manually - browser sets it automatically with boundary

⚠️ **setCustomValidity('')**: Must clear custom validity with empty string, not just any falsy value

⚠️ **File input values**: Cannot be set programmatically for security reasons - only cleared with `input.value = ''`

⚠️ **Validation timing**: Built-in validation only triggers on form submission unless you call `checkValidity()` manually

## Validation Patterns
```javascript
// Common regex patterns
const patterns = {
    email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
    phone: /^\d{3}-\d{3}-\d{4}$/,
    username: /^[a-zA-Z0-9_]{3,20}$/,
    strongPassword: /(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d@$!%*?&]{8,}/,
    zipCode: /^\d{5}(-\d{4})?$/
};
```

## Checklist
- [ ] Form has proper `name` attributes on all inputs
- [ ] Validation errors are cleared when user corrects input
- [ ] File uploads have size and type validation
- [ ] Form submission prevents default browser behavior
- [ ] Error messages are user-friendly and specific
- [ ] Server-side validation exists (never trust client-side only)