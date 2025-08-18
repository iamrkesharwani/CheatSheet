# HTML5 Storage & Offline Features `v 0.07.25.01`

## Essentials
```html
<!-- Local Storage Form Auto-save -->
<form data-auto-save="local" data-save-interval="5000">
  <input type="text" name="title" data-save-key="draft-title">
  <textarea name="content" data-save-key="draft-content"></textarea>
</form>

<!-- Geolocation -->
<button data-action="get-location" aria-describedby="location-help">
  📍 Get My Location
</button>
<output data-location-display="coordinates">Location: Not available</output>

<!-- Notifications -->
<div data-notification-permission="unknown">
  <button data-action="request-notification-permission">Enable Notifications</button>
</div>

<!-- Service Worker PWA -->
<link rel="manifest" href="/manifest.json">
<meta name="theme-color" content="#000000">
```

**Key Concepts**: Local Storage | Session Storage | Geolocation | Notifications | Service Workers | Device APIs
**When to use**: Building offline-capable web apps, PWAs, and location-aware applications

## Local Storage Patterns

### Form Auto-save Structure
```html
<!-- Auto-saving form with local storage -->
<form data-auto-save="local" data-save-interval="5000">
  <input type="hidden" name="storage-key" value="draft-form">
  <input type="text" name="title" data-save-key="draft-title" required>
  <textarea name="content" data-save-key="draft-content" required></textarea>
  
  <div class="form-actions">
    <button type="submit">Publish</button>
    <button type="button" data-action="clear-draft">Clear Draft</button>
  </div>
</form>

<!-- Storage status indicator -->
<div class="storage-status" data-storage-available="unknown">
  <span data-storage-indicator="local">Local Storage: Checking...</span>
</div>
```

### Data Storage Attributes
```html
<!-- Container with storage metadata -->
<article data-stored-content="true" data-storage-key="article-123">
  <header>
    <h1 data-field="title">Article Title</h1>
    <time data-field="created" datetime="2024-01-01">Created</time>
    <time data-field="modified" datetime="2024-01-01">Modified</time>
  </header>
  
  <div data-field="content">
    <p>Article content here...</p>
  </div>
  
  <footer>
    <button data-action="save-local">💾 Save Locally</button>
    <button data-action="sync-server">🔄 Sync to Server</button>
  </footer>
</article>
```

### Storage Error Handling
```html
<div class="storage-errors" hidden>
  <div data-error-type="quota-exceeded" class="error-message" hidden>
    <h3>Storage Full</h3>
    <p>Please clear some data to continue.</p>
    <button data-action="clear-storage">Clear Old Data</button>
  </div>
  
  <div data-error-type="storage-disabled" class="error-message" hidden>
    <h3>Storage Disabled</h3>
    <p>Please enable storage in your browser settings.</p>
  </div>
</div>
```

## Session Storage Patterns

### Multi-step Form Wizard
```html
<form data-step="1" data-session-form="wizard">
  <fieldset data-step-number="1">
    <legend>Step 1: Basic Info</legend>
    <input type="text" name="firstName" required>
    <input type="text" name="lastName" required>
  </fieldset>
  
  <fieldset data-step-number="2" hidden>
    <legend>Step 2: Contact Info</legend>
    <input type="email" name="email" required>
    <input type="tel" name="phone">
  </fieldset>
  
  <div class="form-navigation">
    <button type="button" data-action="prev-step" hidden>Previous</button>
    <button type="button" data-action="next-step">Next</button>
    <button type="submit" hidden>Submit</button>
  </div>
</form>
```

### Session State Management
```html
<div data-session-id="" data-session-active="false">
  <h2>Session Information</h2>
  <p>Session ID: <span data-session-display="id">Not started</span></p>
  <p>Duration: <span data-session-display="duration">0:00</span></p>
  <button data-action="start-session">Start Session</button>
</div>
```

## Geolocation API

### Location-Aware Forms
```html
<form data-requires-location="true">
  <fieldset>
    <legend>Location Information</legend>
    
    <!-- Hidden location fields -->
    <input type="hidden" name="latitude" data-geo-field="lat">
    <input type="hidden" name="longitude" data-geo-field="lng">
    <input type="hidden" name="accuracy" data-geo-field="accuracy">
    
    <!-- Location display -->
    <output data-location-display="coordinates">
      Location: Not available
    </output>
    
    <button type="button" data-action="get-location">
      📍 Get My Location
    </button>
  </fieldset>
</form>
```

