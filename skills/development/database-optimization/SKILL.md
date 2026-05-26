---
name: database-optimization
description: Optimize database performance through indexing, query optimization, schema design, connection pooling, and caching. Achieve fast queries and efficient data storage.
tags: [database, optimization, indexing, queries, sql, postgresql, mysql, performance]
version: 1.0.0
author: Enhanced from database optimization best practices)
---

# Database Optimization

## Overview

Optimize database performance for fast, efficient data access.

**Why database optimization matters:**
- **Speed:** Fast queries = fast application
- **Scalability:** Handle more users/data
- **Cost:** Reduce server resources
- **Reliability:** Prevent timeouts/crashes
- **User experience:** No waiting

**Key principles:**
- **Measure first:** Profile before optimizing
- **Index strategically:** Not every column
- **Optimize queries:** N+1 is the enemy
- **Design for scale:** Think ahead
- **Cache wisely:** Reduce database load

## When to Use

**Use for:**
- Slow queries (> 100ms)
- High database load
- Scaling issues
- Cost optimization
- Performance bottlenecks

**Critical for:**
- High-traffic applications
- Large datasets (1M+ rows)
- Complex queries
- Real-time features
- Multi-tenant systems

## The 7-Layer Database Optimization Framework

```
┌─────────────────────────────────────────────────────────┐
│       DATABASE OPTIMIZATION FRAMEWORK                    │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   LAYER 1    │  Measure
    │   MEASURE    │  - Slow query log
    │              │  - EXPLAIN
    └──────┬───────┘  - Monitoring
           │
           ▼
    ┌──────────────┐
    │   LAYER 2    │  Indexing
    │   INDEXING   │  - B-tree
    │              │  - Composite
    └──────┬───────┘  - Partial
           │
           ▼
    ┌──────────────┐
    │   LAYER 3    │  Query Optimization
    │   QUERIES    │  - N+1 prevention
    │              │  - JOINs
    └──────┬───────┘  - Subqueries
           │
           ▼
    ┌──────────────┐
    │   LAYER 4    │  Schema Design
    │   SCHEMA     │  - Normalization
    │              │  - Denormalization
    └──────┬───────┘  - Partitioning
           │
           ▼
    ┌──────────────┐
    │   LAYER 5    │  Caching
    │   CACHING    │  - Query cache
    │              │  - Application cache
    └──────┬───────┘  - CDN
           │
           ▼
    ┌──────────────┐
    │   LAYER 6    │  Connection Management
    │ CONNECTION   │  - Pooling
    │              │  - Timeouts
    └──────┬───────┘  - Limits
           │
           ▼
    ┌──────────────┐
    │   LAYER 7    │  Scaling
    │   SCALING    │  - Read replicas
    │              │  - Sharding
    └──────────────┘  - Vertical/horizontal
```

## Layer 1: Measure Performance

### Goal: Identify slow queries

**Enable Slow Query Log (PostgreSQL):**

```sql
-- postgresql.conf
log_min_duration_statement = 100  -- Log queries > 100ms
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
log_statement = 'all'  -- Or 'ddl', 'mod', 'none'

-- Reload config
SELECT pg_reload_conf();

-- View slow queries
SELECT
  query,
  calls,
  total_time,
  mean_time,
  max_time
FROM pg_stat_statements
ORDER BY mean_time DESC
LIMIT 20;
```

**Enable Slow Query Log (MySQL):**

```sql
-- my.cnf
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow-query.log
long_query_time = 0.1  -- 100ms

-- Enable at runtime
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 0.1;

-- View slow queries
SELECT * FROM mysql.slow_log
ORDER BY query_time DESC
LIMIT 20;
```

**EXPLAIN (Query Execution Plan):**

```sql
-- PostgreSQL
EXPLAIN ANALYZE
SELECT u.name, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at > '2024-01-01'
GROUP BY u.id, u.name
ORDER BY order_count DESC
LIMIT 10;

-- Output shows:
-- - Seq Scan vs Index Scan
-- - Execution time
-- - Rows scanned
-- - Cost estimates

-- MySQL
EXPLAIN
SELECT u.name, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at > '2024-01-01'
GROUP BY u.id, u.name
ORDER BY order_count DESC
LIMIT 10;
```

**Key EXPLAIN Metrics:**

