---
name: web-performance
description: Optimize web performance with Core Web Vitals (LCP, INP, CLS), code splitting, lazy loading, caching strategies, and performance budgets. Achieve fast, smooth user experiences.
tags: [performance, core-web-vitals, optimization, lcp, inp, cls, caching, lazy-loading]
version: 1.0.0
author: Enhanced from web performance best practices 2026)
---

# Web Performance

## Overview

Optimize web applications for speed and smooth user experience.

**Why performance matters:**
- **User experience:** Fast sites feel better
- **Conversions:** 100ms delay = 1% revenue loss
- **SEO:** Google ranks faster sites higher
- **Accessibility:** Works on slow connections
- **Retention:** Users abandon slow sites

**Key metrics (Core Web Vitals 2026):**
- **LCP (Largest Contentful Paint):** < 2.5s (loading)
- **INP (Interaction to Next Paint):** < 200ms (interactivity)
- **CLS (Cumulative Layout Shift):** < 0.1 (visual stability)

## When to Use

**Use for:**
- All web applications
- E-commerce sites
- Content-heavy sites
- Mobile experiences
- Global audiences

**Critical for:**
- High-traffic sites
- Mobile-first apps
- Slow network regions
- SEO-dependent sites
- Conversion-focused apps

## The 7-Layer Performance Framework

```
┌─────────────────────────────────────────────────────────┐
│         WEB PERFORMANCE FRAMEWORK                        │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   LAYER 1    │  Measure
    │   MEASURE    │  - Core Web Vitals
    │              │  - Real User Monitoring
    └──────┬───────┘  - Lab testing
           │
           ▼
    ┌──────────────┐
    │   LAYER 2    │  Loading
    │   LOADING    │  - Code splitting
    │              │  - Lazy loading
    └──────┬───────┘  - Preloading
           │
           ▼
    ┌──────────────┐
    │   LAYER 3    │  Rendering
    │  RENDERING   │  - Critical CSS
    │              │  - Font loading
    └──────┬───────┘  - Image optimization
           │
           ▼
    ┌──────────────┐
    │   LAYER 4    │  Caching
    │   CACHING    │  - Service Worker
    │              │  - HTTP caching
    └──────┬───────┘  - CDN
           │
           ▼
    ┌──────────────┐
    │   LAYER 5    │  JavaScript
    │ JAVASCRIPT   │  - Bundle size
    │              │  - Tree shaking
    └──────┬───────┘  - Minification
           │
           ▼
    ┌──────────────┐
    │   LAYER 6    │  Network
    │   NETWORK    │  - HTTP/2, HTTP/3
    │              │  - Compression
    └──────┬───────┘  - Resource hints
           │
           ▼
    ┌──────────────┐
    │   LAYER 7    │  Monitoring
    │ MONITORING   │  - RUM
    │              │  - Alerts
    └──────────────┘  - Budgets
```

## Layer 1: Measure Performance

### Goal: Know your baseline

**Core Web Vitals (2026):**

```markdown
## LCP (Largest Contentful Paint)
**What:** Time until largest content element is visible
**Target:** < 2.5 seconds
**Good:** < 2.5s | Needs Improvement: 2.5-4s | Poor: > 4s

**Common causes:**
- Slow server response
- Render-blocking resources
- Large images
- Client-side rendering

## INP (Interaction to Next Paint)
**What:** Responsiveness to user interactions (replaced FID in 2024)
**Target:** < 200ms
**Good:** < 200ms | Needs Improvement: 200-500ms | Poor: > 500ms

**Common causes:**
- Long JavaScript tasks
- Heavy event handlers
- Blocking main thread
- Large DOM updates

## CLS (Cumulative Layout Shift)
**What:** Visual stability (unexpected layout shifts)
**Target:** < 0.1
**Good:** < 0.1 | Needs Improvement: 0.1-0.25 | Poor: > 0.25

**Common causes:**
- Images without dimensions
- Ads/embeds/iframes
- Web fonts (FOIT/FOUT)
- Dynamic content injection
```

**Measuring Tools:**

