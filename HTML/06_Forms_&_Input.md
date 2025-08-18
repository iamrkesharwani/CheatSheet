# HTML Forms & Inputs `v5.0`

## Essentials

```html
<!-- Basic form structure -->
<form action="/submit" method="post">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" required>

  <label for="password">Password:</label>
  <input type="password" id="password" name="password" required>

  <button type="submit">Submit</button>
</form>

<!-- Accessible form with validation -->
<form action="/register" method="post" novalidate>
  <fieldset>
    <legend>Account Information</legend>
    
    <label for="email">Email Address:</label>
    <input type="email" id="email" name="email" required 
           aria-describedby="email-help email-error">
    <div id="email-help">We'll never share your email</div>
    <div id="email-error" role="alert" aria-live="polite"></div>

    <input type="checkbox" id="terms" name="terms" value="agreed" required>
    <label for="terms">I agree to the terms and conditions</label>
  </fieldset>
  
  <button type="submit">Create Account</button>
</form>
```

**Key Concepts**: Form Validation | Accessibility | Input Types | User Experience

**When to use**: User registration, contact forms, surveys, data collection, search interfaces, and any user input scenarios

## Syntax Reference

### Form Structure & Methods

```html
<!-- GET method - data in URL (search, filters) -->
<form action="/search" method="get">
  <input type="search" name="q" placeholder="Search...">
  <select name="category">
    <option value="">All Categories</option>
    <option value="books">Books</option>
  </select>
  <button type="submit">Search</button>
</form>
<!-- Results in: /search?q=laptop&category=books -->

<!-- POST method - data in request body (sensitive data) -->
<form action="/login" method="post">
  <input type="email" name="email" required>
  <input type="password" name="password" required>
  <button type="submit">Login</button>
</form>

<!-- File upload form -->
<form action="/upload" method="post" enctype="multipart/form-data">
  <input type="file" name="avatar" accept="image/*" required>
  <button type="submit">Upload Photo</button>
</form>
```

### Input Types & Validation

```html
<!-- Text inputs with validation -->
<label for="username">Username (3-20 chars):</label>
<input type="text" id="username" name="username" 
       minlength="3" maxlength="20" 
       pattern="[a-zA-Z0-9_]+" 
       title="Letters, numbers, underscore only" required>

<!-- Email with built-in validation -->
<label for="email">Email:</label>
<input type="email" id="email" name="email" 
       placeholder="user@example.com" required>

<!-- Phone with pattern validation -->
<label for="phone">Phone (XXX-XXX-XXXX):</label>
<input type="tel" id="phone" name="phone" 
       pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" 
       placeholder="123-456-7890">

<!-- Number with constraints -->
<label for="age">Age:</label>
<input type="number" id="age" name="age" 
       min="18" max="100" step="1" required>

<!-- URL input -->
<label for="website">Website:</label>
<input type="url" id="website" name="website" 
       placeholder="https://www.example.com">

<!-- Password with strength requirements -->
<label for="password">Password (8+ chars):</label>
<input type="password" id="password" name="password" 
       minlength="8" required autocomplete="new-password">
```

### Date, Time & Advanced Inputs

```html
<!-- Date inputs -->
<label for="birthdate">Birth Date:</label>
<input type="date" id="birthdate" name="birthdate" 
       min="1900-01-01" max="2024-12-31">

<label for="appointment">Appointment:</label>
<input type="datetime-local" id="appointment" name="appointment">

<label for="meeting-time">Meeting Time:</label>
<input type="time" id="meeting-time" name="meeting_time" 
       min="09:00" max="17:00">

<!-- Color and range inputs -->
<label for="theme-color">Theme Color:</label>
<input type="color" id="theme-color" name="theme_color" value="#007bff">

<label for="volume">Volume:</label>
<input type="range" id="volume" name="volume" 
       min="0" max="100" value="50" step="5">

<!-- Search with datalist -->
<label for="search">Search:</label>
<input type="search" id="search" name="q" list="suggestions">
<datalist id="suggestions">
  <option value="JavaScript">
  <option value="Python">
  <option value="React">
</datalist>
```

### File Uploads

```html
<!-- Single file upload -->
<label for="resume">Resume (PDF only):</label>
<input type="file" id="resume" name="resume" 
       accept=".pdf,application/pdf" required>

<!-- Multiple file upload -->
<label for="photos">Photos (Multiple):</label>
<input type="file" id="photos" name="photos[]" 
       accept="image/jpeg,image/png,image/gif" 
       multiple>

<!-- Image upload with preview -->
<label for="avatar">Profile Picture:</label>
<input type="file" id="avatar" name="avatar" 
       accept="image/*" onchange="previewImage(this)">
<img id="preview" style="max-width: 200px; display: none;">
```

### Choice Controls