```markdown
## PostgreSQL
- **Seq Scan:** Full table scan (slow for large tables)
- **Index Scan:** Using index (fast)
- **Bitmap Heap Scan:** Multiple index lookups
- **Nested Loop:** Join method (good for small datasets)
- **Hash Join:** Join method (good for large datasets)
- **Cost:** Estimated cost (lower is better)
- **Rows:** Estimated rows returned
- **Actual time:** Real execution time

## MySQL
- **type:**
  - `ALL`: Full table scan (worst)
  - `index`: Full index scan
  - `range`: Index range scan
  - `ref`: Index lookup
  - `eq_ref`: Unique index lookup
  - `const`: Constant lookup (best)
- **rows:** Rows examined
- **Extra:** Additional info (Using filesort, Using temporary)
```

**Monitoring Tools:**

```bash
# pg_stat_statements (PostgreSQL)
CREATE EXTENSION pg_stat_statements;

# View top queries by total time
SELECT
  query,
  calls,
  total_time,
  mean_time,
  stddev_time,
  rows
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 20;

# pgBadger (PostgreSQL log analyzer)
pgbadger /var/log/postgresql/postgresql.log -o report.html

# pt-query-digest (MySQL)
pt-query-digest /var/log/mysql/slow-query.log

# Application monitoring
# - New Relic
# - Datadog
# - AppDynamics
```

---

## Layer 2: Indexing

### Goal: Fast data lookups

**Index Types:**

```sql
-- B-tree (default, most common)
CREATE INDEX idx_users_email ON users(email);

-- Unique index (enforce uniqueness)
CREATE UNIQUE INDEX idx_users_email ON users(email);

-- Composite index (multiple columns)
CREATE INDEX idx_orders_user_date ON orders(user_id, created_at);

-- Partial index (filtered)
CREATE INDEX idx_active_users ON users(email) WHERE active = true;

-- Expression index (computed values)
CREATE INDEX idx_users_lower_email ON users(LOWER(email));

-- Full-text search (PostgreSQL)
CREATE INDEX idx_posts_search ON posts USING GIN(to_tsvector('english', content));

-- GiST/GIN (arrays, JSON, full-text)
CREATE INDEX idx_tags ON posts USING GIN(tags);

-- Hash index (equality only, PostgreSQL 10+)
CREATE INDEX idx_users_id_hash ON users USING HASH(id);
```

**When to Index:**

```markdown
## ✅ Index These
- Primary keys (automatic)
- Foreign keys
- Columns in WHERE clauses
- Columns in JOIN conditions
- Columns in ORDER BY
- Columns in GROUP BY
- Unique constraints

## ❌ Don't Index These
- Small tables (< 1000 rows)
- Columns with low cardinality (few unique values)
- Columns rarely queried
- Columns frequently updated
- Very large text columns
```

**Composite Index Order:**

```sql
-- ✅ Good: Most selective column first
CREATE INDEX idx_orders_status_user_date
ON orders(status, user_id, created_at);

-- Supports queries:
-- WHERE status = 'pending'
-- WHERE status = 'pending' AND user_id = 123
-- WHERE status = 'pending' AND user_id = 123 AND created_at > '2024-01-01'

-- ❌ Bad: Least selective first
CREATE INDEX idx_orders_date_user_status
ON orders(created_at, user_id, status);

-- Only supports:
-- WHERE created_at > '2024-01-01'
-- Not: WHERE status = 'pending' (can't use index)
```

**Index Maintenance:**

```sql
-- PostgreSQL: Rebuild bloated indexes
REINDEX INDEX idx_users_email;
REINDEX TABLE users;

-- MySQL: Optimize table (rebuilds indexes)
OPTIMIZE TABLE users;

-- Check index usage (PostgreSQL)
SELECT
  schemaname,
  tablename,
  indexname,
  idx_scan,
  idx_tup_read,
  idx_tup_fetch
FROM pg_stat_user_indexes
WHERE idx_scan = 0  -- Unused indexes
ORDER BY pg_relation_size(indexrelid) DESC;

-- Drop unused indexes
DROP INDEX idx_unused;
```

---

## Layer 3: Query Optimization

### Goal: Efficient queries

**N+1 Query Problem:**

