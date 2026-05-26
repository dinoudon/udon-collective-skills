---
name: accessibility-audit
description: WCAG 2.2 compliance audit - screen readers, keyboard navigation, ARIA, color contrast, and automated testing. Make your application accessible to all users.
tags: [accessibility, wcag, a11y, screen-reader, aria, inclusive-design]
version: 1.0.0
author: Enhanced from WCAG 2.2 guidelines)
---

# Accessibility Audit

## Overview

Make your application accessible to everyone, including users with disabilities.

**Why accessibility matters:**
- **Legal:** ADA, Section 508, WCAG compliance required
- **Ethical:** 15% of world population has disabilities
- **Business:** Accessible sites have better SEO, UX, conversion
- **Technical:** Good accessibility = good code quality

**Key principles (POUR):**
- **Perceivable:** Users can perceive the content
- **Operable:** Users can operate the interface
- **Understandable:** Users can understand the content
- **Robust:** Content works across technologies

## When to Use

**Use for:**
- New feature launches
- Website redesigns
- Legal compliance audits
- User complaints about accessibility
- Before public launch

**Critical for:**
- Government websites (Section 508)
- E-commerce (ADA compliance)
- Educational platforms
- Healthcare applications
- Financial services

## WCAG 2.2 Conformance Levels

```markdown
## Level A (Minimum)
**Must have** - Basic accessibility
- Text alternatives for images
- Keyboard accessible
- Sufficient color contrast
- No seizure-inducing content

## Level AA (Recommended)
**Should have** - Enhanced accessibility
- 4.5:1 color contrast ratio
- Resizable text (200%)
- Multiple ways to navigate
- Consistent navigation

## Level AAA (Optimal)
**Nice to have** - Maximum accessibility
- 7:1 color contrast ratio
- Sign language for videos
- Extended audio descriptions
- No time limits

**Target:** Level AA compliance (industry standard)
```

## The 7-Category Audit Framework

```
┌─────────────────────────────────────────────────────────┐
│           ACCESSIBILITY AUDIT FRAMEWORK                  │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │  CATEGORY 1  │  Keyboard Navigation
    │   KEYBOARD   │  - Tab order
    │              │  - Focus indicators
    └──────┬───────┘  - Shortcuts
           │
           ▼
    ┌──────────────┐
    │  CATEGORY 2  │  Screen Readers
    │ SCREEN READER│  - Alt text
    │              │  - ARIA labels
    └──────┬───────┘  - Semantic HTML
           │
           ▼
    ┌──────────────┐
    │  CATEGORY 3  │  Visual Design
    │    VISUAL    │  - Color contrast
    │              │  - Text size
    └──────┬───────┘  - Focus indicators
           │
           ▼
    ┌──────────────┐
    │  CATEGORY 4  │  Forms & Inputs
    │    FORMS     │  - Labels
    │              │  - Error messages
    └──────┬───────┘  - Validation
           │
           ▼
    ┌──────────────┐
    │  CATEGORY 5  │  Multimedia
    │  MULTIMEDIA  │  - Captions
    │              │  - Transcripts
    └──────┬───────┘  - Audio descriptions
           │
           ▼
    ┌──────────────┐
    │  CATEGORY 6  │  Dynamic Content
    │   DYNAMIC    │  - Live regions
    │              │  - Loading states
    └──────┬───────┘  - Modals
           │
           ▼
    ┌──────────────┐
    │  CATEGORY 7  │  Mobile & Touch
    │    MOBILE    │  - Touch targets
    │              │  - Gestures
    └──────────────┘  - Orientation
```

## Category 1: Keyboard Navigation

### Goal: All functionality accessible via keyboard

**Keyboard Navigation Checklist:**

