# Modern HTML Development Tools `v 2.0.25`

## Essentials
```html
<!-- Basic dev setup with live reload -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dev Ready</title>
</head>
<body>
    <h1>Hello Dev World</h1>
    <!-- F12 to inspect, Ctrl+Shift+C to select element -->
</body>
</html>
```

**Key Concepts**: Browser DevTools | Live Reload | HTML Validation | Build Tools
**When to use**: Every HTML project needs proper tooling for efficient development

## Syntax Reference

### DevTools Shortcuts
```bash
# Chrome/Edge DevTools
F12                 # Toggle DevTools
Ctrl+Shift+I       # Open DevTools
Ctrl+Shift+C       # Element selector
Ctrl+Shift+J       # Console
Ctrl+]             # Next panel
Ctrl+[             # Previous panel

# Element inspection
Right-click → Inspect
Double-click → Edit HTML
Delete key → Remove element
Ctrl+F in Elements → Search DOM
```

### Common Patterns
```bash
# Pattern 1: Live Server Setup
npm install -g live-server
live-server --port=3000 --open

# Pattern 2: HTML Validation
npm install -g html-validate
html-validate src/**/*.html

# Pattern 3: Build Pipeline
npm run validate && npm run build && npm run serve
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Use F12 DevTools for debugging | Rely only on code editor |
| Validate HTML regularly | Skip validation until deployment |
| Set up live reload early | Manually refresh browser constantly |
| Use semantic inspector tools | Ignore accessibility warnings |
| Configure build tools properly | Write HTML without tooling support |

## Quick Fixes
- **HTML not updating** → Check live server is running and watching files
- **Validation errors** → Run `html-validate filename.html` for detailed reports
- **DevTools not opening** → Try Ctrl+Shift+I or right-click → Inspect Element
- **Build failing** → Check package.json scripts and dependencies
- **Performance issues** → Use Lighthouse audit in DevTools

## Gotchas
⚠️ **DevTools changes are temporary** - Edits in browser don't save to files
⚠️ **Live reload cache** - Hard refresh (Ctrl+F5) if changes don't appear
⚠️ **Validator differences** - W3C vs Nu Html Checker may show different results
⚠️ **Build tool conflicts** - Multiple servers on same port will cause errors

## Checklist
- [ ] Browser DevTools configured and shortcuts memorized
- [ ] Live reload server running (Live Server, Browser-sync, or http-server)
- [ ] HTML validator integrated into workflow
- [ ] Build pipeline configured for development and production
- [ ] Version control with proper .gitignore for build files