```python
# ❌ Bad: N+1 queries (1 + N)
users = User.query.all()  # 1 query
for user in users:
    orders = user.orders.all()  # N queries (one per user)
    print(f"{user.name}: {len(orders)} orders")

# ✅ Good: Single query with JOIN
users = User.query.options(
    joinedload(User.orders)
).all()  # 1 query
for user in users:
    print(f"{user.name}: {len(user.orders)} orders")

# SQL equivalent
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

**SELECT Only What You Need:**

```sql
-- ❌ Bad: SELECT *
SELECT * FROM users WHERE id = 123;

-- ✅ Good: Specific columns
SELECT id, name, email FROM users WHERE id = 123;

-- ❌ Bad: Fetching all rows
SELECT * FROM orders;

-- ✅ Good: LIMIT results
SELECT * FROM orders ORDER BY created_at DESC LIMIT 100;
```

**JOIN Optimization:**

```sql
-- ✅ Good: INNER JOIN (only matching rows)
SELECT u.name, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.status = 'completed';

-- ❌ Bad: Subquery in SELECT (runs for each row)
SELECT
  u.name,
  (SELECT COUNT(*) FROM orders WHERE user_id = u.id) as order_count
FROM users u;

-- ✅ Good: JOIN with GROUP BY
SELECT u.name, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.name;

-- ✅ Good: Use EXISTS instead of IN for large datasets
SELECT * FROM users u
WHERE EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id AND o.status = 'completed'
);

-- vs (slower for large datasets)
SELECT * FROM users
WHERE id IN (SELECT user_id FROM orders WHERE status = 'completed');
```

**Avoid Functions on Indexed Columns:**

```sql
-- ❌ Bad: Function prevents index usage
SELECT * FROM users WHERE LOWER(email) = 'user@example.com';

-- ✅ Good: Use expression index
CREATE INDEX idx_users_lower_email ON users(LOWER(email));
SELECT * FROM users WHERE LOWER(email) = 'user@example.com';

-- ✅ Better: Store lowercase in application
SELECT * FROM users WHERE email = 'user@example.com';
```

**Pagination:**

```sql
-- ❌ Bad: OFFSET (scans all skipped rows)
SELECT * FROM posts
ORDER BY created_at DESC
LIMIT 20 OFFSET 10000;  -- Scans 10,020 rows

-- ✅ Good: Keyset pagination (cursor-based)
SELECT * FROM posts
WHERE created_at < '2024-01-01 12:00:00'
ORDER BY created_at DESC
LIMIT 20;

-- Next page: Use last created_at from previous page
SELECT * FROM posts
WHERE created_at < '2023-12-31 11:30:00'
ORDER BY created_at DESC
LIMIT 20;
```

---

## Layer 4: Schema Design

### Goal: Efficient data structure

**Normalization (Reduce Redundancy):**

```sql
-- ❌ Bad: Denormalized (redundant data)
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  user_name VARCHAR(255),
  user_email VARCHAR(255),
  user_address TEXT,
  product_name VARCHAR(255),
  product_price DECIMAL(10, 2),
  quantity INT
);

-- ✅ Good: Normalized (3NF)
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255) UNIQUE,
  address TEXT
);

CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255),
  price DECIMAL(10, 2)
);

CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  user_id INT REFERENCES users(id),
  product_id INT REFERENCES products(id),
  quantity INT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Denormalization (Performance):**

```sql
-- When to denormalize:
-- - Read-heavy workloads
-- - Expensive JOINs
-- - Aggregations

-- Example: Cache order count on user
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255),
  order_count INT DEFAULT 0  -- Denormalized
);

-- Update with trigger
CREATE OR REPLACE FUNCTION update_user_order_count()
RETURNS TRIGGER AS $$
BEGIN
  IF TG_OP = 'INSERT' THEN
    UPDATE users SET order_count = order_count + 1 WHERE id = NEW.user_id;
  ELSIF TG_OP = 'DELETE' THEN
    UPDATE users SET order_count = order_count - 1 WHERE id = OLD.user_id;
  END IF;
  RETURN NULL;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_update_order_count
AFTER INSERT OR DELETE ON orders
FOR EACH ROW EXECUTE FUNCTION update_user_order_count();
```

**Partitioning (Large Tables):**

