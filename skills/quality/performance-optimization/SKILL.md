---
name: performance-optimization
description: Systematic performance optimization - profiling, bottleneck identification, caching strategies, database optimization, and frontend performance. Measure first, optimize second.
tags: [performance, optimization, profiling, caching, speed, scalability]
version: 1.0.0
author: Enhanced from performance best practices)
---

# Performance Optimization

## Overview

Make your application fast through systematic measurement and optimization.

**Core principle:** Measure first, optimize second. Don't guess where the bottleneck is.

**Performance matters:**
- **User experience:** Fast apps feel better
- **Conversion:** 100ms delay = 1% revenue loss
- **SEO:** Google ranks faster sites higher
- **Cost:** Efficient code costs less to run
- **Scale:** Optimized apps handle more load

**Key concepts:**
- Profiling (find bottlenecks)
- Caching (avoid repeated work)
- Database optimization (indexes, queries)
- Frontend performance (bundle size, rendering)
- Monitoring (track over time)

## When to Use

**Use for:**
- Slow page loads (>3 seconds)
- High server costs
- Database timeouts
- API latency issues
- Poor user experience
- Scaling problems

**Critical for:**
- High-traffic applications
- Real-time systems
- Mobile applications
- E-commerce (conversion-critical)
- API services (SLA requirements)

## The 5-Phase Optimization Framework

```
┌─────────────────────────────────────────────────────────┐
│         PERFORMANCE OPTIMIZATION FRAMEWORK               │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   PHASE 1    │  Measure Baseline
    │   MEASURE    │  - Current performance
    │              │  - Identify bottlenecks
    └──────┬───────┘  - Set targets
           │
           ▼
    ┌──────────────┐
    │   PHASE 2    │  Profile & Analyze
    │   PROFILE    │  - CPU profiling
    │              │  - Memory profiling
    └──────┬───────┘  - Network analysis
           │
           ▼
    ┌──────────────┐
    │   PHASE 3    │  Optimize
    │  OPTIMIZE    │  - Fix bottlenecks
    │              │  - Add caching
    └──────┬───────┘  - Optimize queries
           │
           ▼
    ┌──────────────┐
    │   PHASE 4    │  Verify
    │   VERIFY     │  - Measure again
    │              │  - Compare results
    └──────┬───────┘  - Check regressions
           │
           ▼
    ┌──────────────┐
    │   PHASE 5    │  Monitor
    │   MONITOR    │  - Track metrics
    │              │  - Set alerts
    └──────────────┘  - Continuous improvement
```

## Phase 1: Measure Baseline

### Goal: Know your current performance

**Key Metrics:**

```markdown
## Backend Metrics

### Response Time
- **P50 (median):** 50% of requests faster than this
- **P95:** 95% of requests faster than this
- **P99:** 99% of requests faster than this
- **Target:** P95 < 200ms, P99 < 500ms

### Throughput
- **Requests per second (RPS):** How many requests handled
- **Target:** Depends on scale (100 RPS for small, 10K+ for large)

### Error Rate
- **5xx errors:** Server errors
- **4xx errors:** Client errors
- **Target:** < 0.1% error rate

### Resource Usage
- **CPU:** % utilization
- **Memory:** MB used, % of total
- **Disk I/O:** Read/write operations per second
- **Network:** Bandwidth usage

## Frontend Metrics

### Core Web Vitals
- **LCP (Largest Contentful Paint):** < 2.5s
- **FID (First Input Delay):** < 100ms
- **CLS (Cumulative Layout Shift):** < 0.1
- **INP (Interaction to Next Paint):** < 200ms

### Load Times
- **TTFB (Time to First Byte):** < 600ms
- **FCP (First Contentful Paint):** < 1.8s
- **TTI (Time to Interactive):** < 3.8s

### Bundle Size
- **JavaScript:** < 200KB (gzipped)
- **CSS:** < 50KB (gzipped)
- **Images:** Optimized (WebP, lazy loading)
```

**Measurement Tools:**

```bash
# Backend profiling (Python)
python -m cProfile -o profile.stats app.py
python -c "import pstats; p = pstats.Stats('profile.stats'); p.sort_stats('cumulative'); p.print_stats(20)"

# Backend profiling (Node.js)
node --prof app.js
node --prof-process isolate-*.log > profile.txt

# Frontend profiling (Lighthouse)
npm install -g lighthouse
lighthouse https://example.com --output html --output-path ./report.html

# Load testing (Apache Bench)
ab -n 1000 -c 10 http://localhost:3000/api/endpoint

# Load testing (wrk)
wrk -t4 -c100 -d30s http://localhost:3000/api/endpoint
```

