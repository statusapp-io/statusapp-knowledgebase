---
title: Embedding Status Pages
slug: embedding-status-pages
category: Status Pages
excerpt: Embed your status page widget directly on your website for instant visibility.
order: 5
---

# Embedding Status Pages

## Overview

Embed your StatusApp status page directly on your website without redirecting users. Visitors see real-time status information without leaving your site.

## Embedding Methods

### Method 1: Widget (Recommended)

**Best For**: Adding status widget to your website

**Benefits**:
- Minimal code snippet
- Auto-updates in real-time
- Responsive design
- Matches website styling options
- No iframe hosting needed

**Implementation**: 1 line of code

### Method 2: IFrame

**Best For**: Embedding full status page

**Benefits**:
- Complete status page in iframe
- Isolated from website code
- Full functionality

**Implementation**: Standard HTML iframe

## Getting Embed Code

1. Go to **Settings → Status Pages**
2. Click the status page to edit
3. Scroll to **Embed Options**
4. Choose **Widget** or **IFrame**
5. Copy the provided code
6. Paste into your website

## Widget Embed

### Basic Widget

Minimal code to add status widget to your page:

```html
<script src="https://statusapp.io/embed/widget.js"></script>
<div id="statusapp-widget" data-page="your-slug"></div>
```

Replace `your-slug` with your status page slug (example: `acme-status`).

### Widget Styling

Customize widget appearance with data attributes:

```html
<div id="statusapp-widget" 
     data-page="your-slug"
     data-size="small"
     data-theme="light"
     data-show-history="true">
</div>
```

**Size Options**:
- `small`: Compact widget (good for sidebars)
- `medium`: Standard widget (default)
- `large`: Full-width widget

**Theme Options**:
- `light`: Light background (default)
- `dark`: Dark background
- `auto`: Matches system preference

**Show History**:
- `true`: Shows incident history
- `false`: Only current status

### Widget Examples

**Compact Widget (Sidebar)**:
```html
<div id="statusapp-widget" 
     data-page="acme-status"
     data-size="small"
     data-theme="light">
</div>
```

**Full-Width Widget (Main Page)**:
```html
<div id="statusapp-widget" 
     data-page="acme-status"
     data-size="large"
     data-theme="auto"
     data-show-history="true">
</div>
```

**Dark Mode Widget**:
```html
<div id="statusapp-widget" 
     data-page="acme-status"
     data-theme="dark">
</div>
```

### Widget Responsiveness

Widgets automatically adapt to screen size:
- Mobile: Full width, stacked layout
- Tablet: Adjusted columns
- Desktop: Full featured display

No additional configuration needed.

## IFrame Embed

### Basic IFrame

Embed full status page in iframe:

```html
<iframe 
  src="https://statusapp.io/pages/your-slug" 
  width="100%" 
  height="800" 
  frameborder="0"
  scrolling="yes">
</iframe>
```

Replace `your-slug` with your status page slug.

### IFrame Sizing

**Responsive IFrame** (recommended):

```html
<div style="width: 100%; height: 800px; border: 1px solid #ccc;">
  <iframe 
    src="https://statusapp.io/pages/your-slug" 
    width="100%" 
    height="100%" 
    frameborder="0"
    scrolling="yes"
    style="border: none;">
  </iframe>
</div>
```

**Fixed Height**:
```html
<iframe 
  src="https://statusapp.io/pages/your-slug" 
  width="100%" 
  height="600" 
  frameborder="0">
</iframe>
```

### IFrame Advanced Options

**Auto-Height IFrame** (JavaScript):

```html
<iframe 
  id="statusapp-iframe"
  src="https://statusapp.io/pages/your-slug" 
  width="100%" 
  height="600" 
  frameborder="0"
  scrolling="no"
  style="border: none;">
</iframe>

<script>
  // Auto-adjust iframe height based on content
  const iframe = document.getElementById('statusapp-iframe');
  iframe.onload = function() {
    try {
      iframe.style.height = iframe.contentWindow.document.documentElement.scrollHeight + 'px';
    } catch(e) {
      console.log('Unable to resize iframe');
    }
  };
</script>
```

## Styling for Your Website

### Widget Styling

The widget inherits some styling from your page. To customize:

**CSS Class**: `.statusapp-widget`