```html
<!-- Radio buttons (single choice) -->
<fieldset>
  <legend>Preferred Contact Method:</legend>
  
  <input type="radio" id="contact-email" name="contact_method" 
         value="email" checked>
  <label for="contact-email">Email</label>
  
  <input type="radio" id="contact-phone" name="contact_method" 
         value="phone">
  <label for="contact-phone">Phone</label>
  
  <input type="radio" id="contact-sms" name="contact_method" 
         value="sms">
  <label for="contact-sms">SMS</label>
</fieldset>

<!-- Checkboxes (multiple choices) -->
<fieldset>
  <legend>Newsletter Subscriptions:</legend>
  
  <input type="checkbox" id="tech-news" name="newsletters[]" 
         value="tech">
  <label for="tech-news">Tech News</label>
  
  <input type="checkbox" id="weekly-digest" name="newsletters[]" 
         value="digest" checked>
  <label for="weekly-digest">Weekly Digest</label>
</fieldset>

<!-- Select dropdown -->
<label for="country">Country:</label>
<select id="country" name="country" required>
  <option value="">-- Please select --</option>
  <option value="us">United States</option>
  <option value="ca">Canada</option>
  <option value="uk" selected>United Kingdom</option>
</select>

<!-- Multiple select -->
<label for="skills">Skills (hold Ctrl/Cmd):</label>
<select id="skills" name="skills[]" multiple size="4">
  <option value="html">HTML</option>
  <option value="css">CSS</option>
  <option value="js" selected>JavaScript</option>
  <option value="react">React</option>
</select>

<!-- Grouped options -->
<label for="food">Favorite Food:</label>
<select id="food" name="food">
  <optgroup label="Fruits">
    <option value="apple">Apple</option>
    <option value="banana">Banana</option>
  </optgroup>
  <optgroup label="Vegetables">
    <option value="carrot">Carrot</option>
    <option value="spinach">Spinach</option>
  </optgroup>
</select>
```

### Text Areas & Buttons

```html
<!-- Textarea with constraints -->
<label for="message">Message (10-500 chars):</label>
<textarea id="message" name="message" 
          rows="4" cols="50" 
          minlength="10" maxlength="500" 
          placeholder="Enter your message..." required></textarea>

<!-- Button types -->
<button type="submit">Submit Form</button>
<button type="reset">Clear Form</button>
<button type="button" onclick="showHelp()">Help</button>

<!-- Input buttons (older style) -->
<input type="submit" value="Submit with Input">
<input type="reset" value="Reset with Input">
<input type="button" value="Custom Action" onclick="doSomething()">

<!-- Image submit button -->
<input type="image" src="submit-button.png" alt="Submit" 
       width="100" height="30">
```

### Form Organization

```html
<!-- Fieldset grouping -->
<form action="/profile" method="post">
  <fieldset>
    <legend>Personal Information</legend>
    <label for="first-name">First Name:</label>
    <input type="text" id="first-name" name="first_name" required>
    
    <label for="last-name">Last Name:</label>
    <input type="text" id="last-name" name="last_name" required>
  </fieldset>
  
  <fieldset disabled>
    <legend>Premium Features (Upgrade Required)</legend>
    <input type="checkbox" id="analytics" name="features[]" value="analytics">
    <label for="analytics">Advanced Analytics</label>
  </fieldset>
  
  <button type="submit">Save Profile</button>
</form>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Always associate labels with inputs using `for` attribute | Leave inputs without proper labels |
| Use appropriate input types for better UX and validation | Use `type="text"` for everything |
| Group related fields with `<fieldset>` and `<legend>` | Mix unrelated form fields without organization |
| Provide clear error messages and validation feedback | Show cryptic validation errors |
| Use `required` attribute for mandatory fields | Rely only on JavaScript for required validation |
| Include helpful placeholder text and instructions | Use placeholder as replacement for labels |

## Quick Fixes

- **Form not submitting** → Check `action` attribute and ensure submit button is inside form
- **Validation not working** → Remove `novalidate` attribute or add proper `pattern`/constraints
- **Labels not clickable** → Ensure `for` attribute matches input `id` exactly
- **File upload failing** → Add `enctype="multipart/form-data"` to form element
- **Radio buttons not exclusive** → Ensure all related radio buttons have same `name` attribute
- **Screen reader issues** → Add `aria-describedby`, `aria-invalid`, and proper `role` attributes

## Gotchas

⚠️ **Label Association**: Both `for` attribute and input `id` must match exactly - case sensitive

⚠️ **File Upload Encoding**: Must use `enctype="multipart/form-data"` and `method="post"` for file uploads

⚠️ **Radio Button Grouping**: Radio buttons with same `name` are mutually exclusive - different names create separate groups

⚠️ **Required vs Validation**: `required` attribute only checks if field has value - use additional validation for format checking

⚠️ **Placeholder vs Label**: Placeholders disappear when typing and aren't accessible - always use proper labels

⚠️ **Submit Button Location**: Submit buttons must be inside the `<form>` element to trigger submission

## Checklist

- [ ] All inputs have associated labels using `for` attribute or nested structure
- [ ] Form has proper `action` and `method` attributes set
- [ ] Required fields marked with `required` attribute and visual indicators
- [ ] Appropriate input types used for better UX and built-in validation
- [ ] Error messages properly associated with inputs using `aria-describedby`
- [ ] Related form controls grouped with `<fieldset>` and `<legend>`
- [ ] File uploads use `enctype="multipart/form-data"` and `method="post"`
- [ ] Form tested with keyboard navigation and screen readers
- [ ] Validation provides clear, helpful error messages
- [ ] Form includes proper autocomplete attributes for better UX