```markdown
## Tab Order

- [ ] Logical tab order (left-to-right, top-to-bottom)
- [ ] No keyboard traps (can tab in and out)
- [ ] Skip links for main content
- [ ] Modal dialogs trap focus

**Test:**
```bash
# Navigate entire page using only keyboard
# Tab: Move forward
# Shift+Tab: Move backward
# Enter: Activate links/buttons
# Space: Activate buttons, checkboxes
# Arrow keys: Radio buttons, dropdowns
# Esc: Close modals
```

## Focus Indicators

- [ ] Visible focus indicator (outline, border, shadow)
- [ ] Sufficient contrast (3:1 ratio)
- [ ] Not removed with CSS (outline: none)
- [ ] Custom focus styles if default removed

**Good focus indicator:**
```css
/* Visible, high-contrast focus */
button:focus {
  outline: 3px solid #0066cc;
  outline-offset: 2px;
}

/* Or custom style */
button:focus-visible {
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.5);
}
```

**Bad focus indicator:**
```css
/* Never do this without replacement */
button:focus {
  outline: none; /* ❌ Removes focus indicator */
}
```

## Keyboard Shortcuts

- [ ] Document keyboard shortcuts
- [ ] Avoid conflicts with browser/screen reader shortcuts
- [ ] Provide way to disable custom shortcuts

**Example:**
```javascript
// Keyboard shortcut with documentation
document.addEventListener('keydown', (e) => {
  // Ctrl+K: Open search
  if (e.ctrlKey && e.key === 'k') {
    e.preventDefault();
    openSearch();
  }
});
```

## Skip Links

- [ ] "Skip to main content" link at top
- [ ] Visible on focus
- [ ] Links to main content area

**Implementation:**
```html
<body>
  <a href="#main-content" class="skip-link">
    Skip to main content
  </a>
  
  <nav>...</nav>
  
  <main id="main-content">
    <!-- Main content -->
  </main>
</body>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
</style>
```
```

---

## Category 2: Screen Readers

### Goal: Content understandable via screen reader

**Screen Reader Checklist:**

```markdown
## Semantic HTML

- [ ] Use semantic elements (header, nav, main, footer, article, section)
- [ ] Headings in logical order (h1 → h2 → h3, no skipping)
- [ ] Lists for list content (ul, ol)
- [ ] Tables for tabular data (with th, caption)

**Good semantic HTML:**
```html
<header>
  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>

<main>
  <article>
    <h1>Article Title</h1>
    <p>Content...</p>
  </article>
</main>

<footer>
  <p>&copy; 2026 Company</p>
</footer>
```

**Bad non-semantic HTML:**
```html
<!-- ❌ Divs for everything -->
<div class="header">
  <div class="nav">
    <div class="link">Home</div>
    <div class="link">About</div>
  </div>
</div>

<div class="content">
  <div class="title">Article Title</div>
  <div class="text">Content...</div>
</div>
```

## Alt Text for Images

- [ ] All images have alt attribute
- [ ] Decorative images: alt=""
- [ ] Informative images: descriptive alt text
- [ ] Complex images: detailed description

**Examples:**
```html
<!-- Informative image -->
<img src="chart.png" alt="Sales increased 50% from Q1 to Q2 2026" />

<!-- Decorative image -->
<img src="divider.png" alt="" />

<!-- Complex image (chart, diagram) -->
<img src="complex-chart.png" alt="Detailed sales data" 
     aria-describedby="chart-description" />
<div id="chart-description">
  Sales data for 2026: Q1 $100K, Q2 $150K, Q3 $200K, Q4 $250K.
  Overall growth of 150% year-over-year.
</div>

<!-- Linked image -->
<a href="/products">
  <img src="products.png" alt="View our products" />
</a>
```

## ARIA Labels

- [ ] Use ARIA labels for non-text content
- [ ] aria-label for short descriptions
- [ ] aria-labelledby for existing text
- [ ] aria-describedby for detailed descriptions

**Examples:**
```html
<!-- Button with icon only -->
<button aria-label="Close dialog">
  <svg>...</svg>
</button>

<!-- Form field with visible label -->
<label id="email-label" for="email">Email</label>
<input id="email" type="email" aria-labelledby="email-label" />

<!-- Complex widget -->
<div role="slider" 
     aria-label="Volume" 
     aria-valuemin="0" 
     aria-valuemax="100" 
     aria-valuenow="50">