```css
/* Customize widget appearance */
.statusapp-widget {
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  margin: 20px 0;
}

/* Customize widget title */
.statusapp-widget h2 {
  font-family: 'Your Font', sans-serif;
  color: #333;
}
```

### IFrame Styling

Add border/shadow to iframe:

```html
<div style="
  border: 1px solid #e0e0e0; 
  border-radius: 8px; 
  box-shadow: 0 2px 8px rgba(0,0,0,0.1); 
  overflow: hidden;">
  <iframe 
    src="https://statusapp.io/pages/your-slug" 
    width="100%" 
    height="800" 
    frameborder="0"
    style="border: none;">
  </iframe>
</div>
```

## Popular Integration Examples

### WordPress

Add to WordPress page/post using HTML block:

1. Edit page/post
2. Add **Custom HTML** block
3. Paste widget code:

```html
<script src="https://statusapp.io/embed/widget.js"></script>
<div id="statusapp-widget" data-page="your-slug" data-size="large"></div>
```

4. Publish

### HTML Website

Add to any HTML page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Your Company</title>
</head>
<body>
  <h1>Your Website</h1>
  
  <h2>Service Status</h2>
  <script src="https://statusapp.io/embed/widget.js"></script>
  <div id="statusapp-widget" data-page="your-slug"></div>
  
</body>
</html>
```

### React/Next.js

Create a component:

```jsx
export default function StatusWidget() {
  useEffect(() => {
    // Load StatusApp widget script
    const script = document.createElement('script');
    script.src = 'https://statusapp.io/embed/widget.js';
    document.body.appendChild(script);
  }, []);

  return (
    <div>
      <h2>Service Status</h2>
      <div id="statusapp-widget" data-page="your-slug"></div>
    </div>
  );
}
```

### Vue.js

```vue
<template>
  <div>
    <h2>Service Status</h2>
    <div id="statusapp-widget" data-page="your-slug"></div>
  </div>
</template>

<script>
export default {
  mounted() {
    // Load StatusApp widget script
    const script = document.createElement('script');
    script.src = 'https://statusapp.io/embed/widget.js';
    document.body.appendChild(script);
  }
}
</script>
```

### Static Site Generator (Hugo, Jekyll, etc.)

Add to your template:

```html
<!-- layouts/partials/status-widget.html -->
<script src="https://statusapp.io/embed/widget.js"></script>
<div id="statusapp-widget" data-page="your-slug"></div>
```

Include in your layouts:
```html
{{ partial "status-widget.html" . }}
```

### Help Center / Knowledge Base

Add status widget to your support documentation:

```html
<!-- At top of knowledge base -->
<h2>Current Status</h2>
<script src="https://statusapp.io/embed/widget.js"></script>
<div id="statusapp-widget" 
     data-page="your-slug" 
     data-size="small"
     data-theme="light">
</div>

<!-- Rest of documentation -->
```

## Widget Customization

### Display Options

**Show All Information**:
```html
<div id="statusapp-widget" 
     data-page="your-slug"
     data-show-title="true"
     data-show-description="true"
     data-show-history="true"
     data-show-uptime="true">
</div>
```

**Minimal Widget**:
```html
<div id="statusapp-widget" 
     data-page="your-slug"
     data-show-title="false"
     data-show-history="false">
</div>
```

### Color Scheme

The widget respects your status page's brand color:

1. Edit status page in StatusApp
2. Set brand color
3. Widget automatically uses that color

## Placement Ideas

### Homepage Banner

Add status banner at top of homepage:

```html
<div style="background: #f5f5f5; padding: 20px; margin-bottom: 20px;">
  <h2>Service Status</h2>
  <script src="https://statusapp.io/embed/widget.js"></script>
  <div id="statusapp-widget" data-page="your-slug" data-size="small"></div>
</div>
```

### Sidebar Widget

Add small status widget to sidebar:

```html
<aside>
  <h3>Status</h3>
  <script src="https://statusapp.io/embed/widget.js"></script>
  <div id="statusapp-widget" 
       data-page="your-slug" 
       data-size="small">
  </div>
</aside>
```

### Dashboard Panel

Include in customer dashboard:

```html
<div class="dashboard-panel">
  <h3>Platform Status</h3>
  <script src="https://statusapp.io/embed/widget.js"></script>
  <div id="statusapp-widget" 
       data-page="your-slug" 
       data-theme="light">
  </div>