```bash
# Lighthouse (lab testing)
npx lighthouse https://example.com --view

# PageSpeed Insights (lab + field data)
# Visit: https://pagespeed.web.dev/

# WebPageTest (detailed analysis)
# Visit: https://www.webpagetest.org/

# Chrome DevTools
# 1. Open DevTools (F12)
# 2. Performance tab
# 3. Record page load
# 4. Analyze waterfall

# Web Vitals extension
# Install: https://chrome.google.com/webstore (search "Web Vitals")

# Real User Monitoring (RUM)
npm install web-vitals
```

**Web Vitals Library:**

```javascript
import { onCLS, onINP, onLCP } from 'web-vitals';

// Log to console
onCLS(console.log);
onINP(console.log);
onLCP(console.log);

// Send to analytics
function sendToAnalytics(metric) {
  const body = JSON.stringify(metric);
  
  // Use `navigator.sendBeacon()` if available, falling back to `fetch()`
  if (navigator.sendBeacon) {
    navigator.sendBeacon('/analytics', body);
  } else {
    fetch('/analytics', { body, method: 'POST', keepalive: true });
  }
}

onCLS(sendToAnalytics);
onINP(sendToAnalytics);
onLCP(sendToAnalytics);

// Attribution (debug info)
import { onLCP } from 'web-vitals/attribution';

onLCP((metric) => {
  console.log('LCP:', metric.value);
  console.log('Element:', metric.attribution.element);
  console.log('URL:', metric.attribution.url);
  console.log('Time to first byte:', metric.attribution.timeToFirstByte);
});
```

---

## Layer 2: Loading Optimization

### Goal: Load only what's needed

**Code Splitting (React):**

```tsx
import { lazy, Suspense } from 'react';

// ❌ Bad: Import everything upfront
import Dashboard from './Dashboard';
import Settings from './Settings';
import Profile from './Profile';

// ✅ Good: Lazy load routes
const Dashboard = lazy(() => import('./Dashboard'));
const Settings = lazy(() => import('./Settings'));
const Profile = lazy(() => import('./Profile'));

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
        <Route path="/profile" element={<Profile />} />
      </Routes>
    </Suspense>
  );
}

// Named exports
const Dashboard = lazy(() =>
  import('./Dashboard').then(module => ({ default: module.Dashboard }))
);

// Preload on hover
function NavLink({ to, children }) {
  const handleMouseEnter = () => {
    // Preload route component
    import(`./pages/${to}`);
  };
  
  return (
    <Link to={to} onMouseEnter={handleMouseEnter}>
      {children}
    </Link>
  );
}
```

**Webpack Code Splitting:**

```javascript
// webpack.config.js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        // Vendor code (node_modules)
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10,
        },
        // Common code (used in 2+ chunks)
        common: {
          minChunks: 2,
          priority: 5,
          reuseExistingChunk: true,
        },
      },
    },
  },
};

// Dynamic imports
button.addEventListener('click', async () => {
  const module = await import('./heavy-module.js');
  module.doSomething();
});
```

**Lazy Loading Images:**

```html
<!-- Native lazy loading -->
<img
  src="image.jpg"
  alt="Description"
  loading="lazy"
  width="800"
  height="600"
/>

<!-- Intersection Observer (custom) -->
<img
  data-src="image.jpg"
  alt="Description"
  class="lazy"
  width="800"
  height="600"
/>

<script>
const images = document.querySelectorAll('img.lazy');

const imageObserver = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      img.classList.remove('lazy');
      observer.unobserve(img);
    }
  });
});

images.forEach(img => imageObserver.observe(img));
</script>
```

**Preloading Critical Resources:**

```html
<!-- Preload critical CSS -->
<link rel="preload" href="/critical.css" as="style">

<!-- Preload critical font -->
<link
  rel="preload"
  href="/fonts/inter.woff2"
  as="font"
  type="font/woff2"
  crossorigin
>

<!-- Preload hero image -->
<link rel="preload" href="/hero.jpg" as="image">

<!-- Prefetch next page -->
<link rel="prefetch" href="/next-page.html">

<!-- DNS prefetch for external domains -->
<link rel="dns-prefetch" href="https://api.example.com">

<!-- Preconnect (DNS + TCP + TLS) -->
<link rel="preconnect" href="https://api.example.com">
```

