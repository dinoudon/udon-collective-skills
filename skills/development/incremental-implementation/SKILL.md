---
name: incremental-implementation
description: Implement features incrementally with continuous verification. Break work into tiny steps, verify after each step, commit frequently, and maintain working state at all times.
tags: [implementation, incremental, verification, commits, workflow]
version: 1.0.0
author: Enhanced from Kent Beck's Extreme Programming)
---

# Incremental Implementation

## Overview

Implement features in tiny, verifiable steps. Never be more than 5 minutes away from a working state.

**Core principles:**
- **Tiny steps** - Each change is small and focused
- **Continuous verification** - Test after every step
- **Frequent commits** - Commit every 5-10 minutes
- **Always working** - Main branch is always deployable
- **Fast feedback** - Know immediately if something breaks

**Benefits:**
- Easier to debug (small changes = easy to isolate issues)
- Easier to review (small commits = clear intent)
- Easier to revert (granular history)
- Less stress (always have working code to fall back to)
- Faster progress (no big-bang integration)

## When to Use

**Always use when:**
- Implementing new features
- Refactoring existing code
- Fixing bugs
- Making any code changes

**Especially important when:**
- Working on unfamiliar code
- Making risky changes
- Working with others (avoid merge conflicts)
- Under time pressure (need to ship fast)

## The Incremental Implementation Process

```
┌─────────────────────────────────────────────────────────┐
│         INCREMENTAL IMPLEMENTATION CYCLE                 │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   STEP 1     │  Plan Next Tiny Step
    │     PLAN     │  - What's the smallest change?
    │              │  - How will I verify it?
    └──────┬───────┘  - 5-10 minutes max
           │
           ▼
    ┌──────────────┐
    │   STEP 2     │  Make the Change
    │   IMPLEMENT  │  - Write minimal code
    │              │  - One thing only
    └──────┬───────┘  - Stay focused
           │
           ▼
    ┌──────────────┐
    │   STEP 3     │  Verify It Works
    │    VERIFY    │  - Run tests
    │              │  - Manual check
    └──────┬───────┘  - Confirm behavior
           │
           ▼
    ┌──────────────┐
    │   STEP 4     │  Commit
    │    COMMIT    │  - Descriptive message
    │              │  - Push to remote
    └──────┬───────┘  - Create checkpoint
           │
           ▼
    ┌──────────────┐
    │   STEP 5     │  Next Step
    │     NEXT     │  - Repeat cycle
    │              │  - Until feature complete
    └──────────────┘

Each cycle: 5-10 minutes
Feature complete: 10-50 cycles
```

## Step 1: Plan Next Tiny Step

### What Makes a Step "Tiny"?

**Good tiny steps:**
- Takes 5-10 minutes to implement
- Changes 1-3 files
- Adds/modifies 10-50 lines of code
- Has one clear purpose
- Can be verified independently

**Too big:**
- Takes 30+ minutes
- Changes 10+ files
- Adds/modifies 200+ lines
- Does multiple things
- Requires complex verification

### Planning Framework

```markdown
## Next Step Planning

**Current State:**
[What works now]

**Next Tiny Step:**
[One small change]

**Expected Outcome:**
[How I'll know it worked]

**Verification:**
[How I'll test it]

**Time Estimate:**
[5-10 minutes]
```

### Example: Building a User Authentication System

<Bad - Too Big>
```markdown
**Next Step:** Implement user authentication

**Changes:**
- Create User model
- Add password hashing
- Create login endpoint
- Create signup endpoint
- Add JWT token generation
- Add authentication middleware
- Write tests

**Time:** 2-3 hours
```
</Bad>

<Good - Tiny Steps>
```markdown
**Step 1:** Create User model with email field only
- Add User model to schema
- Run migration
- Verify: User table exists in database
- Time: 5 minutes

**Step 2:** Add password field to User model
- Add password_hash field
- Run migration
- Verify: Field exists in database
- Time: 5 minutes

**Step 3:** Add password hashing function
- Install bcrypt
- Create hashPassword() utility
- Write unit test
- Verify: Test passes
- Time: 10 minutes

**Step 4:** Create signup endpoint (no validation)
- Add POST /api/signup route
- Hash password and save user
- Return user ID
- Verify: Can create user via curl
- Time: 10 minutes

**Step 5:** Add email validation to signup
- Add email format check
- Return 400 if invalid
- Write test
- Verify: Test passes
- Time: 10 minutes

[... continue with more tiny steps]
```
</Good>