---

## Phase 2: Profile & Analyze

### Goal: Find the bottlenecks

**CPU Profiling:**

```markdown
## Python (cProfile + snakeviz)

```bash
# Profile your application
python -m cProfile -o profile.stats app.py

# Visualize with snakeviz
pip install snakeviz
snakeviz profile.stats
```

**Look for:**
- Functions with high cumulative time
- Functions called many times
- Nested loops
- Inefficient algorithms

## Node.js (--inspect + Chrome DevTools)

```bash
# Start with profiling
node --inspect app.js

# Open Chrome DevTools
# chrome://inspect
# Click "Open dedicated DevTools for Node"
# Go to Profiler tab, start recording
```

**Look for:**
- Hot functions (high self time)
- Synchronous operations blocking event loop
- Large object allocations
```

**Memory Profiling:**

```markdown
## Python (memory_profiler)

```bash
pip install memory-profiler

# Add @profile decorator to functions
# Run with:
python -m memory_profiler script.py
```

**Look for:**
- Memory leaks (growing over time)
- Large object allocations
- Unnecessary data copies

## Node.js (--inspect + Chrome DevTools)

```bash
node --inspect app.js
# Chrome DevTools → Memory tab → Take heap snapshot
```

**Look for:**
- Retained objects (not garbage collected)
- Large arrays/objects
- Circular references
```

**Database Profiling:**

```markdown
## PostgreSQL

```sql
-- Enable query logging
ALTER SYSTEM SET log_min_duration_statement = 100; -- Log queries > 100ms
SELECT pg_reload_conf();

-- Explain query plan
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- Find slow queries
SELECT query, mean_exec_time, calls
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 10;
```

## MySQL

```sql
-- Enable slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 0.1; -- 100ms

-- Explain query plan
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';

-- Show slow queries
SELECT * FROM mysql.slow_log ORDER BY query_time DESC LIMIT 10;
```

**Look for:**
- Full table scans (no index used)
- High execution time
- Many rows examined
- Missing indexes
```

**Network Analysis:**

```bash
# Chrome DevTools → Network tab
# Look for:
# - Large payloads (>1MB)
# - Many requests (>50)
# - Slow requests (>1s)
# - Uncompressed responses
# - Missing caching headers

# curl with timing
curl -w "@curl-format.txt" -o /dev/null -s https://example.com

# curl-format.txt:
#     time_namelookup:  %{time_namelookup}\n
#        time_connect:  %{time_connect}\n
#     time_appconnect:  %{time_appconnect}\n
#    time_pretransfer:  %{time_pretransfer}\n
#       time_redirect:  %{time_redirect}\n
#  time_starttransfer:  %{time_starttransfer}\n
#                     ----------\n
#          time_total:  %{time_total}\n
```

---

## Phase 3: Optimize

### Goal: Fix the bottlenecks

**Backend Optimization:**

```markdown
## 1. Caching

### In-Memory Cache (Redis)

```python
import redis
import json

cache = redis.Redis(host='localhost', port=6379, db=0)

def get_user(user_id):
    # Check cache first
    cached = cache.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)
    
    # Cache miss - fetch from database
    user = db.query("SELECT * FROM users WHERE id = ?", user_id)
    
    # Store in cache (expire after 1 hour)
    cache.setex(f"user:{user_id}", 3600, json.dumps(user))
    
    return user
```

### HTTP Caching

```python
from flask import Flask, make_response

@app.route('/api/data')
def get_data():
    response = make_response(data)
    
    # Cache for 1 hour
    response.headers['Cache-Control'] = 'public, max-age=3600'
    
    # ETag for conditional requests
    response.headers['ETag'] = generate_etag(data)
    
    return response
```

## 2. Database Optimization

### Add Indexes

```sql
-- Find missing indexes
SELECT * FROM pg_stat_user_tables WHERE idx_scan = 0;

-- Add index on frequently queried columns
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_id ON orders(user_id);

-- Composite index for multi-column queries
CREATE INDEX idx_orders_user_status ON orders(user_id, status);
```

### Optimize Queries

```sql
-- Bad: N+1 query problem
SELECT * FROM users;
-- Then for each user:
SELECT * FROM orders WHERE user_id = ?;

-- Good: JOIN
SELECT users.*, orders.*
FROM users
LEFT JOIN orders ON orders.user_id = users.id;

-- Bad: SELECT *
SELECT * FROM users WHERE id = 1;