</div>
```

## ARIA Roles

- [ ] Use semantic HTML first (better than ARIA)
- [ ] Add ARIA roles when semantic HTML insufficient
- [ ] Common roles: button, navigation, search, alert

**Examples:**
```html
<!-- Semantic HTML (preferred) -->
<button>Click me</button>
<nav>...</nav>

<!-- ARIA roles (when semantic HTML not available) -->
<div role="button" tabindex="0" 
     onclick="handleClick()" 
     onkeypress="handleKeyPress(event)">
  Click me
</div>

<div role="navigation">...</div>
```

## Live Regions

- [ ] Use aria-live for dynamic content
- [ ] aria-live="polite" for non-urgent updates
- [ ] aria-live="assertive" for urgent updates
- [ ] aria-atomic for complete announcements

**Examples:**
```html
<!-- Status message -->
<div role="status" aria-live="polite">
  Item added to cart
</div>

<!-- Error message -->
<div role="alert" aria-live="assertive">
  Error: Payment failed
</div>

<!-- Loading state -->
<div aria-live="polite" aria-busy="true">
  Loading...
</div>
```
```

---

## Category 3: Visual Design

### Goal: Content visible and readable

**Visual Design Checklist:**

```markdown
## Color Contrast

- [ ] Text: 4.5:1 contrast ratio (AA)
- [ ] Large text (18pt+): 3:1 contrast ratio (AA)
- [ ] UI components: 3:1 contrast ratio
- [ ] Don't rely on color alone

**Check contrast:**
```bash
# Use browser DevTools or online tools
# Chrome DevTools → Elements → Accessibility pane
# Shows contrast ratio for selected text

# Online tools:
# - WebAIM Contrast Checker
# - Contrast Ratio by Lea Verou
```

**Good contrast:**
```css
/* 7:1 ratio (AAA) */
.text {
  color: #000000; /* Black */
  background: #ffffff; /* White */
}

/* 4.5:1 ratio (AA) */
.text {
  color: #595959; /* Dark gray */
  background: #ffffff; /* White */
}
```

**Bad contrast:**
```css
/* 2:1 ratio (fails) */
.text {
  color: #cccccc; /* Light gray */
  background: #ffffff; /* White */
}
```

## Text Size & Spacing

- [ ] Base font size: 16px minimum
- [ ] Text resizable to 200% without loss of content
- [ ] Line height: 1.5 minimum
- [ ] Paragraph spacing: 2x font size
- [ ] Letter spacing: 0.12x font size

**Good typography:**
```css
body {
  font-size: 16px;
  line-height: 1.5;
  letter-spacing: 0.02em;
}

p {
  margin-bottom: 1.5em;
}

h1 {
  font-size: 2rem;
  line-height: 1.2;
  margin-bottom: 0.5em;
}
```

## Focus Indicators

- [ ] Visible focus indicator (3:1 contrast)
- [ ] Minimum 2px thick
- [ ] Not obscured by other elements

**Implementation:**
```css
/* High-contrast focus indicator */
*:focus-visible {
  outline: 3px solid #0066cc;
  outline-offset: 2px;
}

/* Custom focus for buttons */
button:focus-visible {
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.5);
  outline: none;
}
```

## Color Independence

- [ ] Don't use color alone to convey information
- [ ] Add icons, patterns, or text labels

**Good (color + icon):**
```html
<button class="success">
  <svg aria-hidden="true">✓</svg>
  Success
</button>

<button class="error">
  <svg aria-hidden="true">✗</svg>
  Error
</button>
```

**Bad (color only):**
```html
<button class="green">Submit</button>
<button class="red">Cancel</button>
```
```

---

## Category 4: Forms & Inputs

### Goal: Forms accessible and understandable

**Forms Checklist:**

```markdown
## Labels

- [ ] All inputs have associated labels
- [ ] Use <label> element with for attribute
- [ ] Label visible (not placeholder only)
- [ ] Required fields marked

**Good form:**
```html
<form>
  <div>
    <label for="name">
      Name <span aria-label="required">*</span>
    </label>
    <input id="name" type="text" required aria-required="true" />
  </div>
  
  <div>
    <label for="email">Email</label>
    <input id="email" type="email" 
           aria-describedby="email-hint" />
    <span id="email-hint">We'll never share your email</span>
  </div>
  
  <button type="submit">Submit</button>