### Step Size Guidelines by Task Type

**New feature:**
- Step 1: Add data model
- Step 2: Add API endpoint (happy path only)
- Step 3: Add validation
- Step 4: Add error handling
- Step 5: Add tests
- Step 6: Add UI component
- Step 7: Connect UI to API

**Refactoring:**
- Step 1: Extract function
- Step 2: Rename for clarity
- Step 3: Move to appropriate file
- Step 4: Update imports
- Step 5: Remove duplication

**Bug fix:**
- Step 1: Write failing test that reproduces bug
- Step 2: Fix the bug
- Step 3: Verify test passes
- Step 4: Add regression test

## Step 2: Implement the Change

### Implementation Rules

**Rule 1: One thing at a time**
- Don't fix typos while adding features
- Don't refactor while implementing
- Don't add "just one more thing"

**Rule 2: Minimal code**
- Write just enough to make the step work
- No premature optimization
- No "what if" code

**Rule 3: Stay focused**
- If you notice something else to fix, write it down
- Don't get distracted by tangents
- Finish current step first

### Example: Implementing a Step

<Good>
```typescript
// Step: Add email validation to signup endpoint

// Before
app.post('/api/signup', async (req, res) => {
  const { email, password } = req.body;
  const hashedPassword = await hashPassword(password);
  const user = await db.users.create({
    email,
    password_hash: hashedPassword
  });
  res.json({ id: user.id });
});

// After (just email validation, nothing else)
app.post('/api/signup', async (req, res) => {
  const { email, password } = req.body;
  
  // NEW: Email validation
  if (!email || !email.includes('@')) {
    return res.status(400).json({ error: 'Invalid email' });
  }
  
  const hashedPassword = await hashPassword(password);
  const user = await db.users.create({
    email,
    password_hash: hashedPassword
  });
  res.json({ id: user.id });
});
```
</Good>

<Bad - Doing Too Much>
```typescript
// Step: Add email validation to signup endpoint

// After (added validation + password validation + duplicate check + logging)
app.post('/api/signup', async (req, res) => {
  const { email, password } = req.body;
  
  // Email validation
  if (!email || !email.includes('@')) {
    logger.error('Invalid email', { email }); // EXTRA
    return res.status(400).json({ error: 'Invalid email' });
  }
  
  // Password validation (EXTRA - should be separate step)
  if (!password || password.length < 8) {
    logger.error('Weak password'); // EXTRA
    return res.status(400).json({ error: 'Password too short' });
  }
  
  // Check for duplicate (EXTRA - should be separate step)
  const existing = await db.users.findOne({ email });
  if (existing) {
    logger.warn('Duplicate signup attempt', { email }); // EXTRA
    return res.status(409).json({ error: 'Email already exists' });
  }
  
  const hashedPassword = await hashPassword(password);
  const user = await db.users.create({
    email,
    password_hash: hashedPassword
  });
  
  logger.info('User created', { userId: user.id }); // EXTRA
  res.json({ id: user.id });
});
```
</Bad>

## Step 3: Verify It Works

### Verification Checklist

**After every change:**
- [ ] Run relevant tests
- [ ] Manual verification (if applicable)
- [ ] Check for errors/warnings
- [ ] Confirm expected behavior

### Verification Methods

**1. Automated Tests**
```bash
# Run specific test file
npm test path/to/test.test.ts

# Run all tests
npm test

# Run with coverage
npm test -- --coverage
```

**2. Manual Testing**
```bash
# API endpoint
curl -X POST http://localhost:3000/api/signup \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"secret"}'

# Expected: 200 OK with user ID
# Actual: [verify response]
```

**3. Visual Inspection**
- Open browser
- Navigate to feature
- Interact with UI
- Confirm behavior matches expectation

**4. Database Check**
```sql
-- Verify data was saved
SELECT * FROM users WHERE email = 'test@example.com';

-- Expected: 1 row
-- Actual: [verify result]
```

### What to Do If Verification Fails

**Option 1: Fix immediately (if quick)**
- Fix the issue
- Re-verify
- Proceed to commit

**Option 2: Revert and retry (if complex)**
```bash
# Discard changes
git checkout .

# Start the step over
# Plan better this time
```