-- Good: SELECT only needed columns
SELECT id, name, email FROM users WHERE id = 1;
```

### Connection Pooling

```python
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

# Connection pool (reuse connections)
engine = create_engine(
    'postgresql://user:pass@localhost/db',
    poolclass=QueuePool,
    pool_size=10,        # 10 connections
    max_overflow=20,     # +20 overflow
    pool_timeout=30,     # 30s timeout
    pool_recycle=3600    # Recycle after 1 hour
)
```

## 3. Async Operations

### Python (asyncio)

```python
import asyncio
import aiohttp

async def fetch_user(session, user_id):
    async with session.get(f'/api/users/{user_id}') as response:
        return await response.json()

async def fetch_all_users(user_ids):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_user(session, uid) for uid in user_ids]
        return await asyncio.gather(*tasks)

# Fetch 100 users in parallel (not sequential)
users = asyncio.run(fetch_all_users(range(1, 101)))
```

### Node.js (Promise.all)

```javascript
// Bad: Sequential (slow)
for (const userId of userIds) {
  const user = await fetchUser(userId);
  users.push(user);
}

// Good: Parallel (fast)
const users = await Promise.all(
  userIds.map(userId => fetchUser(userId))
);
```

## 4. Lazy Loading

```python
# Bad: Load everything upfront
users = User.query.all()  # Loads all users into memory

# Good: Paginate
users = User.query.limit(100).offset(0).all()  # Load 100 at a time

# Good: Stream large results
for user in User.query.yield_per(100):
    process(user)  # Process one at a time
```

## 5. Compression

```python
from flask import Flask
from flask_compress import Compress

app = Flask(__name__)
Compress(app)  # Gzip responses automatically

# Reduces response size by 70-90%
```
```

**Frontend Optimization:**

```markdown
## 1. Code Splitting

### React (lazy loading)

```javascript
import React, { lazy, Suspense } from 'react';

// Bad: Bundle everything
import HeavyComponent from './HeavyComponent';

// Good: Lazy load
const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

### Webpack (code splitting)

```javascript
// webpack.config.js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10
        }
      }
    }
  }
};
```

## 2. Image Optimization

```html
<!-- Bad: Large PNG -->
<img src="photo.png" alt="Photo" />

<!-- Good: WebP with fallback -->
<picture>
  <source srcset="photo.webp" type="image/webp" />
  <img src="photo.jpg" alt="Photo" loading="lazy" />
</picture>

<!-- Responsive images -->
<img 
  srcset="photo-320w.jpg 320w,
          photo-640w.jpg 640w,
          photo-1280w.jpg 1280w"
  sizes="(max-width: 320px) 280px,
         (max-width: 640px) 600px,
         1200px"
  src="photo-640w.jpg"
  alt="Photo"
  loading="lazy"
/>
```

## 3. Lazy Loading

```javascript
// Intersection Observer for lazy loading
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      observer.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  observer.observe(img);
});
```

## 4. Minification & Compression

```bash
# Minify JavaScript
npx terser input.js -o output.min.js

# Minify CSS
npx csso input.css -o output.min.css

# Gzip compression (server-side)
# nginx.conf
gzip on;
gzip_types text/plain text/css application/json application/javascript;
gzip_min_length 1000;
```

## 5. CDN for Static Assets

```html
<!-- Bad: Serve from your server -->
<script src="/js/app.js"></script>

<!-- Good: Serve from CDN -->
<script src="https://cdn.example.com/js/app.js"></script>

<!-- Even better: Use CDN for libraries -->
<script src="https://cdn.jsdelivr.net/npm/react@18/umd/react.production.min.js"></script>
```

## 6. Debouncing & Throttling

```javascript
// Debounce: Wait until user stops typing
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// Usage: Search as user types
const search = debounce((query) => {
  fetch(`/api/search?q=${query}`);
}, 300);

input.addEventListener('input', (e) => search(e.target.value));

// Throttle: Limit execution rate
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// Usage: Scroll event
const handleScroll = throttle(() => {
  console.log('Scrolled');
}, 100);

window.addEventListener('scroll', handleScroll);
```
```

---

## Phase 4: Verify

### Goal: Measure improvement

**Before vs After Comparison:**

```markdown
## Performance Comparison

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| **Response Time (P95)** | 800ms | 150ms | 81% faster |
| **Throughput** | 100 RPS | 500 RPS | 5x increase |
| **Database Queries** | 50 per request | 5 per request | 90% reduction |
| **Bundle Size** | 500KB | 150KB | 70% smaller |
| **LCP** | 4.2s | 1.8s | 57% faster |
| **Memory Usage** | 2GB | 500MB | 75% reduction |

