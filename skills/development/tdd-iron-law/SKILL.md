---
name: tdd-iron-law
description: Strict Test-Driven Development enforcement with iron-law discipline, mandatory verification steps, AST-based test mapping, and anti-rationalization guards. No production code without a failing test first.
tags: [tdd, testing, quality, discipline, red-green-refactor, regression-guard]
version: 1.0.0
author: Enhanced from obra/superpowers + TDAD research)
---

# TDD Iron Law - Test-Driven Development Enforcer

## Overview

Write the test first. Watch it fail. Write minimal code to pass. **No exceptions.**

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

**Violating the letter of the rules is violating the spirit of the rules.**

This enhanced version adds:
- AST-derived code-test dependency mapping
- Weighted impact analysis
- Pre-commit enforcement hooks
- Characterization test generation
- Mutation testing integration
- Anti-rationalization detection

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? **Delete it. Start over.**

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete

Implement fresh from tests. Period.

## When to Use

**Always:**
- New features
- Bug fixes
- Refactoring
- Behavior changes
- API changes
- Database schema changes

**Exceptions (ask your human partner):**
- Throwaway prototypes (mark as `prototype/` directory)
- Generated code (mark as `generated/`)
- Configuration files (YAML, JSON, TOML)
- Documentation-only changes

**Thinking "skip TDD just this once"?** Stop. That's rationalization. See Anti-Rationalization section below.

## The Enhanced RED-GREEN-REFACTOR Cycle

```
┌─────────────────────────────────────────────────────────┐
│                    TDD IRON LAW CYCLE                    │
└─────────────────────────────────────────────────────────┘

    ┌──────────┐
    │   RED    │  Write ONE failing test
    │  Write   │  (tests behavior, not implementation)
    │ Failing  │
    │   Test   │
    └────┬─────┘
         │
         ▼
    ┌──────────┐
    │ VERIFY   │  ⚠️ MANDATORY - Watch it fail
    │   RED    │  Confirm: fails for right reason
    └────┬─────┘  Not: passes, errors, wrong failure
         │
         │ ✓ Fails correctly
         ▼
    ┌──────────┐
    │  GREEN   │  Write MINIMAL code to pass
    │  Write   │  (just enough, no more)
    │ Minimal  │
    │   Code   │
    └────┬─────┘
         │
         ▼
    ┌──────────┐
    │ VERIFY   │  ⚠️ MANDATORY - Watch it pass
    │  GREEN   │  Confirm: test passes, others still pass
    └────┬─────┘  No errors, warnings, or console noise
         │
         │ ✓ All green
         ▼
    ┌──────────┐
    │ REFACTOR │  Clean up (optional)
    │  Clean   │  Remove duplication, improve names
    │   Code   │  Keep tests green
    └────┬─────┘
         │
         │ ✓ Still green
         ▼
    ┌──────────┐
    │   NEXT   │  Next failing test
    │   Test   │  for next behavior
    └──────────┘
```

## Phase 1: RED - Write Failing Test

### Step 1.1: Write ONE Minimal Test

Write one test showing what should happen. Test behavior, not implementation.

<Good>
```typescript
// tests/unit/retry.test.ts
import { describe, it, expect } from 'vitest';
import { retryOperation } from '@/lib/retry';

describe('retryOperation', () => {
  it('retries failed operations 3 times before succeeding', async () => {
    let attempts = 0;
    const operation = () => {
      attempts++;
      if (attempts < 3) throw new Error('fail');
      return 'success';
    };

    const result = await retryOperation(operation);

    expect(result).toBe('success');
    expect(attempts).toBe(3);
  });
});
```

**Why this is good:**
- Clear, descriptive name
- Tests real behavior (not mocks)
- One thing only
- Arrange-Act-Assert structure
- No implementation details leaked
</Good>

<Bad>
```typescript
// ❌ BAD: Tests implementation, not behavior
test('retry works', async () => {
  const mock = jest.fn()
    .mockRejectedValueOnce(new Error())
    .mockRejectedValueOnce(new Error())
    .mockResolvedValueOnce('success');
  
  await retryOperation(mock);
  
  expect(mock).toHaveBeenCalledTimes(3);
});
```

**Why this is bad:**
- Vague name ("works" is meaningless)
- Tests mock behavior, not real code
- Coupled to implementation (call count)
- Doesn't verify actual result
</Bad>

### Step 1.2: Test Naming Convention

**Format:** `it('should [expected behavior] when [condition]')`