</div>
```

### Help Center

Add to support/help section:

```html
<h2>Having Issues?</h2>
<p>Check the current status of our services:</p>
<script src="https://statusapp.io/embed/widget.js"></script>
<div id="statusapp-widget" data-page="your-slug"></div>
```

### Footer

Small status indicator in footer:

```html
<footer>
  <div style="margin-bottom: 20px;">
    <script src="https://statusapp.io/embed/widget.js"></script>
    <div id="statusapp-widget" 
         data-page="your-slug" 
         data-size="small"
         data-show-history="false">
    </div>
  </div>
  <p>&copy; 2024 Your Company</p>
</footer>
```

## Embedding Best Practices

### 1. Choose Right Placement

```
Good: Homepage, help center, dashboard
Bad: Buried deep in settings
Bad: Only in footer (hard to find)

Make status visible to users who need it.
```

### 2. Keep It Updated

```
Status page must be actively maintained:
- Create incidents when services down
- Update regularly during outages
- Close incidents when resolved

Don't embed status page without maintaining it.
```

### 3. Test Responsiveness

```
1. Embed code on your site
2. Test on desktop browser
3. Test on tablet
4. Test on mobile phone
5. Test on different screen sizes
6. Verify nothing breaks
```

### 4. Choose Appropriate Size

```
Desktop: medium or large
Sidebar: small
Mobile: auto-responsive (choose small)
```

### 5. Coordinate Styling

```
Match your website's design:
- Use same theme (light/dark)
- Match color scheme
- Use same fonts if possible
- Keep borders and spacing consistent
```

## Performance

### Widget Loading

The widget loads asynchronously:
- Doesn't block page rendering
- Updates in background
- Minimal performance impact
- Loads once, reuses script

### IFrame Performance

IFrames have minimal impact:
- Content isolated from main page
- No CSS conflicts
- Separate scrolling
- Slight memory overhead

**Recommendation**: Use widget for better performance.

## Troubleshooting

### Widget Not Appearing

**Check Script Tag**:
```html
<!-- Verify script loads -->
<script src="https://statusapp.io/embed/widget.js"></script>

<!-- Script must be BEFORE the div -->
<div id="statusapp-widget" data-page="your-slug"></div>
```

**Verify Page Slug**:
- Use correct status page slug
- Example: `acme-status` not `Acme Status`
- Check in StatusApp settings

**Check Browser Console**:
- Open browser DevTools (F12)
- Check Console tab for errors
- Report errors to support

### Widget Shows Old Data

**Hard Refresh**:
- Press Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
- Clear browser cache
- Try incognito window

**Check Status Page**:
- Verify status page is still public
- Check page wasn't deleted
- Verify monitors are active

### IFrame Not Showing

**Check CORS**:
- IFrame shouldn't have CORS issues
- StatusApp allows embedding

**Verify URL**:
- Use: `https://statusapp.io/pages/your-slug`
- Not: `https://status.yourdomain.com`
- Check slug is correct

**Check Dimensions**:
- Width: 100% or specific pixel value
- Height: Should be tall enough (600+ pixels)
- Verify not hidden by CSS

### Styling Looks Wrong

**Check Theme**:
- Verify theme matches: light, dark, or auto
- Try switching themes to see difference

**Check Brand Color**:
- Edit status page to set brand color
- Widget should update automatically

**Check CSS Conflicts**:
- Your website CSS might override widget CSS
- Use browser DevTools to inspect
- Adjust CSS specificity if needed

## Advanced: API Access

Get status data via API for custom integration:

```javascript
fetch('https://statusapp.io/api/v1/pages/your-slug')
  .then(r => r.json())
  .then(data => {
    // Use status data
    console.log(data.monitors);
  });
```

See API documentation for full details.

## Plan Limits

Embedding available on:
- **Free**: ✗ Widget only (limited)
- **Starter**: ✓ Full widget
- **Pro**: ✓ Widget + IFrame
- **Business**: ✓ Widget + IFrame
- **Enterprise**: ✓ Widget + IFrame + API

## Next Steps

- **[Creating Status Pages](/help/knowledge-base/creating-status-pages)** - Set up your status page
- **[Custom Domains](/help/knowledge-base/status-pages/custom-domains)** - Use your own domain
- **[Incidents](/help/knowledge-base/incidents/incident-management-basics)** - Manage status page incidents