### Location Status Indicators
```html
<div class="location-status" data-geo-status="unknown">
  <div data-geo-state="loading" hidden>
    <p>📍 Getting your location...</p>
    <progress></progress>
  </div>
  
  <div data-geo-state="success" hidden>
    <p>✅ Location found</p>
    <dl>
      <dt>Latitude:</dt>
      <dd data-geo-value="latitude">-</dd>
      <dt>Longitude:</dt>
      <dd data-geo-value="longitude">-</dd>
      <dt>Accuracy:</dt>
      <dd data-geo-value="accuracy">-</dd>
    </dl>
  </div>
  
  <div data-geo-state="error" hidden>
    <p>❌ Location unavailable</p>
    <p data-geo-error-message>Error getting location</p>
  </div>
</div>
```

### Privacy and Permissions
```html
<div class="location-privacy">
  <details>
    <summary>Why do we need your location?</summary>
    <ul>
      <li>Show nearby services</li>
      <li>Provide relevant local content</li>
      <li>Improve search results</li>
    </ul>
  </details>
  
  <div data-permission-state="unknown">
    <p data-permission-message>Location permission status unknown</p>
    <button data-action="request-permission">Allow Location Access</button>
  </div>
</div>
```

## Web Notifications

### Notification Permission Setup
```html
<div class="notification-settings">
  <h2>Notification Preferences</h2>
  
  <div data-notification-support="unknown">
    <p data-support-message>Checking notification support...</p>
  </div>
  
  <div data-notification-permission="unknown">
    <p>Permission: <span data-permission-text>Unknown</span></p>
    <button data-action="request-notification-permission">
      🔔 Enable Notifications
    </button>
  </div>
</div>
```

### Notification Controls
```html
<fieldset data-notification-controls="true" disabled>
  <legend>Notification Types</legend>
  
  <label>
    <input type="checkbox" name="notifications[]" value="messages" data-notify-type="messages">
    New Messages
  </label>
  
  <label>
    <input type="checkbox" name="notifications[]" value="updates" data-notify-type="updates">
    App Updates
  </label>
  
  <label>
    <input type="checkbox" name="notifications[]" value="reminders" data-notify-type="reminders">
    Reminders
  </label>
</fieldset>
```

### Permission States Display
```html
<div class="permission-states">
  <div data-permission-state="default" hidden>
    <h4>🔔 Enable Notifications</h4>
    <p>Get notified about important updates</p>
    <button data-action="request-permission">Allow Notifications</button>
  </div>
  
  <div data-permission-state="granted" hidden>
    <h4>✅ Notifications Enabled</h4>
    <button data-action="test-notification">Send Test</button>
  </div>
  
  <div data-permission-state="denied" hidden>
    <h4>❌ Notifications Blocked</h4>
    <details>
      <summary>How to enable notifications</summary>
      <ol>
        <li>Click the lock icon in your address bar</li>
        <li>Change notifications to "Allow"</li>
        <li>Refresh the page</li>
      </ol>
    </details>
  </div>
</div>
```

## Service Workers & PWA

### PWA HTML Structure
```html
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="manifest" href="/manifest.json">
  <meta name="theme-color" content="#000000">
  
  <!-- iOS specific -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="apple-mobile-web-app-title" content="App Name">
  <link rel="apple-touch-icon" href="/icon-192.png">
</head>
<body>
  <!-- App shell structure -->
  <header data-app-shell="true">
    <h1>App Title</h1>
  </header>
  
  <main data-dynamic-content="true">
    <!-- Content loaded dynamically -->
  </main>
  
  <footer data-app-shell="true">
    <p>Footer content</p>
  </footer>
</body>
</html>
```

### Offline Status Indicators
```html
<div class="connection-status">
  <div data-online-indicator class="online-status">
    <span>🟢 Online</span>
  </div>
  <div data-offline-indicator class="offline-status" hidden>
    <span>🔴 Offline - Changes saved locally</span>
  </div>
</div>

<!-- Service worker status -->
<div data-sw-status="unknown" hidden>
  <p>Service Worker: <span data-sw-state>Unknown</span></p>
</div>
```

## Device Orientation

### Orientation-Aware Content
```html
<div class="orientation-container" data-orientation="unknown">
  <div data-orientation-state="portrait" hidden>
    <h2>Portrait Mode Content</h2>
    <p>Optimized for vertical viewing</p>
  </div>
  
  <div data-orientation-state="landscape" hidden>
    <h2>Landscape Mode Content</h2>
    <p>Optimized for horizontal viewing</p>
  </div>
</div>
```