</form>
```

**Bad form:**
```html
<!-- ❌ No labels, placeholder only -->
<form>
  <input type="text" placeholder="Name" />
  <input type="email" placeholder="Email" />
  <button>Submit</button>
</form>
```

## Error Messages

- [ ] Error messages associated with fields
- [ ] Use aria-describedby for error text
- [ ] aria-invalid="true" on invalid fields
- [ ] Error summary at top of form

**Implementation:**
```html
<form>
  <div>
    <label for="email">Email</label>
    <input id="email" type="email" 
           aria-invalid="true"
           aria-describedby="email-error" />
    <span id="email-error" role="alert">
      Please enter a valid email address
    </span>
  </div>
</form>

<style>
[aria-invalid="true"] {
  border-color: #d32f2f;
}

[role="alert"] {
  color: #d32f2f;
  font-size: 0.875rem;
}
</style>
```

## Fieldsets & Legends

- [ ] Group related fields with <fieldset>
- [ ] Use <legend> for group label
- [ ] Especially for radio buttons and checkboxes

**Example:**
```html
<fieldset>
  <legend>Shipping method</legend>
  
  <label>
    <input type="radio" name="shipping" value="standard" />
    Standard (5-7 days)
  </label>
  
  <label>
    <input type="radio" name="shipping" value="express" />
    Express (2-3 days)
  </label>
</fieldset>
```

## Autocomplete

- [ ] Use autocomplete attribute for common fields
- [ ] Helps users with cognitive disabilities
- [ ] Reduces typing errors

**Example:**
```html
<input type="text" name="name" autocomplete="name" />
<input type="email" name="email" autocomplete="email" />
<input type="tel" name="phone" autocomplete="tel" />
<input type="text" name="address" autocomplete="street-address" />
<input type="text" name="city" autocomplete="address-level2" />
<input type="text" name="zip" autocomplete="postal-code" />
```
```

---

## Category 5: Multimedia

### Goal: Audio and video accessible

**Multimedia Checklist:**

```markdown
## Captions

- [ ] All videos have captions
- [ ] Captions synchronized with audio
- [ ] Include speaker identification
- [ ] Include sound effects [applause], [music]

**Implementation:**
```html
<video controls>
  <source src="video.mp4" type="video/mp4" />
  <track kind="captions" src="captions.vtt" srclang="en" label="English" default />
</video>
```

**WebVTT caption file (captions.vtt):**
```
WEBVTT

00:00:00.000 --> 00:00:03.000
[Music playing]

00:00:03.000 --> 00:00:06.000
Speaker 1: Welcome to our tutorial.

00:00:06.000 --> 00:00:09.000
Speaker 2: Today we'll learn about accessibility.
```

## Transcripts

- [ ] Provide text transcripts for audio/video
- [ ] Include all spoken content
- [ ] Include relevant sound effects
- [ ] Link to transcript near media

**Example:**
```html
<video controls>
  <source src="video.mp4" type="video/mp4" />
</video>

<details>
  <summary>View transcript</summary>
  <div>
    <p><strong>Speaker 1:</strong> Welcome to our tutorial.</p>
    <p><strong>Speaker 2:</strong> Today we'll learn about accessibility.</p>
  </div>
</details>
```

## Audio Descriptions

- [ ] Provide audio descriptions for visual content
- [ ] Describe important visual information
- [ ] Use separate audio track or extended video

## Media Controls

- [ ] Keyboard accessible controls
- [ ] Pause/stop for auto-playing media
- [ ] Volume control
- [ ] No seizure-inducing flashing (< 3 flashes/second)

**Accessible video player:**
```html
<video id="video" controls aria-label="Tutorial video">
  <source src="video.mp4" type="video/mp4" />
  <track kind="captions" src="captions.vtt" srclang="en" label="English" />
</video>

<button onclick="document.getElementById('video').play()" 
        aria-label="Play video">
  Play