---

## Layer 3: Rendering Optimization

### Goal: Fast first paint

**Critical CSS (Inline):**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- Inline critical CSS (above-the-fold) -->
  <style>
    /* Critical styles for header, hero, layout */
    body { margin: 0; font-family: sans-serif; }
    .header { background: #fff; padding: 1rem; }
    .hero { height: 100vh; background: #0066cc; }
  </style>
  
  <!-- Async load full CSS -->
  <link
    rel="preload"
    href="/styles.css"
    as="style"
    onload="this.onload=null;this.rel='stylesheet'"
  >
  <noscript><link rel="stylesheet" href="/styles.css"></noscript>
</head>
<body>
  <!-- Content -->
</body>
</html>
```

**Font Loading Strategies:**

```css
/* 1. Font-display: swap (FOUT - Flash of Unstyled Text) */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter.woff2') format('woff2');
  font-display: swap; /* Show fallback immediately, swap when loaded */
}

/* 2. Font-display: optional (best for performance) */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter.woff2') format('woff2');
  font-display: optional; /* Use only if cached, else fallback */
}

/* 3. Preload critical fonts */
<link
  rel="preload"
  href="/fonts/inter.woff2"
  as="font"
  type="font/woff2"
  crossorigin
>

/* 4. Subset fonts (only needed characters) */
/* Use tools like glyphhanger or fonttools */
```

**Image Optimization:**

```html
<!-- 1. Modern formats (WebP, AVIF) -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description" width="800" height="600">
</picture>

<!-- 2. Responsive images -->
<img
  srcset="
    image-400.jpg 400w,
    image-800.jpg 800w,
    image-1200.jpg 1200w
  "
  sizes="(max-width: 600px) 400px, (max-width: 1200px) 800px, 1200px"
  src="image-800.jpg"
  alt="Description"
  width="800"
  height="600"
  loading="lazy"
>

<!-- 3. Always specify dimensions (prevent CLS) -->
<img
  src="image.jpg"
  alt="Description"
  width="800"
  height="600"
  loading="lazy"
>

<!-- 4. Use CSS aspect-ratio for dynamic images -->
<style>
  .image-container {
    aspect-ratio: 16 / 9;
    width: 100%;
  }
  
  .image-container img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
</style>
```

**Image Optimization Tools:**

```bash
# Sharp (Node.js)
npm install sharp

# Convert to WebP
npx sharp input.jpg -o output.webp

# Resize and optimize
npx sharp input.jpg -o output.jpg --resize 800 --quality 80

# ImageOptim (GUI, macOS)
# Download: https://imageoptim.com/

# Squoosh (Web)
# Visit: https://squoosh.app/

# Next.js Image component (automatic optimization)
import Image from 'next/image';

<Image
  src="/image.jpg"
  alt="Description"
  width={800}
  height={600}
  loading="lazy"
  placeholder="blur"
/>
```

---

## Layer 4: Caching Strategies

### Goal: Serve from cache when possible

**HTTP Caching Headers:**

```nginx
# nginx.conf

# Static assets (immutable, long cache)
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
  expires 1y;
  add_header Cache-Control "public, immutable";
}

# HTML (short cache, revalidate)
location ~* \.html$ {
  expires 5m;
  add_header Cache-Control "public, must-revalidate";
}

# API responses (no cache)
location /api/ {
  add_header Cache-Control "no-store, no-cache, must-revalidate";
}
```

**Service Worker (Offline support):**

```javascript
// service-worker.js

const CACHE_NAME = 'v1';
const urlsToCache = [
  '/',
  '/styles.css',
  '/script.js',
  '/logo.png',
];

// Install: cache static assets
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(urlsToCache))
  );
});