**Option 3: Debug systematically**
- Add logging
- Check assumptions
- Isolate the issue
- Fix and verify

## Step 4: Commit

### Commit Frequency

**Commit after every successful step:**
- Every 5-10 minutes
- Every time tests pass
- Every time you have working code

**Benefits:**
- Easy to revert if next step fails
- Clear history of progress
- Safe checkpoints
- Easy to review

### Commit Message Format

**Format:**
```
<type>: <short description>

<optional longer description>
<optional issue reference>
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `refactor:` Code restructuring
- `test:` Add/update tests
- `docs:` Documentation
- `style:` Formatting
- `chore:` Maintenance

**Examples:**

<Good>
```bash
git add src/models/user.ts prisma/schema.prisma
git commit -m "feat: add User model with email field

- Created User model in Prisma schema
- Added email field (required, unique)
- Ran migration to create users table"

git push
```
</Good>

<Bad>
```bash
# Too vague
git commit -m "updates"

# Too much in one commit
git commit -m "feat: implement entire auth system"

# No context
git commit -m "fix"
```
</Bad>

### Commit Best Practices

**1. Atomic commits**
- Each commit does one thing
- Each commit is self-contained
- Each commit leaves code in working state

**2. Descriptive messages**
- Explain what and why
- Reference issues/tickets
- Include context for future readers

**3. Push frequently**
```bash
# After every commit (or every few commits)
git push origin feature-branch
```

**Benefits:**
- Backup in case of local failure
- Others can see your progress
- CI/CD runs automatically

## Step 5: Next Step

### Planning the Next Step

**After committing, ask:**
1. What's the next smallest thing?
2. What's blocking me from shipping?
3. What's the highest risk to validate?

**Prioritization:**
- **High risk first** - Validate assumptions early
- **Dependencies first** - Unblock other work
- **Happy path first** - Get basic flow working
- **Edge cases later** - After happy path works

### Example: Next Steps After User Model

```markdown
**Completed:**
✅ Step 1: Create User model with email field
✅ Step 2: Add password field to User model
✅ Step 3: Add password hashing function

**Next Steps (Prioritized):**

**High Priority (Blocking):**
1. Create signup endpoint (happy path)
2. Test signup with curl
3. Add email validation
4. Add password validation

**Medium Priority (Important):**
5. Check for duplicate emails
6. Add error handling
7. Write integration tests

**Low Priority (Nice to have):**
8. Add email verification
9. Add rate limiting
10. Add logging
```

## Advanced Patterns

### Pattern 1: Feature Flags

**Use feature flags for large features:**

```typescript
// Step 1: Add feature flag
const ENABLE_NEW_AUTH = process.env.ENABLE_NEW_AUTH === 'true';

// Step 2: Implement behind flag
if (ENABLE_NEW_AUTH) {
  // New authentication logic
} else {
  // Old authentication logic
}

// Step 3: Test new logic in production (flag off)
// Step 4: Enable flag for 10% of users
// Step 5: Enable flag for 100% of users
// Step 6: Remove flag and old code
```

**Benefits:**
- Deploy incomplete features safely
- Test in production gradually
- Easy rollback if issues arise

### Pattern 2: Parallel Branches

**Work on multiple features simultaneously:**

```bash
# Main branch (always working)
git checkout main

# Feature branch 1
git checkout -b feature/auth
# Implement auth in tiny steps
# Commit frequently
# Push regularly

# Feature branch 2 (independent)
git checkout main
git checkout -b feature/posts
# Implement posts in tiny steps
# Commit frequently
# Push regularly

# Merge when ready
git checkout main
git merge feature/auth
git merge feature/posts
```

### Pattern 3: Spike and Stabilize

**For uncertain work:**

**Phase 1: Spike (exploration)**
- Quick and dirty implementation
- No tests, no polish
- Goal: Validate approach
- Commit to spike branch

**Phase 2: Stabilize (production)**
- Start fresh from main
- Implement properly with tests
- Use spike as reference
- Commit incrementally

```bash
# Spike
git checkout -b spike/new-feature
# Hack together quickly
git commit -m "spike: explore new feature approach"