**Examples:**
```typescript
// ✅ Good names
it('should return user when ID exists')
it('should throw NotFoundError when ID does not exist')
it('should retry 3 times before failing')
it('should cache result after first successful call')
it('should invalidate cache when TTL expires')

// ❌ Bad names
it('works')
it('test user')
it('should call the API')
it('handles errors')
```

### Step 1.3: Test Structure (Arrange-Act-Assert)

**Always use AAA pattern:**

```typescript
it('should calculate total price with tax', () => {
  // Arrange - Set up test data
  const items = [
    { price: 10, quantity: 2 },
    { price: 5, quantity: 3 }
  ];
  const taxRate = 0.1;

  // Act - Execute the behavior
  const total = calculateTotal(items, taxRate);

  // Assert - Verify the result
  expect(total).toBe(38.5); // (20 + 15) * 1.1
});
```

### Step 1.4: One Behavior Per Test

**Don't test multiple things in one test:**

<Bad>
```typescript
// ❌ BAD: Tests multiple behaviors
it('should handle user operations', async () => {
  const user = await createUser({ name: 'John' });
  expect(user.name).toBe('John');
  
  const updated = await updateUser(user.id, { name: 'Jane' });
  expect(updated.name).toBe('Jane');
  
  await deleteUser(user.id);
  const deleted = await getUser(user.id);
  expect(deleted).toBeNull();
});
```
</Bad>

<Good>
```typescript
// ✅ GOOD: Separate tests for each behavior
describe('User operations', () => {
  it('should create user with given name', async () => {
    const user = await createUser({ name: 'John' });
    expect(user.name).toBe('John');
  });

  it('should update user name', async () => {
    const user = await createUser({ name: 'John' });
    const updated = await updateUser(user.id, { name: 'Jane' });
    expect(updated.name).toBe('Jane');
  });

  it('should return null when getting deleted user', async () => {
    const user = await createUser({ name: 'John' });
    await deleteUser(user.id);
    const deleted = await getUser(user.id);
    expect(deleted).toBeNull();
  });
});
```
</Good>

## Phase 2: VERIFY RED - Watch It Fail

### ⚠️ MANDATORY - Never Skip This Step

**Run the test and confirm it fails:**

```bash
npm test path/to/test.test.ts
```

### What to Verify

**✅ Test must fail (not error, not pass):**
- Exit code is non-zero
- Test runner shows "FAIL" or "✗"
- Failure message is expected

**✅ Failure reason is correct:**
- Fails because feature is missing
- Not because of typos
- Not because of wrong imports
- Not because of test setup issues

**✅ Failure message is clear:**
```
❌ FAIL  tests/unit/retry.test.ts
  retryOperation
    ✗ retries failed operations 3 times before succeeding
    
    ReferenceError: retryOperation is not defined
```

### Common Issues

**Issue 1: Test passes immediately**
```
✓ retries failed operations 3 times before succeeding
```

**Problem:** You're testing existing behavior, not new behavior.

**Solution:** Fix the test to test the actual new behavior.

---

**Issue 2: Test errors instead of failing**
```
✗ retryOperation
  TypeError: Cannot read property 'retry' of undefined
```

**Problem:** Test setup is wrong (imports, mocks, etc.)

**Solution:** Fix the error, re-run until it fails correctly.

---

**Issue 3: Wrong failure message**
```
✗ retries failed operations 3 times before succeeding
  Expected: 'success'
  Received: undefined
```

**Problem:** Test is failing for wrong reason (implementation exists but is broken)

**Solution:** Check if you accidentally wrote implementation first. If so, delete it and start over.

## Phase 3: GREEN - Write Minimal Code

### Step 3.1: Write Just Enough Code

Write the simplest code that makes the test pass. **No more, no less.**

<Good>
```typescript
// src/lib/retry.ts
export async function retryOperation<T>(
  fn: () => Promise<T>
): Promise<T> {
  for (let i = 0; i < 3; i++) {
    try {
      return await fn();
    } catch (e) {
      if (i === 2) throw e;
    }
  }
  throw new Error('unreachable');
}
```

**Why this is good:**
- Just enough to pass the test
- No extra features
- No premature optimization
- No "what if" code
</Good>