// Fetch: cache-first strategy
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        // Cache hit - return response
        if (response) {
          return response;
        }
        
        // Clone request
        const fetchRequest = event.request.clone();
        
        return fetch(fetchRequest).then(response => {
          // Check if valid response
          if (!response || response.status !== 200 || response.type !== 'basic') {
            return response;
          }
          
          // Clone response
          const responseToCache = response.clone();
          
          caches.open(CACHE_NAME)
            .then(cache => {
              cache.put(event.request, responseToCache);
            });
          
          return response;
        });
      })
  );
});

// Activate: clean old caches
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cacheName => {
          if (cacheName !== CACHE_NAME) {
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
});

// Register service worker
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(reg => console.log('Service Worker registered'))
    .catch(err => console.error('Service Worker registration failed', err));
}
```

**Caching Strategies:**

```javascript
// 1. Cache First (static assets)
async function cacheFirst(request) {
  const cached = await caches.match(request);
  return cached || fetch(request);
}

// 2. Network First (API, dynamic content)
async function networkFirst(request) {
  try {
    const response = await fetch(request);
    const cache = await caches.open(CACHE_NAME);
    cache.put(request, response.clone());
    return response;
  } catch (error) {
    return caches.match(request);
  }
}

// 3. Stale While Revalidate (balance freshness + speed)
async function staleWhileRevalidate(request) {
  const cached = await caches.match(request);
  
  const fetchPromise = fetch(request).then(response => {
    const cache = caches.open(CACHE_NAME);
    cache.then(c => c.put(request, response.clone()));
    return response;
  });
  
  return cached || fetchPromise;
}
```

---

## Layer 5: JavaScript Optimization

### Goal: Minimize JavaScript impact

**Bundle Analysis:**

```bash
# Webpack Bundle Analyzer
npm install --save-dev webpack-bundle-analyzer

# webpack.config.js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin(),
  ],
};

# Run build and view report
npm run build

# Vite Bundle Analyzer
npm install --save-dev rollup-plugin-visualizer

# vite.config.js
import { visualizer } from 'rollup-plugin-visualizer';

export default {
  plugins: [
    visualizer({ open: true }),
  ],
};
```

**Tree Shaking:**

```javascript
// ❌ Bad: Import entire library
import _ from 'lodash';
_.debounce(fn, 300);

// ✅ Good: Import only what you need
import debounce from 'lodash/debounce';
debounce(fn, 300);

// ✅ Better: Use ES modules (automatic tree shaking)
import { debounce } from 'lodash-es';
debounce(fn, 300);

// package.json: mark as side-effect-free
{
  "sideEffects": false
}

// Or specify files with side effects
{
  "sideEffects": ["*.css", "*.scss"]
}
```

**Minification:**

```javascript
// webpack.config.js
const TerserPlugin = require('terser-webpack-plugin');

module.exports = {
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true, // Remove console.log
          },
        },
      }),
    ],
  },
};

// Vite (automatic in production)
// vite.config.js
export default {
  build: {
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true,
      },
    },
  },
};
```

**Defer/Async Scripts:**

```html
<!-- ❌ Bad: Blocking script -->
<script src="/script.js"></script>

<!-- ✅ Good: Defer (execute after HTML parsed) -->
<script src="/script.js" defer></script>

<!-- ✅ Good: Async (execute when downloaded) -->
<script src="/analytics.js" async></script>

<!-- ✅ Best: Module (defer by default) -->
<script type="module" src="/app.js"></script>
```

---

## Layer 6: Network Optimization

### Goal: Fast data transfer

**HTTP/2 & HTTP/3:**

```nginx
# nginx.conf

# Enable HTTP/2
listen 443 ssl http2;

# Enable HTTP/3 (QUIC)
listen 443 quic reuseport;
add_header Alt-Svc 'h3=":443"; ma=86400';

# Server push (HTTP/2)
http2_push /styles.css;
http2_push /script.js;
```

**Compression:**

```nginx
# nginx.conf

# Gzip compression
gzip on;
gzip_vary on;
gzip_min_length 1024;
gzip_types
  text/plain
  text/css
  text/javascript
  application/javascript
  application/json
  application/xml
  image/svg+xml;

