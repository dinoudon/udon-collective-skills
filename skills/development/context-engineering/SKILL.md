---
name: context-engineering
description: Load exactly the right context at each step to maximize effectiveness and minimize token waste. Master file reading strategies, search patterns, and context window management for AI coding agents.
tags: [context, efficiency, tokens, search, files, optimization]
version: 1.0.0
author: Enhanced from token optimization research)
---

# Context Engineering

## Overview

Load exactly the right context at each step. No more, no less.

**The problem:**
- AI agents have limited context windows (200K tokens)
- Reading entire codebases wastes tokens
- Missing critical context causes errors
- Poor context = poor decisions

**The solution:**
- Strategic file reading (read what you need, when you need it)
- Targeted searching (find before reading)
- Context layering (overview → detail → implementation)
- Just-in-time loading (load right before use)

**Benefits:**
- 10x more efficient token usage
- Faster responses (less to process)
- Better decisions (right context, not noise)
- Longer sessions (context doesn't fill up)

## When to Use

**Always use when:**
- Working with large codebases (100+ files)
- Implementing features across multiple files
- Debugging issues in unfamiliar code
- Refactoring existing systems
- Reviewing pull requests

**Especially important when:**
- Context window is filling up
- You need to work on multiple features
- Codebase is poorly documented
- You're new to the project

## The 4-Layer Context Strategy

```
┌─────────────────────────────────────────────────────────┐
│              CONTEXT LOADING LAYERS                      │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   LAYER 1    │  Project Overview
    │   OVERVIEW   │  - README, package.json
    │              │  - Directory structure
    └──────┬───────┘  - Tech stack
           │
           ▼
    ┌──────────────┐
    │   LAYER 2    │  Relevant Files
    │   DISCOVERY  │  - Search for keywords
    │              │  - Find related files
    └──────┬───────┘  - Identify dependencies
           │
           ▼
    ┌──────────────┐
    │   LAYER 3    │  Targeted Reading
    │    DETAIL    │  - Read specific files
    │              │  - Focus on relevant sections
    └──────┬───────┘  - Skip boilerplate
           │
           ▼
    ┌──────────────┐
    │   LAYER 4    │  Implementation
    │ IMPLEMENTATION│ - Load exact context needed
    │              │  - Make changes
    └──────────────┘  - Verify
```

## Layer 1: Project Overview

### Goal: Understand the project structure in < 1000 tokens

**What to read:**
1. `README.md` (first 100 lines)
2. `package.json` or equivalent (dependencies, scripts)
3. Directory structure (top 2 levels)
4. Main entry point (first 50 lines)

**Commands:**

```bash
# Read README (overview only)
read_file path=README.md limit=100

# Read package.json (full file, usually small)
read_file path=package.json

# List directory structure (2 levels)
search_files pattern="*" target=files path=. | head -50

# Read main entry point (overview)
read_file path=src/index.ts limit=50
```

**What you learn:**
- Tech stack (React, Node.js, PostgreSQL, etc.)
- Project structure (src/, tests/, docs/)
- Available commands (npm run dev, npm test)
- Key dependencies (express, prisma, zod)

**Example output:**

```markdown
## Project Overview (from README + package.json)

**Tech Stack:**
- Framework: Next.js 14
- Language: TypeScript 5
- Database: PostgreSQL (via Prisma)
- Styling: Tailwind CSS
- Testing: Vitest + Playwright

**Structure:**
```
src/
  app/          # Next.js app directory (routes)
  components/   # React components
  lib/          # Utilities and helpers
  types/        # TypeScript types
tests/
  unit/         # Unit tests
  e2e/          # End-to-end tests
```

**Commands:**
- `npm run dev` - Start dev server
- `npm test` - Run tests
- `npm run build` - Production build

**Key Dependencies:**
- next-auth (authentication)
- zod (validation)
- react-query (data fetching)
```

**Token cost:** ~500-1000 tokens

---

## Layer 2: Discovery (Find Before Reading)

### Goal: Locate relevant files without reading everything

**Use search_files, not read_file:**

```bash
# WRONG: Read every file to find what you need
read_file path=src/components/Button.tsx
read_file path=src/components/Input.tsx
read_file path=src/components/Form.tsx
# ... 50 more files

# RIGHT: Search first, read only matches
search_files pattern="authentication" target=content path=src/
```

### Search Strategies

**1. Keyword Search (Find by content)**

```bash
# Find files containing "authentication"
search_files pattern="authentication" target=content

# Find files containing "User" type
search_files pattern="interface User|type User" target=content

# Find API routes
search_files pattern="app\.(get|post|put|delete)" target=content

# Find database queries
search_files pattern="db\.|prisma\." target=content
```

**2. File Name Search (Find by name)**

```bash
# Find all test files
search_files pattern="*.test.ts" target=files

# Find all component files
search_files pattern="*Component.tsx" target=files

# Find config files
search_files pattern="*config*" target=files

# Find specific file
search_files pattern="auth*" target=files path=src/lib/
```

**3. Pattern-Based Search (Find by structure)**

```bash
# Find all exports
search_files pattern="^export (function|class|const)" target=content

# Find all imports from specific module
search_files pattern="from ['\"]@/lib/auth['\"]" target=content

# Find all TODO comments
search_files pattern="TODO:|FIXME:" target=content

# Find all error handling
search_files pattern="try \{|catch \(" target=content
```

**4. Dependency Search (Find related files)**

```bash
# Find files that import a specific module
search_files pattern="from ['\"].*User['\"]" target=content

# Find files that use a specific function
search_files pattern="hashPassword\(" target=content

# Find files that reference a specific type
search_files pattern=": User\b" target=content
```

### Search Output Analysis

**Example search result:**

```
src/lib/auth.ts:15:  export async function hashPassword(password: string) {
src/lib/auth.ts:23:  export async function verifyPassword(password: string, hash: string) {
src/api/signup.ts:8:  import { hashPassword } from '@/lib/auth';
src/api/signup.ts:18:  const hashedPassword = await hashPassword(password);
```

**What you learn:**
- `hashPassword` is defined in `src/lib/auth.ts` (line 15)
- `hashPassword` is used in `src/api/signup.ts` (line 18)
- Related function: `verifyPassword` (line 23)

**Next step:**
- Read `src/lib/auth.ts` (lines 10-30) for implementation
- Read `src/api/signup.ts` (lines 1-30) for usage

**Token cost:** ~100-200 tokens (vs 5000+ reading all files)

---

## Layer 3: Targeted Reading

### Goal: Read only what you need, skip the rest

**Reading strategies:**

### Strategy 1: Read Specific Line Ranges

```bash
# WRONG: Read entire file (1000 lines)
read_file path=src/lib/auth.ts

# RIGHT: Read only relevant section (20 lines)
read_file path=src/lib/auth.ts offset=10 limit=20
```

**When to use:**
- You know the line number (from search)
- File is large (>200 lines)
- You only need one function

### Strategy 2: Read Multiple Sections

```bash
# Read function definition (lines 15-25)
read_file path=src/lib/auth.ts offset=15 limit=10

# Read function usage (lines 100-110)
read_file path=src/lib/auth.ts offset=100 limit=10

# Read tests (lines 200-220)
read_file path=tests/auth.test.ts offset=200 limit=20
```

**When to use:**
- Function is defined in one place, used in another
- You need to see implementation + tests
- File has multiple relevant sections

### Strategy 3: Read Related Files Together

```bash
# Read interface definition
read_file path=src/types/user.ts limit=50

# Read implementation
read_file path=src/lib/user.ts offset=20 limit=30

# Read tests
read_file path=tests/user.test.ts offset=10 limit=40
```

**When to use:**
- Working on a feature spanning multiple files
- Need to understand data flow
- Implementing or fixing a bug

### Strategy 4: Progressive Reading

```bash
# Step 1: Read function signature only (first 5 lines)
read_file path=src/lib/auth.ts offset=15 limit=5

# Step 2: If needed, read full implementation (next 20 lines)
read_file path=src/lib/auth.ts offset=15 limit=25

# Step 3: If still unclear, read tests
read_file path=tests/auth.test.ts offset=50 limit=30
```

**When to use:**
- Unfamiliar code
- Complex logic
- Need to understand behavior

### What to Skip

**Always skip:**
- Generated code (`node_modules/`, `dist/`, `build/`)
- Lock files (`package-lock.json`, `yarn.lock`)
- Large data files (`.json` with >1000 lines)
- Binary files (images, videos, PDFs)
- Vendor code (third-party libraries)

**Usually skip:**
- Boilerplate (imports, exports)
- Comments (unless critical)
- Whitespace
- Duplicate code

**Example: Reading a React Component**

```typescript
// FULL FILE (200 lines, 5000 tokens)
import React, { useState, useEffect } from 'react';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
// ... 20 more imports

interface UserProfileProps {
  userId: string;
  onUpdate?: (user: User) => void;
}

export function UserProfile({ userId, onUpdate }: UserProfileProps) {
  const [user, setUser] = useState<User | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  // ... 50 lines of state management
  
  useEffect(() => {
    // ... 30 lines of data fetching
  }, [userId]);
  
  const handleSubmit = async (data: FormData) => {
    // ... 40 lines of form handling
  };
  
  return (
    <div>
      {/* ... 80 lines of JSX */}
    </div>
  );
}

// TARGETED READ (30 lines, 800 tokens)
// Read only the function signature and key logic

// Lines 25-30: Interface
interface UserProfileProps {
  userId: string;
  onUpdate?: (user: User) => void;
}

// Lines 32-40: Component signature and state
export function UserProfile({ userId, onUpdate }: UserProfileProps) {
  const [user, setUser] = useState<User | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  
// Lines 50-70: Key logic (form handling)
  const handleSubmit = async (data: FormData) => {
    const response = await updateUser(userId, data);
    setUser(response);
    onUpdate?.(response);
  };
```

**Token savings:** 4200 tokens (84% reduction)

---

## Layer 4: Implementation Context

### Goal: Load exactly what you need to make changes

**Just-in-time loading:**

```markdown
## Implementation: Add Email Validation to Signup

**Step 1: Find the signup endpoint**
```bash
search_files pattern="signup" target=content path=src/api/
# Result: src/api/signup.ts
```

**Step 2: Read the endpoint (targeted)**
```bash
read_file path=src/api/signup.ts offset=1 limit=50
# Read only the endpoint logic, skip imports
```

**Step 3: Find validation utilities**
```bash
search_files pattern="validate.*email" target=content path=src/lib/
# Result: src/lib/validation.ts
```

**Step 4: Read validation utilities**
```bash
read_file path=src/lib/validation.ts offset=10 limit=20
# Read only email validation function
```

**Step 5: Make the change**
```typescript
// Now you have exactly the context you need:
// 1. Signup endpoint structure
// 2. Existing validation patterns
// 3. Where to add new validation

// Add email validation to signup
import { validateEmail } from '@/lib/validation';

app.post('/api/signup', async (req, res) => {
  const { email, password } = req.body;
  
  // NEW: Email validation
  if (!validateEmail(email)) {
    return res.status(400).json({ error: 'Invalid email' });
  }
  
  // ... rest of signup logic
});
```

**Total tokens used:** ~1500 tokens (vs 10,000+ reading entire codebase)
```

### Context Loading Patterns

**Pattern 1: Top-Down (Overview → Detail)**

```bash
# 1. Project overview
read_file path=README.md limit=50

# 2. Find relevant area
search_files pattern="authentication" target=content

# 3. Read specific files
read_file path=src/lib/auth.ts offset=10 limit=30

# 4. Read implementation details
read_file path=src/api/login.ts offset=1 limit=50
```

**When to use:** New to the project, unfamiliar feature

---

**Pattern 2: Bottom-Up (Error → Root Cause)**

```bash
# 1. Read error message
# Error: "User not found" in src/api/users.ts:45

# 2. Read error location
read_file path=src/api/users.ts offset=40 limit=10

# 3. Find related code
search_files pattern="findUserById" target=content

# 4. Read implementation
read_file path=src/lib/user.ts offset=20 limit=30

# 5. Read database query
read_file path=src/db/queries.ts offset=50 limit=20
```

**When to use:** Debugging, fixing bugs

---

**Pattern 3: Horizontal (Related Files)**

```bash
# 1. Read type definition
read_file path=src/types/user.ts limit=30

# 2. Read model
read_file path=src/models/user.ts limit=50

# 3. Read API endpoint
read_file path=src/api/users.ts offset=10 limit=40

# 4. Read tests
read_file path=tests/users.test.ts offset=20 limit=50
```

**When to use:** Implementing features, understanding data flow

---

**Pattern 4: Dependency Chain**

```bash
# 1. Find entry point
read_file path=src/api/signup.ts offset=1 limit=20

# 2. Find imports
search_files pattern="from ['\"]@/lib/auth['\"]" target=content

# 3. Read dependencies
read_file path=src/lib/auth.ts offset=10 limit=30

# 4. Read sub-dependencies
read_file path=src/lib/crypto.ts offset=5 limit=20
```

**When to use:** Understanding complex logic, refactoring

---

## Context Window Management

### Monitoring Context Usage

**Check context size:**
```markdown
Current context: 45,000 / 200,000 tokens (22.5%)
```

**When to worry:**
- **< 50%:** Healthy, plenty of room
- **50-75%:** Moderate, be strategic
- **75-90%:** High, load only essentials
- **> 90%:** Critical, context compaction imminent

### Context Optimization Techniques

**Technique 1: Summarize and Discard**

```markdown
## After reading multiple files, summarize key points:

**Key Findings:**
- Authentication uses JWT tokens (src/lib/auth.ts)
- Tokens expire after 24 hours (src/config.ts:15)
- Refresh tokens stored in Redis (src/lib/redis.ts:30)

**Now discard the full file contents from context**
```

**Technique 2: Extract Only What You Need**

```typescript
// Instead of keeping entire file in context:
// [5000 tokens of full file]

// Extract only the relevant function:
function hashPassword(password: string): Promise<string> {
  return bcrypt.hash(password, 10);
}
// [100 tokens]
```

**Technique 3: Use References Instead of Content**

```markdown
// Instead of:
// [Full file content: 3000 tokens]

// Use:
"See src/lib/auth.ts:15-25 for hashPassword implementation"
// [20 tokens]
```

**Technique 4: Batch Related Reads**

```bash
# INEFFICIENT: Multiple separate reads
read_file path=src/types/user.ts
# [Process and discuss]
read_file path=src/models/user.ts
# [Process and discuss]
read_file path=src/api/users.ts
# [Process and discuss]

# EFFICIENT: Batch read, process together
read_file path=src/types/user.ts limit=30
read_file path=src/models/user.ts limit=50
read_file path=src/api/users.ts offset=10 limit=40
# [Process all together, extract key points, discard details]
```

---

## Context Engineering Checklist

**Before reading any file:**
- [ ] Do I really need this file?
- [ ] Can I search instead of reading?
- [ ] Do I need the whole file or just a section?
- [ ] Have I already read something similar?

**When searching:**
- [ ] Used specific keywords (not generic)
- [ ] Searched in relevant directory only
- [ ] Limited results to manageable number
- [ ] Analyzed results before reading files

**When reading:**
- [ ] Read only relevant sections (offset + limit)
- [ ] Skipped boilerplate (imports, comments)
- [ ] Extracted key information
- [ ] Summarized findings

**After reading:**
- [ ] Summarized key points
- [ ] Discarded unnecessary details
- [ ] Noted file locations for future reference
- [ ] Checked context usage

---

## Common Mistakes

### Mistake 1: Reading Everything

**Problem:** Reading entire codebase "just in case"

**Solution:** Search first, read only matches

### Mistake 2: Reading Full Files

**Problem:** Reading 1000-line files when you need 20 lines

**Solution:** Use offset + limit to read specific sections

### Mistake 3: Re-reading Same Files

**Problem:** Reading the same file multiple times

**Solution:** Summarize key points, reference file location

### Mistake 4: Not Using Search

**Problem:** Reading files one by one to find something

**Solution:** Use search_files to locate before reading

### Mistake 5: Ignoring Context Usage

**Problem:** Context fills up, session ends prematurely

**Solution:** Monitor context usage, optimize proactively

---

## Integration with Other Skills

**Use with:**
- `incremental-implementation` - Load context for each step
- `systematic-debugging` - Find root cause efficiently
- `github-code-review` - Review only changed files
- `parallel-orchestration` - Provide minimal context to each agent

---

## Real-World Example

### Task: Add Rate Limiting to API Endpoints

**Traditional Approach (Inefficient):**
```bash
# Read entire codebase
read_file path=src/api/users.ts
read_file path=src/api/posts.ts
read_file path=src/api/comments.ts
# ... 20 more files
# Total: 50,000 tokens
```

**Context Engineering Approach (Efficient):**

```bash
# Step 1: Search for existing rate limiting (200 tokens)
search_files pattern="rate.*limit|rateLimit" target=content path=src/

# Result: No existing rate limiting found

# Step 2: Search for middleware pattern (200 tokens)
search_files pattern="app\.use\(|middleware" target=content path=src/

# Result: src/api/index.ts uses middleware pattern

# Step 3: Read middleware setup (500 tokens)
read_file path=src/api/index.ts offset=1 limit=30

# Step 4: Search for API endpoints (200 tokens)
search_files pattern="app\.(get|post|put|delete)" target=content path=src/api/

# Result: 5 endpoint files found

# Step 5: Read one endpoint as example (300 tokens)
read_file path=src/api/users.ts offset=10 limit=20

# Total: 1,400 tokens (97% reduction)
```

**Implementation:**
```typescript
// Now you have exactly what you need:
// 1. Middleware pattern (from src/api/index.ts)
// 2. Endpoint structure (from src/api/users.ts)
// 3. No existing rate limiting (from search)

// Add rate limiting middleware
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

---

**Remember:** Context is your most valuable resource. Use it wisely. Search before reading, read only what you need, summarize and discard.