### Motion Data Display
```html
<section data-device-motion="true">
  <h2>Device Motion Data</h2>
  
  <div class="motion-display">
    <div data-motion-type="acceleration">
      <h3>Acceleration</h3>
      <dl>
        <dt>X:</dt><dd data-accel-x>0</dd>
        <dt>Y:</dt><dd data-accel-y>0</dd>
        <dt>Z:</dt><dd data-accel-z>0</dd>
      </dl>
    </div>
  </div>
</section>
```

## Data Attributes Reference

### Storage Attributes
```html
<!-- Local Storage -->
<form data-auto-save="local" data-save-interval="5000">
<input data-save-key="user-input" type="text">
<div data-storage-available="unknown">

<!-- Session Storage -->
<form data-session-storage="true">
<div data-session-id="" data-session-active="false">
```

### Geolocation Attributes
```html
<!-- Location Data -->
<div data-geo-enabled="false">
<input data-geo-field="latitude" type="hidden">
<button data-geo-action="getCurrentPosition">
<span data-geo-display="coordinates">

<!-- Location States -->
<div data-geo-status="unknown">
<div data-geo-state="loading">
<output data-location-display="coordinates">
```

### Notification Attributes
```html
<!-- Permission & Controls -->
<div data-notification-permission="unknown">
<button data-notify-action="request">
<fieldset data-notification-controls="true">
<div data-permission-state="granted">
```

### Device Attributes
```html
<!-- Orientation -->
<div data-orientation="portrait">
<section data-motion-sensitive="true">
<canvas data-orientation-canvas="true">

<!-- PWA -->
<header data-app-shell="true">
<main data-dynamic-content="true">
<div data-sw-status="unknown">
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Check storage availability before using | Assume localStorage is always available |
| Request permissions with clear explanations | Request permissions without context |
| Provide fallbacks for denied permissions | Break functionality when permissions denied |
| Store sensitive data securely | Store passwords in localStorage |
| Handle quota exceeded errors gracefully | Ignore storage errors |
| Use HTTPS for location and notifications | Use these APIs over HTTP |

## Quick Fixes
- **LocalStorage not working** → Check if private/incognito mode is enabled
- **Geolocation permission denied** → Provide manual location input fallback
- **Notifications not showing** → Check permission status and browser support
- **Storage quota exceeded** → Implement automatic cleanup of old data
- **Service worker not updating** → Check cache versioning and update strategies
- **Device orientation not detected** → Ensure HTTPS and check browser support

## Gotchas
⚠️ **Storage limits**: 5-10MB per origin for localStorage/sessionStorage - implement quota management  
⚠️ **Private browsing**: Storage APIs may be disabled or limited in private/incognito mode  
⚠️ **HTTPS required**: Geolocation, notifications, and service workers require secure contexts  
⚠️ **Permission persistence**: User can revoke permissions at any time - always check status  
⚠️ **Cross-origin restrictions**: Storage is isolated per origin (protocol + domain + port)  
⚠️ **Mobile permissions**: iOS Safari has different permission models than other browsers

## Accessibility Considerations
```html
<!-- ARIA labels for device features -->
<button 
  data-action="get-location" 
  aria-describedby="location-help"
  aria-pressed="false">
  Get My Location
</button>

<div id="location-help" class="sr-only">
  This will request your current location to show nearby results
</div>

<!-- Live regions for status updates -->
<div role="alert" data-geo-error="true" aria-live="polite" hidden>
  <p data-error-message>Location error will appear here</p>
</div>

<div role="status" data-notification-status="true" aria-live="polite">
  <p data-notify-message>Notification status updates</p>
</div>
```

## Checklist
- [ ] Check browser support for required APIs before using
- [ ] Implement proper error handling for all storage operations
- [ ] Request permissions with clear user explanations
- [ ] Provide fallbacks for denied permissions or unsupported features
- [ ] Use HTTPS for geolocation, notifications, and service workers
- [ ] Test storage quota limits and implement cleanup strategies
- [ ] Include proper ARIA labels and live regions for accessibility
- [ ] Handle offline scenarios gracefully with appropriate user feedback
- [ ] Implement data validation and sanitization for stored content
- [ ] Test across different browsers and private browsing modes