# Stabilize
git checkout main
git checkout -b feature/new-feature
# Implement properly, incrementally
git commit -m "feat: add X (step 1)"
git commit -m "feat: add Y (step 2)"
# ...
```

## Incremental Implementation Checklist

**Before starting:**
- [ ] Feature is broken down into tiny steps
- [ ] Each step is 5-10 minutes
- [ ] Each step has clear verification
- [ ] Steps are ordered by dependency

**During implementation:**
- [ ] Working on one step at a time
- [ ] Not getting distracted by tangents
- [ ] Writing minimal code for current step
- [ ] Verifying after each change

**After each step:**
- [ ] Tests pass
- [ ] Manual verification done
- [ ] No errors or warnings
- [ ] Code committed with descriptive message
- [ ] Changes pushed to remote

**Before moving to next step:**
- [ ] Current step is complete
- [ ] Code is in working state
- [ ] Next step is planned
- [ ] Ready to proceed

## Common Pitfalls

### Pitfall 1: Steps Too Large

**Symptom:** Spending 30+ minutes on one step

**Solution:** Break it down further. If you can't finish in 10 minutes, the step is too big.

### Pitfall 2: Not Verifying

**Symptom:** Multiple steps without running tests

**Solution:** Verify after EVERY step. No exceptions.

### Pitfall 3: Committing Too Infrequently

**Symptom:** Working for hours without committing

**Solution:** Commit every 5-10 minutes. Set a timer if needed.

### Pitfall 4: Doing Multiple Things

**Symptom:** "While I'm here, I'll also fix..."

**Solution:** Write it down, finish current step first.

### Pitfall 5: Skipping Tests

**Symptom:** "I'll add tests later"

**Solution:** Tests are part of the step. No step is complete without verification.

## Integration with Other Skills

**Use before incremental implementation:**
- `spec-driven-development-enhanced` - Define what to build
- `architecture-blueprint` - Design the system
- `writing-plans` - Break down into tasks

**Use during incremental implementation:**
- `tdd-iron-law` - Write tests first
- `systematic-debugging` - When issues arise

**Use after incremental implementation:**
- `github-pr-workflow` - Submit for review
- `requesting-code-review` - Get feedback

## Real-World Example

### Task: Add User Profile Page

**Traditional Approach (Big Bang):**
```
Day 1-3: Implement entire profile page
Day 4: Test everything
Day 5: Fix bugs
Day 6: Submit PR
```

**Incremental Approach:**

```markdown
**Day 1 (Morning):**
✅ 9:00 - Step 1: Add /profile route (5 min)
✅ 9:05 - Step 2: Fetch user data (10 min)
✅ 9:15 - Step 3: Display user name (5 min)
✅ 9:20 - Step 4: Display user email (5 min)
✅ 9:25 - Step 5: Add profile picture placeholder (10 min)
✅ 9:35 - Step 6: Style with CSS (10 min)
✅ 9:45 - Step 7: Add loading state (10 min)
✅ 9:55 - Step 8: Add error state (10 min)

**Day 1 (Afternoon):**
✅ 1:00 - Step 9: Add edit button (5 min)
✅ 1:05 - Step 10: Add edit form (10 min)
✅ 1:15 - Step 11: Handle form submission (10 min)
✅ 1:25 - Step 12: Update user data (10 min)
✅ 1:35 - Step 13: Show success message (5 min)
✅ 1:40 - Step 14: Add validation (10 min)
✅ 1:50 - Step 15: Write integration test (10 min)

**Result:**
- Feature complete in 1 day (vs 6 days)
- 15 commits (vs 1 big commit)
- Always had working code
- Easy to review (small commits)
- No big-bang integration issues
```

## Metrics to Track

**Commit frequency:**
- Target: 1 commit every 5-10 minutes
- Measure: Commits per hour
- Good: 6+ commits/hour
- Bad: <2 commits/hour

**Step size:**
- Target: 5-10 minutes per step
- Measure: Time between commits
- Good: 5-10 minutes
- Bad: 30+ minutes

**Verification rate:**
- Target: 100% (verify after every step)
- Measure: Tests run per commit
- Good: 1+ test run per commit
- Bad: Multiple commits without tests

**Revert rate:**
- Target: <5% (rarely need to revert)
- Measure: Reverted commits / total commits
- Good: <5%
- Bad: >20%

---

**Remember:** Incremental implementation is about discipline, not speed. Go slow to go fast. Small steps, continuous verification, frequent commits.