```sql
-- PostgreSQL: Range partitioning by date
CREATE TABLE orders (
  id SERIAL,
  user_id INT,
  total DECIMAL(10, 2),
  created_at TIMESTAMP
) PARTITION BY RANGE (created_at);

-- Create partitions
CREATE TABLE orders_2024_q1 PARTITION OF orders
FOR VALUES FROM ('2024-01-01') TO ('2024-04-01');

CREATE TABLE orders_2024_q2 PARTITION OF orders
FOR VALUES FROM ('2024-04-01') TO ('2024-07-01');

-- Queries automatically use correct partition
SELECT * FROM orders WHERE created_at >= '2024-02-01' AND created_at < '2024-03-01';

-- List partitioning (by category)
CREATE TABLE products (
  id SERIAL,
  name VARCHAR(255),
  category VARCHAR(50)
) PARTITION BY LIST (category);

CREATE TABLE products_electronics PARTITION OF products
FOR VALUES IN ('electronics', 'computers');

CREATE TABLE products_clothing PARTITION OF products
FOR VALUES IN ('clothing', 'shoes');
```

**Data Types:**

```sql
-- ✅ Use appropriate types
CREATE TABLE users (
  id SERIAL PRIMARY KEY,  -- Auto-increment integer
  email VARCHAR(255),     -- Variable length (not CHAR)
  age SMALLINT,           -- 2 bytes (vs INT 4 bytes)
  balance DECIMAL(10, 2), -- Exact decimal (not FLOAT)
  is_active BOOLEAN,      -- Boolean (not TINYINT)
  created_at TIMESTAMP,   -- Timestamp (not VARCHAR)
  metadata JSONB          -- JSONB (not TEXT)
);

-- ❌ Avoid
-- - CHAR (fixed length, wastes space)
-- - TEXT for short strings
-- - VARCHAR without length
-- - FLOAT for money (precision issues)
```

---

## Layer 5: Caching

### Goal: Reduce database load

**Application-Level Caching (Redis):**

```python
import redis
import json

cache = redis.Redis(host='localhost', port=6379, db=0)

def get_user(user_id):
    # Try cache first
    cache_key = f"user:{user_id}"
    cached = cache.get(cache_key)
    
    if cached:
        return json.loads(cached)
    
    # Cache miss: query database
    user = db.query(User).filter(User.id == user_id).first()
    
    # Store in cache (expire after 1 hour)
    cache.setex(cache_key, 3600, json.dumps(user.to_dict()))
    
    return user

# Invalidate cache on update
def update_user(user_id, data):
    user = db.query(User).filter(User.id == user_id).first()
    user.update(data)
    db.commit()
    
    # Invalidate cache
    cache.delete(f"user:{user_id}")
```

**Query Result Caching:**

```python
# Flask-Caching
from flask_caching import Cache

cache = Cache(config={'CACHE_TYPE': 'redis'})

@cache.cached(timeout=300, key_prefix='all_users')
def get_all_users():
    return User.query.all()

# Invalidate on change
@cache.delete('all_users')
def create_user(data):
    user = User(**data)
    db.session.add(user)
    db.session.commit()
```

**Database Query Cache (MySQL):**

```sql
-- MySQL query cache (deprecated in 8.0)
-- Use application-level caching instead

-- Check query cache status
SHOW VARIABLES LIKE 'query_cache%';

-- Enable (MySQL < 8.0)
SET GLOBAL query_cache_type = ON;
SET GLOBAL query_cache_size = 67108864;  -- 64MB
```

---

## Layer 6: Connection Management

### Goal: Efficient connection usage

**Connection Pooling:**

```python
# SQLAlchemy (Python)
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

engine = create_engine(
    'postgresql://user:pass@localhost/db',
    poolclass=QueuePool,
    pool_size=10,          # Max connections
    max_overflow=20,       # Extra connections when pool full
    pool_timeout=30,       # Wait time for connection
    pool_recycle=3600,     # Recycle connections after 1 hour
    pool_pre_ping=True     # Check connection before use
)

# Node.js (pg)
const { Pool } = require('pg');

const pool = new Pool({
  host: 'localhost',
  database: 'mydb',
  max: 20,                // Max connections
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

// Use pool
const result = await pool.query('SELECT * FROM users WHERE id = $1', [123]);
```

**Connection Limits:**

```sql
-- PostgreSQL: Check current connections
SELECT count(*) FROM pg_stat_activity;

-- Set max connections
ALTER SYSTEM SET max_connections = 200;
SELECT pg_reload_conf();

-- Kill idle connections
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE state = 'idle'
AND state_change < NOW() - INTERVAL '10 minutes';

-- MySQL: Check connections
SHOW PROCESSLIST;
SHOW STATUS LIKE 'Threads_connected';

-- Set max connections
SET GLOBAL max_connections = 200;
```

