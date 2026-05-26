---
name: responsive-design
description: Mobile-first responsive design - breakpoints, flexible layouts, media queries, responsive images, and cross-device testing. Build interfaces that work everywhere.
tags: [responsive, mobile-first, css, breakpoints, media-queries, flexbox, grid]
version: 1.0.0
author: Enhanced from responsive design best practices)
---

# Responsive Design

## Overview

Build interfaces that adapt seamlessly to any screen size.

**Why responsive design matters:**
- **Mobile traffic:** 60%+ of web traffic is mobile
- **SEO:** Google prioritizes mobile-friendly sites
- **User experience:** Users expect sites to work on their device
- **Cost:** One codebase for all devices
- **Future-proof:** Works on devices that don't exist yet

**Key principles:**
- **Mobile-first:** Design for small screens first
- **Flexible layouts:** Use relative units (%, em, rem)
- **Breakpoints:** Adapt at specific screen sizes
- **Touch-friendly:** Large tap targets (44×44px)
- **Performance:** Fast on slow mobile networks

## When to Use

**Use for:**
- All web applications
- All websites
- Mobile apps (responsive web views)
- Email templates
- Any user-facing interface

**Critical for:**
- E-commerce (mobile conversion)
- Content sites (mobile readership)
- SaaS applications (mobile access)
- Marketing sites (mobile traffic)

## The 5-Layer Responsive Framework

```
┌─────────────────────────────────────────────────────────┐
│           RESPONSIVE DESIGN FRAMEWORK                    │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   LAYER 1    │  Mobile-First Strategy
    │ MOBILE-FIRST │  - Start with mobile
    │              │  - Progressive enhancement
    └──────┬───────┘  - Content priority
           │
           ▼
    ┌──────────────┐
    │   LAYER 2    │  Flexible Layouts
    │   LAYOUTS    │  - Flexbox
    │              │  - CSS Grid
    └──────┬───────┘  - Relative units
           │
           ▼
    ┌──────────────┐
    │   LAYER 3    │  Breakpoints
    │ BREAKPOINTS  │  - Media queries
    │              │  - Device ranges
    └──────┬───────┘  - Content-based
           │
           ▼
    ┌──────────────┐
    │   LAYER 4    │  Responsive Images
    │    IMAGES    │  - srcset
    │              │  - picture element
    └──────┬───────┘  - Lazy loading
           │
           ▼
    ┌──────────────┐
    │   LAYER 5    │  Testing
    │   TESTING    │  - Device testing
    │              │  - Browser testing
    └──────────────┘  - Performance
```

## Layer 1: Mobile-First Strategy

### Goal: Design for mobile first, enhance for desktop

**Mobile-First Approach:**

```css
/* Base styles (mobile) */
.container {
  width: 100%;
  padding: 16px;
}

.card {
  margin-bottom: 16px;
}

/* Tablet (min-width: 768px) */
@media (min-width: 768px) {
  .container {
    padding: 24px;
  }
  
  .card {
    width: 48%;
    display: inline-block;
  }
}

/* Desktop (min-width: 1024px) */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 32px;
  }
  
  .card {
    width: 32%;
  }
}
```