# Brotli compression (better than gzip)
brotli on;
brotli_types
  text/plain
  text/css
  text/javascript
  application/javascript
  application/json
  application/xml
  image/svg+xml;
```

**CDN (Content Delivery Network):**

```html
<!-- Use CDN for static assets -->
<script src="https://cdn.example.com/script.js"></script>
<link rel="stylesheet" href="https://cdn.example.com/styles.css">

<!-- Popular CDNs -->
<!-- Cloudflare, Fastly, AWS CloudFront, Vercel Edge Network -->
```

---

## Layer 7: Monitoring & Budgets

### Goal: Maintain performance over time

**Performance Budgets:**

```javascript
// webpack.config.js
module.exports = {
  performance: {
    maxAssetSize: 250000, // 250 KB
    maxEntrypointSize: 400000, // 400 KB
    hints: 'error', // Fail build if exceeded
  },
};

// Lighthouse CI
// lighthouserc.json
{
  "ci": {
    "assert": {
      "assertions": {
        "first-contentful-paint": ["error", { "maxNumericValue": 2000 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 2500 }],
        "interactive": ["error", { "maxNumericValue": 3500 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }]
      }
    }
  }
}
```

**Real User Monitoring (RUM):**

```javascript
// Send metrics to analytics
import { onCLS, onINP, onLCP, onFCP, onTTFB } from 'web-vitals';

function sendToAnalytics(metric) {
  // Google Analytics 4
  gtag('event', metric.name, {
    value: Math.round(metric.value),
    metric_id: metric.id,
    metric_value: metric.value,
    metric_delta: metric.delta,
  });
  
  // Or custom endpoint
  fetch('/analytics', {
    method: 'POST',
    body: JSON.stringify(metric),
    keepalive: true,
  });
}

onCLS(sendToAnalytics);
onINP(sendToAnalytics);
onLCP(sendToAnalytics);
onFCP(sendToAnalytics);
onTTFB(sendToAnalytics);
```

---

## Performance Checklist

**Loading:**
- [ ] Code splitting implemented
- [ ] Lazy loading for routes and images
- [ ] Critical resources preloaded
- [ ] Bundle size < 250 KB (initial)

**Rendering:**
- [ ] Critical CSS inlined
- [ ] Fonts optimized (font-display: swap/optional)
- [ ] Images optimized (WebP/AVIF, responsive, dimensions)
- [ ] No layout shifts (CLS < 0.1)

**Caching:**
- [ ] HTTP caching headers configured
- [ ] Service Worker implemented (optional)
- [ ] CDN for static assets
- [ ] Compression enabled (Gzip/Brotli)

**JavaScript:**
- [ ] Tree shaking enabled
- [ ] Minification enabled
- [ ] Scripts deferred/async
- [ ] No blocking scripts

**Metrics:**
- [ ] LCP < 2.5s
- [ ] INP < 200ms
- [ ] CLS < 0.1
- [ ] Performance budgets set
- [ ] RUM implemented

---

## Common Performance Mistakes

### Mistake 1: No Image Optimization
**Problem:** Serving 5MB images

**Solution:** Optimize, resize, use modern formats

### Mistake 2: Blocking Scripts
**Problem:** `<script src="...">` in `<head>`

**Solution:** Use `defer` or `async`

### Mistake 3: No Code Splitting
**Problem:** 2MB initial bundle

**Solution:** Lazy load routes and components

### Mistake 4: Missing Dimensions
**Problem:** Images without width/height cause CLS

**Solution:** Always specify dimensions

### Mistake 5: No Caching
**Problem:** Re-downloading assets on every visit

**Solution:** Configure HTTP caching headers

---

## Integration with Other Skills

**Use before optimizing:**
- `architecture-blueprint` - Performance architecture
- `accessibility-audit` - Accessible performance

**Use during optimization:**
- `systematic-debugging` - Debug performance issues
- `test-driven-development` - Performance tests

**Use after optimization:**
- `requesting-code-review` - Review optimizations
- `user-research` - Test with real users

---

**Remember:** Performance is a feature, not an afterthought. Measure first, optimize what matters, set budgets, and monitor continuously. Fast sites win.
