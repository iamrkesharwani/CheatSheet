# Browser Storage & JSON `v 0.07.25.01`

## Essentials
```javascript
// localStorage - persists until explicitly removed
localStorage.setItem('username', 'john_doe');
localStorage.setItem('settings', JSON.stringify({theme: 'dark'}));

// sessionStorage - cleared when tab closes
sessionStorage.setItem('tempData', 'value');

// Retrieving data
const username = localStorage.getItem('username');
const settings = JSON.parse(localStorage.getItem('settings') || '{}');

// JSON operations
const obj = {name: 'John', age: 30};
const jsonString = JSON.stringify(obj);
const parsedObj = JSON.parse(jsonString);
```

**Key Concepts**: localStorage | sessionStorage | JSON Serialization | Storage Events | IndexedDB
**When to use**: Persisting user preferences, form data, app state, and complex data structures

## Syntax Reference

### localStorage & sessionStorage
```javascript
// Basic operations (both work identically)
localStorage.setItem('key', 'value');
const value = localStorage.getItem('key');        // Returns string or null
localStorage.removeItem('key');
localStorage.clear();                             // Remove all items

// Checking existence
if (localStorage.getItem('username') !== null) {
    console.log('Username exists');
}

// Iteration
for (let i = 0; i < localStorage.length; i++) {
    const key = localStorage.key(i);
    const value = localStorage.getItem(key);
}

// Storage limits (5-10MB per origin)
try {
    localStorage.setItem('key', 'value');
} catch (e) {
    if (e.name === 'QuotaExceededError') {
        console.log('Storage quota exceeded');
    }
}
```

### JSON Operations
```javascript
// JSON.stringify() - Object to string
const obj = {name: 'John', hobbies: ['reading', 'gaming']};
const jsonString = JSON.stringify(obj);

// With replacer function
const filtered = JSON.stringify(obj, (key, value) => 
    key === 'age' ? undefined : value
);

// With spacing (pretty print)
const pretty = JSON.stringify(obj, null, 2);

// JSON.parse() - String to object
const parsedObj = JSON.parse(jsonString);

// With reviver function
const modified = JSON.parse(jsonString, (key, value) => 
    key === 'age' ? value * 2 : value
);
```

### Common Patterns
```javascript
// Pattern 1: Safe storage with defaults
function getStoredValue(key, defaultValue = null) {
    const stored = localStorage.getItem(key);
    return stored !== null ? JSON.parse(stored) : defaultValue;
}

function setStoredValue(key, value) {
    localStorage.setItem(key, JSON.stringify(value));
}

// Pattern 2: Auto-save form data
function autoSaveForm(formId) {
    const form = document.getElementById(formId);
    const inputs = form.querySelectorAll('input, select, textarea');
    
    inputs.forEach(input => {
        // Load saved value
        const saved = localStorage.getItem(`form_${input.name}`);
        if (saved !== null) input.value = saved;
        
        // Save on change
        input.addEventListener('input', () => {
            localStorage.setItem(`form_${input.name}`, input.value);
        });
    });
}

// Pattern 3: State management with versioning
const stateManager = {
    version: '1.0.0',
    
    save(key, data) {
        const state = {
            version: this.version,
            timestamp: Date.now(),
            data: data
        };
        localStorage.setItem(key, JSON.stringify(state));
    },
    
    load(key) {
        const stored = localStorage.getItem(key);
        if (!stored) return null;
        
        const state = JSON.parse(stored);
        if (state.version !== this.version) {
            return this.migrate(state);
        }
        return state.data;
    }
};
```

## Storage Events & Cross-Tab Communication
```javascript
// Listen for storage changes (only fires in OTHER tabs)
window.addEventListener('storage', (e) => {
    console.log('Key:', e.key);
    console.log('Old value:', e.oldValue);
    console.log('New value:', e.newValue);
});

// Cross-tab messaging
function broadcastMessage(type, data) {
    const message = {type, data, timestamp: Date.now()};
    localStorage.setItem('broadcast', JSON.stringify(message));
    localStorage.removeItem('broadcast'); // Triggers storage event
}

// Handle broadcast messages
window.addEventListener('storage', (e) => {
    if (e.key === 'broadcast' && e.newValue) {
        const message = JSON.parse(e.newValue);
        handleMessage(message);
    }
});
```