**Desktop-First (❌ Don't do this):**

```css
/* Desktop styles */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 32px;
}

/* Tablet (max-width: 1024px) */
@media (max-width: 1024px) {
  .container {
    padding: 24px;
  }
}

/* Mobile (max-width: 768px) */
@media (max-width: 768px) {
  .container {
    padding: 16px;
  }
}
```

**Why Mobile-First?**

```markdown
## Benefits

1. **Performance:** Mobile gets minimal CSS, desktop gets enhancements
2. **Progressive enhancement:** Works on all devices
3. **Content priority:** Forces focus on essential content
4. **Future-proof:** New devices get mobile styles by default
5. **Easier to maintain:** Add features, don't remove them

## Content Priority

**Mobile forces you to prioritize:**
- What's most important?
- What can be hidden/collapsed?
- What's the core user journey?

**Example:**
```html
<!-- Mobile: Essential content only -->
<header>
  <h1>Logo</h1>
  <button>Menu</button>
</header>

<!-- Desktop: Full navigation -->
<header>
  <h1>Logo</h1>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
    <a href="/products">Products</a>
    <a href="/contact">Contact</a>
  </nav>
  <button>Search</button>
  <button>Cart</button>
</header>
```
```

---

## Layer 2: Flexible Layouts

### Goal: Layouts that adapt to any screen size

**Flexbox:**

```css
/* Flexible container */
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

/* Flexible items */
.item {
  flex: 1 1 300px; /* grow, shrink, basis */
  min-width: 0; /* Prevent overflow */
}

/* Common patterns */

/* Center content */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Space between */
.space-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Column on mobile, row on desktop */
.responsive-flex {
  display: flex;
  flex-direction: column;
}

@media (min-width: 768px) {
  .responsive-flex {
    flex-direction: row;
  }
}
```

**CSS Grid:**

```css
/* Grid container */
.grid {
  display: grid;
  gap: 16px;
  
  /* Mobile: 1 column */
  grid-template-columns: 1fr;
}

/* Tablet: 2 columns */
@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop: 3 columns */
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Auto-fit (responsive without media queries) */
.auto-grid {
  display: grid;
  gap: 16px;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

/* Common layouts */

/* Sidebar layout */
.sidebar-layout {
  display: grid;
  gap: 24px;
  
  /* Mobile: stacked */
  grid-template-columns: 1fr;
}

@media (min-width: 1024px) {
  .sidebar-layout {
    /* Desktop: sidebar + main */
    grid-template-columns: 250px 1fr;
  }
}

/* Holy grail layout */
.holy-grail {
  display: grid;
  min-height: 100vh;
  grid-template-rows: auto 1fr auto;
  grid-template-columns: 1fr;
}

@media (min-width: 1024px) {
  .holy-grail {
    grid-template-columns: 200px 1fr 200px;
  }
  
  .header {
    grid-column: 1 / -1;
  }
  
  .footer {
    grid-column: 1 / -1;
  }
}
```

**Relative Units:**

```css
/* Use relative units, not fixed pixels */

/* ❌ Bad: Fixed pixels */
.container {
  width: 1200px;
  padding: 20px;
  font-size: 16px;
}

/* ✅ Good: Relative units */
.container {
  max-width: 75rem; /* 1200px / 16px = 75rem */
  padding: 1.25rem; /* 20px / 16px = 1.25rem */
  font-size: 1rem; /* 16px */
}

/* Percentage for widths */
.column {
  width: 50%; /* Half of parent */
}

/* rem for spacing and typography */
.spacing {
  margin-bottom: 1.5rem; /* 24px */
  padding: 1rem 2rem; /* 16px 32px */
}

/* em for component-relative sizing */
.button {
  font-size: 1rem;
  padding: 0.5em 1em; /* Relative to button font-size */
}

/* vw/vh for viewport-relative sizing */
.hero {
  height: 100vh; /* Full viewport height */
  width: 100vw; /* Full viewport width */
}

/* clamp() for fluid typography */
.heading {
  font-size: clamp(1.5rem, 5vw, 3rem);
  /* Min: 1.5rem (24px)
     Preferred: 5vw (5% of viewport width)
     Max: 3rem (48px) */
}
```

---

## Layer 3: Breakpoints

### Goal: Adapt layout at specific screen sizes

**Standard Breakpoints:**

```css
/* Mobile-first breakpoints */

/* Extra small (default, no media query needed) */
/* 0px - 575px */

/* Small (sm) - Large phones */
@media (min-width: 576px) {
  /* Styles for ≥576px */
}

/* Medium (md) - Tablets */
@media (min-width: 768px) {
  /* Styles for ≥768px */
}

/* Large (lg) - Desktops */
@media (min-width: 1024px) {
  /* Styles for ≥1024px */
}

/* Extra large (xl) - Large desktops */
@media (min-width: 1280px) {
  /* Styles for ≥1280px */
}

/* 2X large (2xl) - Very large screens */
@media (min-width: 1536px) {
  /* Styles for ≥1536px */
}
```

**Content-Based Breakpoints:**

```css
/* Don't just use standard breakpoints */
/* Add breakpoints where YOUR content breaks */

.card-grid {
  display: grid;
  gap: 16px;
  grid-template-columns: 1fr;
}

/* Content breaks at 500px, not 576px */
@media (min-width: 500px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Content breaks at 900px, not 1024px */
@media (min-width: 900px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

**Breakpoint Ranges:**

```css
/* Target specific ranges */

/* Only tablets (768px - 1023px) */
@media (min-width: 768px) and (max-width: 1023px) {
  .tablet-only {
    display: block;
  }
}

/* Only mobile (0px - 767px) */
@media (max-width: 767px) {
  .mobile-only {
    display: block;
  }
}

/* Only desktop (≥1024px) */
@media (min-width: 1024px) {
  .desktop-only {
    display: block;
  }
}
```

**Orientation:**

```css
/* Portrait (height > width) */
@media (orientation: portrait) {
  .container {
    flex-direction: column;
  }
}

/* Landscape (width > height) */
@media (orientation: landscape) {
  .container {
    flex-direction: row;
  }
}
```

**Print:**

```css
/* Print styles */
@media print {
  /* Hide navigation, ads, etc. */
  nav, aside, .no-print {
    display: none;
  }
  
  /* Optimize for printing */
  body {
    font-size: 12pt;
    color: black;
    background: white;
  }
  
  /* Page breaks */
  h1, h2, h3 {
    page-break-after: avoid;
  }
  
  /* Show link URLs */
  a[href]:after {
    content: " (" attr(href) ")";
  }
}
```

---

## Layer 4: Responsive Images

### Goal: Serve appropriate images for each device

**srcset (Resolution Switching):**

```html
<!-- Serve different sizes based on viewport width -->
<img 
  src="image-800w.jpg"
  srcset="
    image-400w.jpg 400w,
    image-800w.jpg 800w,
    image-1200w.jpg 1200w,
    image-1600w.jpg 1600w
  "
  sizes="
    (max-width: 600px) 100vw,
    (max-width: 1200px) 50vw,
    33vw
  "
  alt="Responsive image"
/>

<!-- Explanation:
  - srcset: Available image sizes (400w = 400px wide)
  - sizes: How much space image takes at different viewports
  - Browser picks best image based on viewport and pixel density
-->
```

**picture Element (Art Direction):**

```html
<!-- Different crops for different screen sizes -->
<picture>
  <!-- Mobile: Square crop -->
  <source 
    media="(max-width: 767px)" 
    srcset="image-square-400w.jpg 400w, image-square-800w.jpg 800w"
  />
  
  <!-- Tablet: 16:9 crop -->
  <source 
    media="(max-width: 1023px)" 
    srcset="image-wide-800w.jpg 800w, image-wide-1600w.jpg 1600w"
  />
  
  <!-- Desktop: Original crop -->
  <source 
    srcset="image-original-1200w.jpg 1200w, image-original-2400w.jpg 2400w"
  />
  
  <!-- Fallback -->
  <img src="image-original-1200w.jpg" alt="Responsive image" />
</picture>
```

**WebP with Fallback:**

```html
<picture>
  <!-- WebP for modern browsers -->
  <source type="image/webp" srcset="image.webp" />
  
  <!-- JPEG fallback -->
  <img src="image.jpg" alt="Image" />
</picture>
```

**Lazy Loading:**

```html
<!-- Native lazy loading -->
<img src="image.jpg" alt="Image" loading="lazy" />

<!-- Lazy load below the fold images -->
<img 
  src="placeholder.jpg" 
  data-src="actual-image.jpg" 
  alt="Image" 
  loading="lazy"
/>
```

**Background Images:**

```css
/* Responsive background images */
.hero {
  background-image: url('hero-mobile.jpg');
  background-size: cover;
  background-position: center;
}

@media (min-width: 768px) {
  .hero {
    background-image: url('hero-tablet.jpg');
  }
}

@media (min-width: 1024px) {
  .hero {
    background-image: url('hero-desktop.jpg');
  }
}

/* High DPI (Retina) displays */
@media (min-width: 1024px) and (-webkit-min-device-pixel-ratio: 2) {
  .hero {
    background-image: url('hero-desktop@2x.jpg');
  }
}
```

---

## Layer 5: Testing

### Goal: Verify responsive design works everywhere

**Browser DevTools:**

```bash
# Chrome DevTools
# 1. Open DevTools (F12)
# 2. Click device toolbar icon (Ctrl+Shift+M)
# 3. Select device or enter custom dimensions
# 4. Test at different sizes

# Common test sizes:
# - 320px (iPhone SE)
# - 375px (iPhone 12/13)
# - 390px (iPhone 14)
# - 768px (iPad)
# - 1024px (iPad Pro)
# - 1280px (Desktop)
# - 1920px (Large desktop)
```

**Real Device Testing:**

```markdown
## Test on Real Devices

**iOS:**
- iPhone SE (small screen)
- iPhone 14 (standard)
- iPhone 14 Pro Max (large)
- iPad (tablet)

**Android:**
- Samsung Galaxy S23 (standard)
- Google Pixel 7 (standard)
- Samsung Galaxy Tab (tablet)

**Desktop:**
- Windows (Chrome, Edge, Firefox)
- Mac (Safari, Chrome, Firefox)
- Linux (Chrome, Firefox)

## Testing Tools

**BrowserStack:** Test on real devices in the cloud
**LambdaTest:** Cross-browser testing
**Sauce Labs:** Automated testing
**Responsively App:** Desktop app for responsive testing
```

**Responsive Testing Checklist:**

```markdown
## Visual Testing

- [ ] Layout doesn't break at any size
- [ ] No horizontal scrolling (except intentional)
- [ ] Images scale properly
- [ ] Text is readable (not too small)
- [ ] Touch targets are 44×44px minimum
- [ ] Navigation works on mobile
- [ ] Forms are usable on mobile
- [ ] Modals/popups work on mobile

## Functional Testing

- [ ] All features work on mobile
- [ ] Touch gestures work (swipe, pinch, tap)
- [ ] Keyboard works on tablets
- [ ] Orientation change works (portrait ↔ landscape)
- [ ] Zoom works (pinch-to-zoom)
- [ ] Performance is acceptable on mobile

## Content Testing

- [ ] All content visible on mobile
- [ ] No content hidden unintentionally
- [ ] Images have appropriate alt text
- [ ] Videos play on mobile
- [ ] Forms are accessible

## Performance Testing

- [ ] Page loads in <3s on 3G
- [ ] Images optimized (WebP, lazy loading)
- [ ] CSS/JS minified
- [ ] No render-blocking resources
- [ ] Core Web Vitals pass (LCP, FID, CLS)
```

---

## Common Responsive Patterns

**Navigation:**

```html
<!-- Mobile: Hamburger menu -->
<nav class="mobile-nav">
  <button class="menu-toggle" aria-label="Toggle menu">
    ☰
  </button>
  
  <ul class="menu" hidden>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>

<style>
/* Mobile: Hidden by default */
.menu {
  display: none;
}

.menu[hidden="false"] {
  display: block;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  background: white;
  padding: 16px;
}

/* Desktop: Always visible */
@media (min-width: 768px) {
  .menu-toggle {
    display: none;
  }
  
  .menu {
    display: flex !important;
    position: static;
    gap: 24px;
  }
}
</style>

<script>
document.querySelector('.menu-toggle').addEventListener('click', () => {
  const menu = document.querySelector('.menu');
  const isHidden = menu.getAttribute('hidden') === 'true';
  menu.setAttribute('hidden', !isHidden);
});
</script>
```

**Cards:**

```css
/* Responsive card grid */
.card-grid {
  display: grid;
  gap: 16px;
  
  /* Mobile: 1 column */
  grid-template-columns: 1fr;
}

/* Tablet: 2 columns */
@media (min-width: 768px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop: 3 columns */
@media (min-width: 1024px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Or auto-fit (no media queries) */
.card-grid-auto {
  display: grid;
  gap: 16px;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
```

**Typography:**

```css
/* Fluid typography */
body {
  /* Base: 16px, scales with viewport */
  font-size: clamp(1rem, 2.5vw, 1.125rem);
  line-height: 1.6;
}

h1 {
  /* Min: 2rem (32px), Max: 4rem (64px) */
  font-size: clamp(2rem, 5vw + 1rem, 4rem);
  line-height: 1.2;
}

h2 {
  font-size: clamp(1.5rem, 4vw + 0.5rem, 3rem);
}

h3 {
  font-size: clamp(1.25rem, 3vw + 0.25rem, 2rem);
}

/* Or with media queries */
h1 {
  font-size: 2rem; /* Mobile: 32px */
}

@media (min-width: 768px) {
  h1 {
    font-size: 3rem; /* Tablet: 48px */
  }
}

@media (min-width: 1024px) {
  h1 {
    font-size: 4rem; /* Desktop: 64px */
  }
}
```

**Tables:**

```css
/* Responsive table (horizontal scroll) */
.table-container {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

table {
  width: 100%;
  min-width: 600px; /* Prevent squishing */
}

/* Or stack on mobile */
@media (max-width: 767px) {
  table, thead, tbody, th, td, tr {
    display: block;
  }
  
  thead tr {
    position: absolute;
    top: -9999px;
    left: -9999px;
  }
  
  tr {
    margin-bottom: 16px;
    border: 1px solid #ddd;
  }
  
  td {
    border: none;
    position: relative;
    padding-left: 50%;
  }
  
  td:before {
    content: attr(data-label);
    position: absolute;
    left: 6px;
    font-weight: bold;
  }
}
```

---

## Responsive Design Checklist

**Mobile-First:**
- [ ] Start with mobile styles
- [ ] Use min-width media queries
- [ ] Progressive enhancement

**Flexible Layouts:**
- [ ] Use Flexbox or CSS Grid
- [ ] Use relative units (%, rem, em)
- [ ] Avoid fixed widths

**Breakpoints:**
- [ ] Standard breakpoints (576px, 768px, 1024px, 1280px)
- [ ] Content-based breakpoints where needed
- [ ] Test at all breakpoints

**Images:**
- [ ] Use srcset for resolution switching
- [ ] Use picture for art direction
- [ ] Lazy load below-the-fold images
- [ ] Optimize images (WebP, compression)

**Touch:**
- [ ] Touch targets 44×44px minimum
- [ ] Adequate spacing between targets
- [ ] Touch-friendly navigation

**Testing:**
- [ ] Test on real devices
- [ ] Test at various sizes
- [ ] Test orientation changes
- [ ] Test performance on mobile

---

## Common Responsive Mistakes

### Mistake 1: Desktop-First
**Problem:** Mobile gets bloated CSS

**Solution:** Start with mobile, enhance for desktop

### Mistake 2: Fixed Widths
**Problem:** Layout breaks on small screens

**Solution:** Use max-width, percentages, or flexible units

### Mistake 3: Ignoring Touch
**Problem:** Tiny tap targets, hover-only interactions

**Solution:** 44×44px targets, no hover-only features

### Mistake 4: Not Testing on Real Devices
**Problem:** Looks good in DevTools, broken on real phones

**Solution:** Test on actual devices

### Mistake 5: Disabling Zoom
**Problem:** Accessibility issue

**Solution:** Allow pinch-to-zoom

### Mistake 6: Too Many Breakpoints
**Problem:** Hard to maintain

**Solution:** Use content-based breakpoints, auto-fit grids

---

## Integration with Other Skills

**Use before responsive design:**
- `accessibility-audit` - Accessible responsive design
- `performance-optimization` - Fast mobile experience

**Use during responsive design:**
- `component-library` - Responsive components
- `web-performance` - Optimize for mobile

**Use after responsive design:**
- `user-research` - Test with real users
- `requesting-code-review` - Review responsive changes

---

**Remember:** Responsive design is not optional. 60%+ of traffic is mobile. Design for mobile first, enhance for desktop. Test on real devices. Performance matters more on mobile. Touch targets must be 44×44px minimum.