**Cost Impact:**
- Server costs: $500/mo → $100/mo (80% reduction)
- CDN costs: $200/mo → $50/mo (75% reduction)
- **Total savings:** $550/mo = $6,600/year
```

**Regression Testing:**

```bash
# Run load test before optimization
wrk -t4 -c100 -d30s http://localhost:3000 > before.txt

# Apply optimizations

# Run load test after optimization
wrk -t4 -c100 -d30s http://localhost:3000 > after.txt

# Compare results
diff before.txt after.txt
```

---

## Phase 5: Monitor

### Goal: Track performance over time

**Monitoring Setup:**

```markdown
## Application Performance Monitoring (APM)

### Open Source
- **Prometheus + Grafana:** Metrics and dashboards
- **Jaeger:** Distributed tracing
- **ELK Stack:** Logs and analytics

### Commercial
- **New Relic:** Full-stack monitoring
- **Datadog:** Infrastructure and APM
- **Sentry:** Error tracking and performance

## Key Metrics to Track

### Backend
- Response time (P50, P95, P99)
- Throughput (RPS)
- Error rate (%)
- CPU usage (%)
- Memory usage (MB)
- Database query time (ms)

### Frontend
- Core Web Vitals (LCP, FID, CLS, INP)
- Page load time (s)
- Bundle size (KB)
- API latency (ms)

### Business
- Conversion rate (%)
- Bounce rate (%)
- Revenue per user ($)
```

**Alerting:**

```yaml
# Prometheus alert rules
groups:
  - name: performance
    rules:
      - alert: HighResponseTime
        expr: http_request_duration_seconds{quantile="0.95"} > 0.5
        for: 5m
        annotations:
          summary: "High response time (P95 > 500ms)"
      
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.01
        for: 5m
        annotations:
          summary: "High error rate (>1%)"
```

---

## Performance Optimization Checklist

**Backend:**
- [ ] Add caching (Redis, Memcached)
- [ ] Optimize database queries (indexes, JOINs)
- [ ] Use connection pooling
- [ ] Enable compression (gzip)
- [ ] Implement async operations
- [ ] Add pagination for large datasets
- [ ] Use CDN for static assets

**Frontend:**
- [ ] Code splitting (lazy loading)
- [ ] Optimize images (WebP, lazy loading)
- [ ] Minify JavaScript and CSS
- [ ] Enable browser caching
- [ ] Use CDN for libraries
- [ ] Debounce/throttle expensive operations
- [ ] Reduce bundle size (<200KB)

**Database:**
- [ ] Add indexes on frequently queried columns
- [ ] Optimize slow queries (EXPLAIN ANALYZE)
- [ ] Use connection pooling
- [ ] Enable query caching
- [ ] Denormalize for read-heavy workloads
- [ ] Archive old data

**Monitoring:**
- [ ] Set up APM (New Relic, Datadog, or Prometheus)
- [ ] Track Core Web Vitals
- [ ] Monitor error rates
- [ ] Set up alerts for performance degradation
- [ ] Create performance dashboards

---

## Common Performance Mistakes

### Mistake 1: Premature Optimization
**Problem:** Optimizing before measuring

**Solution:** Measure first, optimize bottlenecks only

### Mistake 2: Optimizing the Wrong Thing
**Problem:** Spending time on non-bottlenecks

**Solution:** Profile to find real bottlenecks

### Mistake 3: No Caching
**Problem:** Recomputing same results repeatedly

**Solution:** Cache expensive operations

### Mistake 4: N+1 Query Problem
**Problem:** Querying database in a loop

**Solution:** Use JOINs or batch queries

### Mistake 5: Large Bundle Sizes
**Problem:** Shipping entire app upfront

**Solution:** Code splitting and lazy loading

### Mistake 6: No Monitoring
**Problem:** Can't detect performance regressions

**Solution:** Set up APM and alerts

---

## Integration with Other Skills

**Use before optimization:**
- `systematic-debugging` - Find performance bugs
- `architecture-blueprint` - Design for performance

**Use during optimization:**
- `test-driven-development` - Ensure optimizations don't break functionality
- `requesting-code-review` - Review performance changes

**Use after optimization:**
- `web-performance` - Frontend-specific optimizations
- `database-optimization` - Deep database tuning

---

**Remember:** Performance optimization is iterative. Measure, optimize, verify, monitor. Focus on user-perceived performance (Core Web Vitals) over vanity metrics. A 100ms improvement in load time can increase conversion by 1%.