---

## Layer 7: Scaling

### Goal: Handle growth

**Read Replicas:**

```markdown
## Setup
1. **Primary (write):** Handles all writes
2. **Replicas (read):** Handle reads

## Benefits
- Distribute read load
- Geographic distribution
- Backup/disaster recovery

## Implementation
```

```python
# SQLAlchemy: Read/write splitting
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# Primary (write)
primary_engine = create_engine('postgresql://primary/db')

# Replica (read)
replica_engine = create_engine('postgresql://replica/db')

# Session with routing
class RoutingSession(Session):
    def get_bind(self, mapper=None, clause=None):
        if self._flushing:
            return primary_engine
        else:
            return replica_engine

# Usage
session = RoutingSession()
users = session.query(User).all()  # Uses replica
session.add(new_user)
session.commit()  # Uses primary
```

**Sharding (Horizontal Partitioning):**

```markdown
## Sharding Strategies

### 1. Range-based (by ID)
- Shard 1: users 1-1M
- Shard 2: users 1M-2M
- Shard 3: users 2M-3M

### 2. Hash-based (by user_id)
- shard = user_id % num_shards
- Even distribution

### 3. Geographic
- US users → US shard
- EU users → EU shard

### 4. Tenant-based (multi-tenant)
- Each customer → separate shard
```

**Vertical Scaling:**

```markdown
## Increase Resources
- More CPU
- More RAM
- Faster disk (SSD → NVMe)

## Limits
- Hardware limits
- Expensive
- Single point of failure
```

**Horizontal Scaling:**

```markdown
## Add More Servers
- Read replicas
- Sharding
- Load balancing

## Benefits
- No hardware limits
- Cost-effective
- High availability
```

---

## Database Optimization Checklist

**Measurement:**
- [ ] Slow query log enabled
- [ ] EXPLAIN analyzed for slow queries
- [ ] Monitoring in place (pg_stat_statements)
- [ ] Baseline metrics recorded

**Indexing:**
- [ ] Primary keys indexed
- [ ] Foreign keys indexed
- [ ] WHERE clause columns indexed
- [ ] Composite indexes for multi-column queries
- [ ] Unused indexes removed

**Queries:**
- [ ] N+1 queries eliminated
- [ ] SELECT only needed columns
- [ ] LIMIT used for large result sets
- [ ] Pagination optimized (keyset vs offset)
- [ ] JOINs optimized

**Schema:**
- [ ] Appropriate data types
- [ ] Normalization applied (3NF)
- [ ] Denormalization for performance (if needed)
- [ ] Partitioning for large tables (if needed)

**Caching:**
- [ ] Application-level cache (Redis)
- [ ] Query result caching
- [ ] Cache invalidation strategy

**Connections:**
- [ ] Connection pooling configured
- [ ] Pool size appropriate
- [ ] Connection limits set
- [ ] Idle connections cleaned up

**Scaling:**
- [ ] Read replicas (if needed)
- [ ] Sharding strategy (if needed)
- [ ] Monitoring and alerts

---

## Common Database Mistakes

### Mistake 1: No Indexes
**Problem:** Full table scans on large tables

**Solution:** Index WHERE, JOIN, ORDER BY columns

### Mistake 2: Over-Indexing
**Problem:** Slow writes, wasted space

**Solution:** Only index frequently queried columns

### Mistake 3: SELECT *
**Problem:** Fetching unnecessary data

**Solution:** SELECT only needed columns

### Mistake 4: N+1 Queries
**Problem:** 1 + N queries instead of 1

**Solution:** Use JOINs or eager loading

### Mistake 5: No Connection Pooling
**Problem:** Opening/closing connections for each query

**Solution:** Use connection pool

---

## Integration with Other Skills

**Use before optimizing:**
- `architecture-blueprint` - Database architecture
- `performance-optimization` - Application performance

**Use during optimization:**
- `systematic-debugging` - Debug slow queries
- `test-driven-development` - Performance tests

**Use after optimization:**
- `requesting-code-review` - Review queries
- `web-performance` - End-to-end performance

---

**Remember:** Measure first, optimize what matters. Indexes are powerful but not free. N+1 is the enemy. Cache wisely. Scale when needed, not before.