## IndexedDB Basics
```javascript
// Opening connection
let db;
const request = indexedDB.open('MyDatabase', 1);

request.onsuccess = (event) => {
    db = event.target.result;
};

request.onupgradeneeded = (event) => {
    db = event.target.result;
    const store = db.createObjectStore('customers', {
        keyPath: 'id',
        autoIncrement: true
    });
    store.createIndex('email', 'email', {unique: true});
};

// CRUD operations
function addRecord(data) {
    const transaction = db.transaction(['customers'], 'readwrite');
    const store = transaction.objectStore('customers');
    return store.add(data);
}

function getRecord(id) {
    const transaction = db.transaction(['customers'], 'readonly');
    const store = transaction.objectStore('customers');
    return store.get(id);
}
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Always stringify objects with JSON.stringify | Store objects directly in localStorage |
| Handle JSON.parse errors with try/catch | Assume stored data is always valid JSON |
| Use sessionStorage for temporary data | Use localStorage for sensitive data |
| Check for null before parsing | Parse without null checks |
| Set storage quota error handlers | Ignore storage capacity limits |
| Use IndexedDB for complex/large data | Use localStorage for large datasets |

## Quick Fixes
- **"Unexpected token" in JSON.parse** → Wrap in try/catch and provide fallback
- **Storage quota exceeded** → Implement cleanup strategy or use IndexedDB
- **Data not persisting** → Check if using sessionStorage instead of localStorage
- **Cross-tab updates not working** → Ensure using storage event listeners
- **Form data lost on refresh** → Implement auto-save with input event listeners
- **Circular reference error** → Remove circular references or use custom replacer

## Gotchas
⚠️ **Storage only stores strings**: All values are converted to strings, use JSON for objects
⚠️ **Storage events don't fire in same tab**: Only triggers in other tabs/windows
⚠️ **sessionStorage scope**: Limited to single tab, not shared across tabs
⚠️ **JSON.stringify limitations**: Functions, undefined, and symbols are ignored
⚠️ **Private browsing**: Storage may be disabled or cleared aggressively
⚠️ **Storage capacity**: 5-10MB limit varies by browser and can be exceeded

## Real-World Applications

### Shopping Cart Persistence
```javascript
const cart = {
    items: [],
    total: 0,
    
    add(item) {
        this.items.push(item);
        this.total += item.price;
        this.save();
    },
    
    save() {
        localStorage.setItem('cart', JSON.stringify({
            items: this.items,
            total: this.total
        }));
    },
    
    load() {
        const saved = localStorage.getItem('cart');
        if (saved) {
            const data = JSON.parse(saved);
            this.items = data.items || [];
            this.total = data.total || 0;
        }
    }
};
```

### User Preferences System
```javascript
const preferences = {
    theme: 'light',
    fontSize: 14,
    notifications: true,
    
    save() {
        localStorage.setItem('userPrefs', JSON.stringify(this));
    },
    
    load() {
        const saved = localStorage.getItem('userPrefs');
        if (saved) {
            Object.assign(this, JSON.parse(saved));
        }
        return this;
    },
    
    reset() {
        localStorage.removeItem('userPrefs');
        this.theme = 'light';
        this.fontSize = 14;
        this.notifications = true;
    }
};
```

## Checklist
- [ ] Handle storage quota exceeded errors
- [ ] Implement proper JSON error handling
- [ ] Choose appropriate storage type (local vs session)
- [ ] Set up storage event listeners for cross-tab sync
- [ ] Consider IndexedDB for complex data structures
- [ ] Implement data migration for version changes
- [ ] Test storage behavior in private browsing mode