</button>

<button onclick="document.getElementById('video').pause()" 
        aria-label="Pause video">
  Pause
</button>
```
```

---

## Category 6: Dynamic Content

### Goal: Dynamic updates accessible

**Dynamic Content Checklist:**

```markdown
## Live Regions

- [ ] Announce dynamic content changes
- [ ] Use aria-live for updates
- [ ] aria-live="polite" for non-urgent
- [ ] aria-live="assertive" for urgent

**Examples:**
```html
<!-- Search results updating -->
<div role="region" aria-live="polite" aria-atomic="true">
  <p>Found 42 results for "accessibility"</p>
</div>

<!-- Form validation -->
<div role="alert" aria-live="assertive">
  Error: Please fill in all required fields
</div>

<!-- Loading state -->
<div aria-live="polite" aria-busy="true">
  Loading content...
</div>
```

## Modals & Dialogs

- [ ] Trap focus inside modal
- [ ] Return focus when closed
- [ ] Close with Esc key
- [ ] Use role="dialog" and aria-modal="true"

**Accessible modal:**
```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm action</h2>
  <p>Are you sure you want to delete this item?</p>
  
  <button onclick="confirmDelete()">Delete</button>
  <button onclick="closeDialog()">Cancel</button>
</div>

<script>
function openDialog() {
  const dialog = document.querySelector('[role="dialog"]');
  const firstFocusable = dialog.querySelector('button');
  
  // Save current focus
  previousFocus = document.activeElement;
  
  // Show dialog
  dialog.style.display = 'block';
  
  // Focus first element
  firstFocusable.focus();
  
  // Trap focus
  dialog.addEventListener('keydown', trapFocus);
}

function closeDialog() {
  const dialog = document.querySelector('[role="dialog"]');
  
  // Hide dialog
  dialog.style.display = 'none';
  
  // Restore focus
  previousFocus.focus();
  
  // Remove trap
  dialog.removeEventListener('keydown', trapFocus);
}

function trapFocus(e) {
  if (e.key === 'Escape') {
    closeDialog();
  }
  
  if (e.key === 'Tab') {
    const dialog = e.currentTarget;
    const focusable = dialog.querySelectorAll('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
    const first = focusable[0];
    const last = focusable[focusable.length - 1];
    
    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault();
      last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault();
      first.focus();
    }
  }
}
</script>
```

## Loading States

- [ ] Announce loading states
- [ ] Use aria-busy="true"
- [ ] Provide progress indication

**Example:**
```html
<div aria-live="polite" aria-busy="true">
  <span role="status">Loading data...</span>
  <progress value="50" max="100" aria-label="Loading progress">50%</progress>
</div>
```

## Infinite Scroll

- [ ] Provide "Load more" button alternative
- [ ] Announce new content loaded
- [ ] Maintain keyboard focus position

**Example:**
```html
<div id="content">
  <!-- Content items -->
</div>

<div role="status" aria-live="polite" aria-atomic="true">
  <span id="load-status"></span>
</div>

<button onclick="loadMore()">Load more</button>

<script>
function loadMore() {
  // Load content
  fetch('/api/items?page=2')
    .then(res => res.json())
    .then(items => {
      // Add items
      items.forEach(item => {
        document.getElementById('content').appendChild(createItem(item));
      });
      
      // Announce
      document.getElementById('load-status').textContent = 
        `Loaded ${items.length} more items`;
    });
}
</script>
```
```

---

## Category 7: Mobile & Touch

### Goal: Accessible on mobile devices

**Mobile Checklist:**

```markdown
## Touch Targets

- [ ] Minimum 44×44px touch target size
- [ ] Adequate spacing between targets (8px minimum)
- [ ] No overlapping touch areas

**Good touch targets:**
```css
button, a {
  min-width: 44px;
  min-height: 44px;
  padding: 12px 16px;
  margin: 8px;
}
```

## Gestures

- [ ] Provide alternatives to complex gestures
- [ ] Don't require multi-finger gestures
- [ ] Support single-tap activation

**Example:**
```html
<!-- Swipe to delete (provide button alternative) -->
<div class="item">
  <span>Item name</span>
  <button onclick="deleteItem()" aria-label="Delete item">
    Delete
  </button>