<Bad>
```typescript
// ❌ BAD: Over-engineered
export async function retryOperation<T>(
  fn: () => Promise<T>,
  options?: {
    maxRetries?: number;
    backoff?: 'linear' | 'exponential';
    onRetry?: (attempt: number) => void;
    shouldRetry?: (error: Error) => boolean;
  }
): Promise<T> {
  const maxRetries = options?.maxRetries ?? 3;
  const backoff = options?.backoff ?? 'linear';
  
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (e) {
      if (options?.shouldRetry && !options.shouldRetry(e as Error)) {
        throw e;
      }
      
      if (i < maxRetries - 1) {
        options?.onRetry?.(i + 1);
        const delay = backoff === 'exponential' 
          ? Math.pow(2, i) * 1000 
          : (i + 1) * 1000;
        await new Promise(resolve => setTimeout(resolve, delay));
      } else {
        throw e;
      }
    }
  }
  throw new Error('unreachable');
}
```

**Why this is bad:**
- YAGNI (You Aren't Gonna Need It)
- No test requires these features
- Adds complexity without value
- Harder to maintain
</Bad>

### Step 3.2: Resist Temptation

**Don't:**
- Add features not in the test
- Refactor other code
- "Improve" beyond the test
- Add error handling not tested
- Add logging not tested
- Add configuration not tested

**The test defines the contract. Nothing more.**

## Phase 4: VERIFY GREEN - Watch It Pass

### ⚠️ MANDATORY - Never Skip This Step

**Run the test and confirm it passes:**

```bash
npm test path/to/test.test.ts
```

### What to Verify

**✅ Test passes:**
```
✓ retries failed operations 3 times before succeeding
```

**✅ All other tests still pass:**
```bash
npm test
```

**✅ No errors, warnings, or console noise:**
- No `console.error` output
- No `console.warn` output
- No unhandled promise rejections
- No deprecation warnings

### Common Issues

**Issue 1: Test still fails**

**Solution:** Fix the code (not the test). The test defines the contract.

---

**Issue 2: Other tests fail**

**Solution:** Fix the regression immediately. Don't proceed until all tests are green.

---

**Issue 3: Test passes but with warnings**

**Solution:** Fix the warnings. Clean output is mandatory.

## Phase 5: REFACTOR - Clean Up (Optional)

**Only after green, you may refactor:**

### What You Can Do

**✅ Allowed:**
- Remove duplication
- Improve variable names
- Extract helper functions
- Improve code structure
- Add comments (sparingly)

**❌ Not Allowed:**
- Add new behavior
- Change behavior
- Add features
- Skip tests
- Break tests

### Refactoring Rules

**Rule 1: Keep tests green**

Run tests after every refactoring step:

```bash
npm test -- --watch
```

**Rule 2: Small steps**

Refactor in tiny increments. If tests fail, you know exactly what broke.

**Rule 3: Commit often**

Commit after each successful refactor:

```bash
git add .
git commit -m "refactor: extract retry logic into helper"
```

### Example Refactoring

<Before>
```typescript
export async function retryOperation<T>(fn: () => Promise<T>): Promise<T> {
  for (let i = 0; i < 3; i++) {
    try {
      return await fn();
    } catch (e) {
      if (i === 2) throw e;
    }
  }
  throw new Error('unreachable');
}
```
</Before>

<After>
```typescript
const MAX_RETRIES = 3;

export async function retryOperation<T>(fn: () => Promise<T>): Promise<T> {
  return retryWithCount(fn, MAX_RETRIES);
}

async function retryWithCount<T>(
  fn: () => Promise<T>,
  maxRetries: number
): Promise<T> {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (isLastAttempt(attempt, maxRetries)) {
        throw error;
      }
    }
  }
  throw new Error('unreachable');
}

function isLastAttempt(attempt: number, maxRetries: number): boolean {
  return attempt === maxRetries - 1;
}
```
</After>

**Verify tests still pass:**
```bash
npm test
✓ retries failed operations 3 times before succeeding
```

## Enhanced Features

### Feature 1: AST-Based Test Mapping

**Generate a test dependency map to prevent regressions:**

```bash
# Generate test map (run once, update when structure changes)
npm run test:map
```

**Output: `.hermes/test-map.json`**
```json
{
  "src/lib/retry.ts": {
    "tests": [
      "tests/unit/retry.test.ts"
    ],
    "dependencies": [
      "src/lib/utils.ts"
    ],
    "impact_score": 0.8
  },
  "src/lib/utils.ts": {
    "tests": [
      "tests/unit/utils.test.ts",
      "tests/unit/retry.test.ts"
    ],
    "dependencies": [],
    "impact_score": 0.95
  }
}
```

**Usage:**
```bash
# Before modifying a file, check which tests to run
npm run test:impact src/lib/retry.ts

# Output:
# Tests to run:
#   - tests/unit/retry.test.ts (direct)
#   - tests/integration/api.test.ts (indirect via src/api/handler.ts)
```

### Feature 2: Pre-Commit Hook Enforcement

**Install TDD enforcement hook:**

```bash
# Add to .husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# Check for untested code
npm run test:coverage -- --check-coverage

# Check for skipped tests
if git diff --cached --name-only | grep -q '\.test\.ts$'; then
  if git diff --cached | grep -q '\.skip\|\.todo'; then
    echo "❌ Error: Skipped tests detected (.skip or .todo)"
    echo "Remove .skip() or .todo() before committing"
    exit 1
  fi
fi

# Check for @ts-ignore without ticket reference
if git diff --cached | grep -q '@ts-ignore'; then
  if ! git diff --cached | grep -B1 '@ts-ignore' | grep -q 'TODO\|FIXME\|ticket'; then
    echo "❌ Error: @ts-ignore without ticket reference"
    echo "Add a TODO/FIXME comment with ticket number"
    exit 1
  fi
fi
```

### Feature 3: Coverage Threshold Enforcement

**Configure minimum coverage in `package.json`:**

```json
{
  "jest": {
    "coverageThreshold": {
      "global": {
        "branches": 80,
        "functions": 80,
        "lines": 80,
        "statements": 80
      },
      "src/lib/": {
        "branches": 90,
        "functions": 90,
        "lines": 90,
        "statements": 90
      }
    }
  }
}
```

**Run with threshold check:**
```bash
npm test -- --coverage --coverageThreshold
```

### Feature 4: Characterization Test Generation

**When refactoring legacy code without tests:**

```bash
# Generate characterization tests from existing behavior
npm run test:characterize src/legacy/module.ts
```

**Output: `tests/characterization/module.test.ts`**
```typescript
// Auto-generated characterization tests
// These tests capture current behavior before refactoring
// Review and adjust as needed

describe('Legacy module (characterization)', () => {
  it('should return 42 when called with no arguments', () => {
    const result = legacyFunction();
    expect(result).toBe(42);
  });

  it('should throw TypeError when called with null', () => {
    expect(() => legacyFunction(null)).toThrow(TypeError);
  });

  // ... more tests capturing current behavior
});
```

**Workflow:**
1. Generate characterization tests
2. Verify tests pass (capturing current behavior)
3. Refactor code
4. Tests should still pass (behavior unchanged)
5. Replace characterization tests with proper unit tests

### Feature 5: Mutation Testing

**Verify test quality with mutation testing:**

```bash
# Install Stryker
npm install --save-dev @stryker-mutator/core @stryker-mutator/typescript-checker

# Run mutation tests
npm run test:mutation
```

**What it does:**
- Introduces small changes (mutations) to your code
- Runs tests to see if they catch the mutations
- Reports "mutation score" (% of mutations caught)

**Example output:**
```
Mutation Score: 85%

Survived Mutants (tests didn't catch these):
  src/lib/retry.ts:12:5
    - Changed `i < 3` to `i <= 3`
    → Add test for exact retry count

  src/lib/retry.ts:15:7
    - Removed `if (i === 2)` condition
    → Add test for error propagation
```

## Anti-Rationalization Guards

### Common Rationalizations (and Rebuttals)

| Rationalization | Reality | Action |
|---|---|---|
| "This is too simple to test" | Simple code still needs tests. Simple tests are fast to write. | Write the test. |
| "I'll add tests later" | Later never comes. Tests written after are documentation, not TDD. | Write the test now. |
| "The test is obvious" | If it's obvious, it's quick to write. If you skip it, you don't know it works. | Write the test. |
| "I'm just experimenting" | Experiments become production code. Test from the start or mark as `prototype/`. | Write the test or move to `prototype/`. |
| "I need to see if this approach works first" | That's what the test is for. Write a test for the approach. | Write the test. |
| "The test would be too complex" | Complex tests reveal complex code. Simplify the code. | Simplify, then write the test. |
| "I'm refactoring, not adding features" | Refactoring without tests is rewriting. Tests prove behavior is preserved. | Write characterization tests first. |
| "The deadline is tight" | Skipping tests creates bugs that take longer to fix than writing tests. | Write the test. It's faster. |
| "I'll just do a quick fix" | Quick fixes without tests become permanent bugs. | Write the test. |
| "This code is hard to test" | Hard to test = poorly designed. Redesign for testability. | Redesign, then write the test. |

### Rationalization Detection

**If you catch yourself thinking any of these, STOP:**

- "Just this once..."
- "I'll come back to it..."
- "It's too simple..."
- "It's too complex..."
- "I don't have time..."
- "I'll test it manually..."
- "The test would be longer than the code..."

**These are all rationalizations. Write the test.**

## Good Tests Checklist

| Quality | Good | Bad |
|---------|------|-----|
| **Name** | Describes behavior | Vague or technical |
| **Focus** | One behavior | Multiple behaviors |
| **Structure** | Arrange-Act-Assert | Mixed or unclear |
| **Dependencies** | Minimal mocks | Heavy mocking |
| **Assertions** | Specific values | Vague matchers |
| **Speed** | Fast (< 100ms) | Slow (> 1s) |
| **Isolation** | Independent | Depends on other tests |
| **Clarity** | Self-documenting | Needs comments |

## Red Flags (Stop Immediately)

**Stop and fix if you notice:**

- [ ] Writing production code before test
- [ ] Test passes on first run (didn't watch it fail)
- [ ] Test name doesn't describe behavior
- [ ] Test tests implementation, not behavior
- [ ] Test requires complex setup (>10 lines)
- [ ] Test is slow (>1 second)
- [ ] Test depends on other tests
- [ ] Test has multiple assertions for different behaviors
- [ ] Skipping tests with `.skip()` or `.todo()`
- [ ] Using `@ts-ignore` without ticket reference
- [ ] Coverage dropping below threshold
- [ ] Committing code without tests

## Verification Checklist

**Before committing, verify:**

- [ ] All new code has tests
- [ ] All tests pass
- [ ] No skipped tests (`.skip()`, `.todo()`)
- [ ] No console errors or warnings
- [ ] Coverage meets threshold (80%+)
- [ ] Test names describe behavior
- [ ] Tests follow AAA pattern
- [ ] Tests are fast (< 100ms each)
- [ ] Tests are isolated (no dependencies)
- [ ] Mutation score > 80% (if running mutation tests)

## Integration with Other Skills

**Load these skills alongside TDD:**

- `spec-driven-development-enhanced` - For requirements clarity
- `systematic-debugging` - When tests reveal bugs
- `github-pr-workflow` - For committing tested code
- `requesting-code-review` - For pre-commit review

## Tools and Setup

### Recommended Test Frameworks

**JavaScript/TypeScript:**
- Vitest (fast, modern)
- Jest (mature, widely used)
- Playwright (e2e)

**Python:**
- pytest (flexible, powerful)
- unittest (built-in)

**Go:**
- testing (built-in)
- testify (assertions)

**Rust:**
- cargo test (built-in)

### Recommended Tools

**Coverage:**
- c8 (Node.js)
- coverage.py (Python)
- tarpaulin (Rust)

**Mutation Testing:**
- Stryker (JavaScript/TypeScript)
- mutmut (Python)
- cargo-mutants (Rust)

**Test Mapping:**
- Custom script (see `scripts/test-map.sh`)

## Example: Complete TDD Session

See `references/tdd-session-example.md` for a complete walkthrough of a TDD session following this iron-law approach.

## Pitfalls

### Pitfall 1: Testing Implementation Instead of Behavior

**Symptom:** Tests break when refactoring, even though behavior is unchanged

**Solution:** Test public API and observable behavior, not internal implementation

### Pitfall 2: Writing Tests After Code

**Symptom:** Tests always pass, don't catch bugs, feel like busywork

**Solution:** Delete code, write test first, watch it fail, then implement

### Pitfall 3: Skipping Verification Steps

**Symptom:** Tests pass but don't actually test anything, false confidence

**Solution:** Always watch tests fail before making them pass

### Pitfall 4: Over-Mocking

**Symptom:** Tests are complex, brittle, don't catch real bugs

**Solution:** Use real objects when possible, mock only external dependencies

### Pitfall 5: Slow Tests

**Symptom:** Tests take minutes to run, developers skip running them

**Solution:** Keep tests fast (<100ms), use test doubles for slow operations

---

**Remember:** TDD is not about testing. It's about design. The tests are a side effect of good design. The iron law ensures you design before you implement.

**The iron law is not negotiable. It's not a suggestion. It's a law.**