</div>
```

## Orientation

- [ ] Support both portrait and landscape
- [ ] Don't lock orientation unless essential
- [ ] Content adapts to orientation

**CSS:**
```css
/* Responsive to orientation */
@media (orientation: portrait) {
  .container {
    flex-direction: column;
  }
}

@media (orientation: landscape) {
  .container {
    flex-direction: row;
  }
}
```

## Zoom & Reflow

- [ ] Allow pinch-to-zoom (don't disable)
- [ ] Content reflows at 400% zoom
- [ ] No horizontal scrolling at 320px width

**Good viewport:**
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

**Bad viewport:**
```html
<!-- ❌ Disables zoom -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```
```

---

## Automated Testing Tools

```bash
# Lighthouse (Chrome DevTools)
lighthouse https://example.com --only-categories=accessibility

# axe DevTools (browser extension)
# Install from Chrome Web Store or Firefox Add-ons

# Pa11y (CLI)
npm install -g pa11y
pa11y https://example.com

# axe-core (JavaScript)
npm install axe-core
# Use in tests:
const { AxePuppeteer } = require('@axe-core/puppeteer');
const results = await new AxePuppeteer(page).analyze();

# WAVE (Web Accessibility Evaluation Tool)
# Browser extension or online tool
# https://wave.webaim.org/

# HTML validator
# https://validator.w3.org/
```

---

## Manual Testing Checklist

**Keyboard navigation:**
- [ ] Tab through entire page
- [ ] All interactive elements reachable
- [ ] Logical tab order
- [ ] Visible focus indicators
- [ ] No keyboard traps

**Screen reader testing:**
- [ ] Test with NVDA (Windows, free)
- [ ] Test with JAWS (Windows, paid)
- [ ] Test with VoiceOver (Mac/iOS, built-in)
- [ ] Test with TalkBack (Android, built-in)
- [ ] All content announced correctly
- [ ] Images have alt text
- [ ] Forms have labels
- [ ] Headings in logical order

**Visual testing:**
- [ ] Zoom to 200% (no loss of content)
- [ ] Check color contrast (4.5:1 minimum)
- [ ] Test with grayscale (color not sole indicator)
- [ ] Test with high contrast mode

**Mobile testing:**
- [ ] Touch targets 44×44px minimum
- [ ] Pinch-to-zoom works
- [ ] Content reflows (no horizontal scroll)
- [ ] Portrait and landscape work

---

## Common Accessibility Mistakes

### Mistake 1: Missing Alt Text
**Problem:** Images without alt attributes

**Solution:** Add descriptive alt text or alt="" for decorative images

### Mistake 2: Poor Color Contrast
**Problem:** Text hard to read (low contrast)

**Solution:** Use 4.5:1 contrast ratio minimum

### Mistake 3: No Keyboard Access
**Problem:** Interactive elements only work with mouse

**Solution:** Ensure all functionality keyboard accessible

### Mistake 4: Missing Form Labels
**Problem:** Inputs without associated labels

**Solution:** Use <label> element with for attribute

### Mistake 5: Removing Focus Indicators
**Problem:** outline: none without replacement

**Solution:** Provide custom focus styles

### Mistake 6: Inaccessible Modals
**Problem:** Focus not trapped, can't close with keyboard

**Solution:** Trap focus, close with Esc, return focus on close

---

## Integration with Other Skills

**Use before accessibility audit:**
- `responsive-design` - Mobile-first approach
- `component-library` - Accessible components

**Use during accessibility audit:**
- `web-performance` - Performance affects accessibility
- `systematic-debugging` - Fix accessibility issues

**Use after accessibility audit:**
- `user-research` - Test with users with disabilities
- `requesting-code-review` - Review accessibility changes

---

**Remember:** Accessibility is not a feature, it's a requirement. Build accessibility in from the start, not as an afterthought. Test with real users with disabilities. Accessibility benefits everyone, not just users with disabilities.
