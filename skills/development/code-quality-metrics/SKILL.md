---
name: code-quality-metrics
description: Measure and improve code quality with cyclomatic complexity, test coverage, code churn, maintainability index, and technical debt tracking. Data-driven quality improvement.
tags: [code-quality, metrics, complexity, coverage, maintainability, technical-debt, sonarqube]
version: 1.0.0
author: Enhanced from code quality best practices)
---

# Code Quality Metrics

## Overview

Measure and improve code quality with objective metrics.

**Why code quality metrics matter:**
- **Visibility:** Know where problems are
- **Trends:** Track improvement over time
- **Prioritization:** Focus on high-impact areas
- **Prevention:** Catch issues early
- **Communication:** Objective quality discussions

**Key principles:**
- **Measure consistently:** Same metrics over time
- **Act on data:** Metrics without action are useless
- **Balance:** No single metric tells the whole story
- **Context matters:** High complexity isn't always bad
- **Automate:** Manual tracking doesn't scale

## When to Use

**Use for:**
- Code reviews
- Refactoring decisions
- Technical debt tracking
- Team quality goals
- CI/CD quality gates

**Critical for:**
- Large codebases (10K+ LOC)
- Team projects (3+ developers)
- Long-lived projects (1+ years)
- Quality-critical systems
- Legacy code modernization

## The 8-Metric Quality Framework

```
┌─────────────────────────────────────────────────────────┐
│         CODE QUALITY METRICS FRAMEWORK                   │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   METRIC 1   │  Cyclomatic Complexity
    │  COMPLEXITY  │  - Measure: Branches
    │              │  - Target: < 10
    └──────┬───────┘  - Tool: Radon, SonarQube
           │
           ▼
    ┌──────────────┐
    │   METRIC 2   │  Test Coverage
    │   COVERAGE   │  - Measure: % lines tested
    │              │  - Target: > 80%
    └──────┬───────┘  - Tool: Coverage.py, Jest
           │
           ▼
    ┌──────────────┐
    │   METRIC 3   │  Code Churn
    │    CHURN     │  - Measure: Lines changed
    │              │  - Target: Low churn
    └──────┬───────┘  - Tool: Git, CodeScene
           │
           ▼
    ┌──────────────┐
    │   METRIC 4   │  Maintainability Index
    │MAINTAINABILITY│ - Measure: 0-100 score
    │              │  - Target: > 65
    └──────┬───────┘  - Tool: Radon, CodeClimate
           │
           ▼
    ┌──────────────┐
    │   METRIC 5   │  Code Duplication
    │ DUPLICATION  │  - Measure: % duplicated
    │              │  - Target: < 5%
    └──────┬───────┘  - Tool: PMD, SonarQube
           │
           ▼
    ┌──────────────┐
    │   METRIC 6   │  Technical Debt
    │     DEBT     │  - Measure: Hours to fix
    │              │  - Target: Decreasing
    └──────┬───────┘  - Tool: SonarQube, CodeScene
           │
           ▼
    ┌──────────────┐
    │   METRIC 7   │  Code Smells
    │    SMELLS    │  - Measure: Count
    │              │  - Target: 0
    └──────┬───────┘  - Tool: SonarQube, Pylint
           │
           ▼
    ┌──────────────┐
    │   METRIC 8   │  Dependency Health
    │ DEPENDENCIES │  - Measure: Outdated/vulnerable
    │              │  - Target: 0 critical
    └──────────────┘  - Tool: Dependabot, Snyk
```

## Metric 1: Cyclomatic Complexity

### Goal: Measure code complexity

**What is Cyclomatic Complexity?**

```markdown
## Definition
Number of independent paths through code.

## Formula
Complexity = Edges - Nodes + 2 * Connected Components

## Simplified
Count decision points (if, for, while, case) + 1

## Example
```python
def calculate_discount(price, customer_type, quantity):
    discount = 0
    
    if customer_type == 'premium':  # +1
        discount = 0.2
    elif customer_type == 'regular':  # +1
        discount = 0.1
    
    if quantity > 100:  # +1
        discount += 0.05
    elif quantity > 50:  # +1
        discount += 0.02
    
    return price * (1 - discount)

# Complexity = 5 (1 base + 4 decision points)
```

**Complexity Ratings:**

```markdown
## Thresholds
- **1-5:** Simple, easy to test
- **6-10:** Moderate, acceptable
- **11-20:** Complex, consider refactoring
- **21+:** Very complex, refactor immediately

## Target
- Functions: < 10
- Classes: < 50
- Modules: < 100
```

**Measuring Complexity:**

```bash
# Python: Radon
pip install radon

# Cyclomatic complexity
radon cc myapp/ -a -s
# Output:
# myapp/views.py
#   M 45:0 calculate_discount - B (5)
#   M 60:0 process_order - C (12)

# JavaScript: ESLint complexity rule
# .eslintrc.json
{
  "rules": {
    "complexity": ["error", 10]
  }
}

# Java: Checkstyle
# checkstyle.xml
<module name="CyclomaticComplexity">
  <property name="max" value="10"/>
</module>

# SonarQube (all languages)
# Automatic complexity analysis
```

**Reducing Complexity:**

```python
# ❌ High complexity (12)
def process_order(order):
    if order.status == 'pending':
        if order.payment_method == 'credit_card':
            if order.amount > 1000:
                if order.customer.is_verified:
                    # Process large verified credit card order
                    pass
                else:
                    # Reject unverified
                    pass
            else:
                # Process small credit card order
                pass
        elif order.payment_method == 'paypal':
            if order.amount > 500:
                # Process large PayPal order
                pass
            else:
                # Process small PayPal order
                pass
        else:
            # Unknown payment method
            pass
    else:
        # Order not pending
        pass

# ✅ Low complexity (3) - Extract methods
def process_order(order):
    if order.status != 'pending':
        return handle_non_pending_order(order)
    
    if order.payment_method == 'credit_card':
        return process_credit_card_order(order)
    elif order.payment_method == 'paypal':
        return process_paypal_order(order)
    else:
        return handle_unknown_payment_method(order)

def process_credit_card_order(order):
    if order.amount > 1000 and not order.customer.is_verified:
        return reject_unverified_large_order(order)
    return charge_credit_card(order)

def process_paypal_order(order):
    return charge_paypal(order)
```

---

## Metric 2: Test Coverage

### Goal: Measure test completeness

**What is Test Coverage?**

```markdown
## Types
- **Line coverage:** % of lines executed
- **Branch coverage:** % of branches taken
- **Function coverage:** % of functions called
- **Statement coverage:** % of statements executed

## Target
- **Minimum:** 80% line coverage
- **Good:** 90% line coverage
- **Excellent:** 95%+ line coverage

## Note
100% coverage ≠ bug-free code
Coverage measures quantity, not quality
```

**Measuring Coverage:**

```bash
# Python: Coverage.py
pip install coverage

# Run tests with coverage
coverage run -m pytest
coverage report
coverage html  # Generate HTML report

# Output:
# Name                Stmts   Miss  Cover
# ---------------------------------------
# myapp/views.py        100     10    90%
# myapp/models.py        50      5    90%
# myapp/utils.py         30      0   100%
# ---------------------------------------
# TOTAL                 180     15    92%

# JavaScript: Jest
npm test -- --coverage

# Output:
# File      | % Stmts | % Branch | % Funcs | % Lines
# ----------|---------|----------|---------|--------
# All files |   92.5  |   85.7   |   90.0  |  92.5
# utils.js  |  100.0  |  100.0   |  100.0  | 100.0
# api.js    |   85.0  |   75.0   |   80.0  |  85.0

# Java: JaCoCo
mvn clean test jacoco:report

# Go: Built-in
go test -cover ./...
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

**Coverage in CI/CD:**

```yaml
# GitHub Actions
name: Test Coverage
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run tests with coverage
        run: |
          pip install coverage pytest
          coverage run -m pytest
          coverage report --fail-under=80
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage.xml
          fail_ci_if_error: true
```

**Improving Coverage:**

```python
# Identify uncovered code
coverage report --show-missing

# Output:
# Name              Stmts   Miss  Cover   Missing
# -----------------------------------------------
# myapp/views.py      100     10    90%   45-50, 78-82

# Write tests for missing lines
def test_edge_case():
    # Test lines 45-50
    result = calculate_discount(price=100, customer_type='vip', quantity=1)
    assert result == 80  # 20% VIP discount
```

---

## Metric 3: Code Churn

### Goal: Identify unstable code

**What is Code Churn?**

```markdown
## Definition
Number of times a file/function changes over time.

## High Churn Indicates
- Unstable requirements
- Poor design
- Bug-prone code
- Lack of understanding

## Calculation
Churn = Lines added + Lines deleted + Lines modified
```

**Measuring Churn:**

```bash
# Git: Lines changed per file (last 30 days)
git log --since="30 days ago" --numstat --pretty=format: | \
  awk '{files[$3]++; added[$3]+=$1; deleted[$3]+=$2} \
  END {for (file in files) print files[file], added[file]+deleted[file], file}' | \
  sort -rn | head -20

# Output:
# 45 2340 src/api/orders.py
# 32 1890 src/models/user.py
# 28 1560 src/utils/helpers.py

# CodeScene (commercial tool)
# Automatic churn analysis + hotspot detection

# Git: Commits per file
git log --all --format='%H' -- path/to/file.py | wc -l

# Git: Authors per file (complexity indicator)
git log --all --format='%aN' -- path/to/file.py | sort -u | wc -l
```

**Churn Heatmap:**

```markdown
## High Churn + High Complexity = DANGER
- Frequently changed
- Hard to understand
- Bug-prone
- **Action:** Refactor immediately

## High Churn + Low Complexity = OK
- Frequently changed
- Easy to understand
- **Action:** Monitor

## Low Churn + High Complexity = TECHNICAL DEBT
- Rarely changed
- Hard to understand
- **Action:** Refactor when touched

## Low Churn + Low Complexity = GOOD
- Stable
- Easy to understand
- **Action:** Leave alone
```

---

## Metric 4: Maintainability Index

### Goal: Overall code health score

**What is Maintainability Index?**

```markdown
## Definition
Composite metric (0-100) based on:
- Cyclomatic complexity
- Lines of code
- Halstead volume (operators/operands)
- Comment density

## Formula (Microsoft)
MI = 171 - 5.2 * ln(HV) - 0.23 * CC - 16.2 * ln(LOC)

Where:
- HV = Halstead Volume
- CC = Cyclomatic Complexity
- LOC = Lines of Code

## Ratings
- **85-100:** Highly maintainable (green)
- **65-84:** Moderately maintainable (yellow)
- **0-64:** Difficult to maintain (red)

## Target
> 65 (yellow or green)
```

**Measuring Maintainability:**

```bash
# Python: Radon
radon mi myapp/ -s

# Output:
# myapp/views.py - A (85.3)
# myapp/models.py - B (72.1)
# myapp/utils.py - A (90.5)

# Visual Studio (C#, VB.NET)
# Analyze > Calculate Code Metrics

# SonarQube (all languages)
# Automatic maintainability analysis
```

---

## Metric 5: Code Duplication

### Goal: Identify copy-paste code

**What is Code Duplication?**

```markdown
## Definition
Percentage of duplicated code blocks.

## Problems
- Bugs multiply (fix in one place, miss others)
- Maintenance burden
- Wasted space

## Target
< 5% duplication

## Types
- **Exact duplication:** Identical code
- **Structural duplication:** Same structure, different names
- **Semantic duplication:** Same logic, different implementation
```

**Measuring Duplication:**

```bash
# Python: Pylint
pylint --disable=all --enable=duplicate-code myapp/

# JavaScript: jscpd
npm install -g jscpd
jscpd src/

# Output:
# Found 15 clones with 450 duplicated lines in 8 files
# Duplication: 12.5%

# Java: PMD CPD
pmd cpd --minimum-tokens 100 --files src/

# SonarQube
# Automatic duplication detection
```

**Fixing Duplication:**

```python
# ❌ Duplication
def calculate_order_total_usd(items):
    total = 0
    for item in items:
        total += item.price * item.quantity
    tax = total * 0.08
    return total + tax

def calculate_order_total_eur(items):
    total = 0
    for item in items:
        total += item.price * item.quantity
    tax = total * 0.20
    return total + tax

# ✅ Extract common logic
def calculate_order_total(items, tax_rate):
    total = sum(item.price * item.quantity for item in items)
    tax = total * tax_rate
    return total + tax

def calculate_order_total_usd(items):
    return calculate_order_total(items, tax_rate=0.08)

def calculate_order_total_eur(items):
    return calculate_order_total(items, tax_rate=0.20)
```

---

## Metric 6: Technical Debt

### Goal: Quantify cleanup work

**What is Technical Debt?**

```markdown
## Definition
Estimated time to fix all code quality issues.

## Calculation (SonarQube)
Debt = Sum of remediation times for all issues

Example:
- 10 critical bugs × 1 hour = 10 hours
- 50 code smells × 15 minutes = 12.5 hours
- 5 security vulnerabilities × 2 hours = 10 hours
- **Total debt:** 32.5 hours

## Debt Ratio
Debt Ratio = (Remediation Cost / Development Cost) × 100

Target: < 5%

## SQALE Rating (SonarQube)
- **A:** ≤ 5% (≤ 0.05 days/day)
- **B:** 6-10%
- **C:** 11-20%
- **D:** 21-50%
- **E:** > 50%
```

**Tracking Technical Debt:**

```bash
# SonarQube
# Automatic debt calculation

# Manual tracking (GitHub Issues)
# Label: technical-debt
# Estimate: hours to fix
# Priority: high/medium/low

# Debt burndown chart
# Track debt over time (sprints/months)
```

**Managing Technical Debt:**

```markdown
## Boy Scout Rule
Leave code better than you found it.

## 20% Time
Dedicate 20% of sprint to debt reduction.

## Debt Ceiling
Set maximum debt ratio (e.g., 10%).
Block new features if exceeded.

## Prioritization
1. **Critical bugs:** Fix immediately
2. **Security vulnerabilities:** Fix ASAP
3. **High-churn + high-complexity:** Refactor
4. **Low-impact smells:** Ignore or defer
```

---

## Metric 7: Code Smells

### Goal: Identify design problems

**Common Code Smells:**

```markdown
## 1. Long Method
**Problem:** Method > 50 lines
**Fix:** Extract smaller methods

## 2. Large Class
**Problem:** Class > 500 lines or > 10 methods
**Fix:** Split into smaller classes

## 3. Long Parameter List
**Problem:** > 5 parameters
**Fix:** Use parameter object

## 4. Duplicated Code
**Problem:** Copy-paste code
**Fix:** Extract method/class

## 5. Dead Code
**Problem:** Unused code
**Fix:** Delete it

## 6. Magic Numbers
**Problem:** Hardcoded values
**Fix:** Use named constants

## 7. God Object
**Problem:** Class does everything
**Fix:** Single Responsibility Principle

## 8. Feature Envy
**Problem:** Method uses another class more than its own
**Fix:** Move method to other class

## 9. Shotgun Surgery
**Problem:** One change requires many edits
**Fix:** Move related code together

## 10. Primitive Obsession
**Problem:** Using primitives instead of objects
**Fix:** Create value objects
```

**Detecting Code Smells:**

```bash
# Python: Pylint
pylint myapp/

# Output:
# C0103: Variable name "x" doesn't conform to snake_case
# R0913: Too many arguments (6/5)
# R0914: Too many local variables (16/15)
# W0612: Unused variable 'result'

# JavaScript: ESLint
eslint src/

# Java: PMD
pmd check -d src/ -R rulesets/java/quickstart.xml

# SonarQube
# Comprehensive smell detection
```

---

## Metric 8: Dependency Health

### Goal: Track outdated/vulnerable dependencies

**What to Track:**

```markdown
## Outdated Dependencies
- Major versions behind
- Security patches available
- End-of-life libraries

## Vulnerable Dependencies
- Known CVEs
- Severity (critical, high, medium, low)
- Exploitability

## License Issues
- Incompatible licenses
- Copyleft requirements
- Commercial restrictions
```

**Checking Dependencies:**

```bash
# Python: pip-audit (security)
pip install pip-audit
pip-audit

# Python: pip list --outdated
pip list --outdated

# JavaScript: npm audit
npm audit
npm audit fix

# JavaScript: npm outdated
npm outdated

# Java: OWASP Dependency-Check
mvn org.owasp:dependency-check-maven:check

# Ruby: bundler-audit
bundle audit

# Snyk (all languages)
npm install -g snyk
snyk test
snyk monitor

# Dependabot (GitHub)
# Automatic PR for dependency updates
```

**Dependency Policy:**

```markdown
## Update Frequency
- **Security patches:** Immediately
- **Minor versions:** Monthly
- **Major versions:** Quarterly (with testing)

## Vulnerability Response
- **Critical:** Fix within 24 hours
- **High:** Fix within 1 week
- **Medium:** Fix within 1 month
- **Low:** Fix when convenient

## Dependency Limits
- Max dependencies: 50 (direct)
- Max depth: 5 levels
- No unmaintained packages (> 2 years)
```

---

## Quality Gates (CI/CD)

### Goal: Enforce quality standards

**SonarQube Quality Gate:**

```yaml
# sonar-project.properties
sonar.qualitygate.wait=true

# Quality gate conditions:
# - Coverage > 80%
# - Duplications < 5%
# - Maintainability rating ≥ A
# - Reliability rating ≥ A
# - Security rating ≥ A
# - Security hotspots reviewed = 100%
```

**GitHub Actions Quality Gate:**

```yaml
name: Quality Gate
on: [push, pull_request]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run tests with coverage
        run: |
          pytest --cov=myapp --cov-report=xml
      
      - name: Check coverage threshold
        run: |
          coverage report --fail-under=80
      
      - name: Check complexity
        run: |
          radon cc myapp/ -a -nb
          # Fail if average complexity > 10
      
      - name: Check code smells
        run: |
          pylint myapp/ --fail-under=8.0
      
      - name: Check security
        run: |
          pip-audit --strict
      
      - name: SonarQube Scan
        uses: sonarsource/sonarqube-scan-action@master
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

---

## Quality Dashboard

### Goal: Visualize metrics over time

**Metrics to Track:**

```markdown
## Weekly Dashboard
1. **Test coverage:** 85% → 87% ✅
2. **Technical debt:** 45h → 42h ✅
3. **Code smells:** 120 → 115 ✅
4. **Complexity (avg):** 8.5 → 8.2 ✅
5. **Duplication:** 6% → 5.5% ✅
6. **Vulnerabilities:** 3 → 0 ✅

## Trends (Last 3 Months)
- Coverage: 78% → 85% → 87% (↑ 9%)
- Debt: 60h → 50h → 42h (↓ 30%)
- Smells: 180 → 140 → 115 (↓ 36%)

## Goals (Next Sprint)
- [ ] Coverage > 90%
- [ ] Debt < 40h
- [ ] Smells < 100
- [ ] 0 critical vulnerabilities
```

**Tools:**

```markdown
## SonarQube
- Comprehensive metrics
- Quality gates
- Trend analysis
- Multi-language

## CodeClimate
- Maintainability
- Test coverage
- Duplication
- GitHub integration

## Codecov
- Test coverage
- Coverage diff
- PR comments

## CodeScene
- Behavioral analysis
- Hotspot detection
- Team dynamics
```

---

## Code Quality Checklist

**Measurement:**
- [ ] Complexity measured (< 10 per function)
- [ ] Coverage measured (> 80%)
- [ ] Churn tracked (identify hotspots)
- [ ] Maintainability scored (> 65)

**Issues:**
- [ ] Duplication < 5%
- [ ] Technical debt tracked
- [ ] Code smells identified
- [ ] Dependencies up-to-date

**Automation:**
- [ ] Quality gates in CI/CD
- [ ] Automated dependency updates
- [ ] Coverage reports on PRs
- [ ] Complexity checks on commits

**Process:**
- [ ] Regular debt reduction sprints
- [ ] Code review checklist
- [ ] Quality metrics in retrospectives
- [ ] Team quality goals

---

## Common Quality Mistakes

### Mistake 1: Chasing 100% Coverage
**Problem:** Diminishing returns, false confidence

**Solution:** Target 80-90%, focus on critical paths

### Mistake 2: Ignoring Complexity
**Problem:** High complexity = bugs

**Solution:** Refactor functions > 10 complexity

### Mistake 3: No Quality Gates
**Problem:** Quality degrades over time

**Solution:** Enforce thresholds in CI/CD

### Mistake 4: Metrics Without Action
**Problem:** Tracking but not improving

**Solution:** Set goals, assign owners, track progress

### Mistake 5: One-Time Analysis
**Problem:** Quality is a snapshot, not a trend

**Solution:** Continuous monitoring and improvement

---

## Integration with Other Skills

**Use before measuring:**
- `test-driven-development` - Write tests first
- `requesting-code-review` - Review quality

**Use during improvement:**
- `systematic-debugging` - Fix quality issues
- `incremental-implementation` - Improve gradually

**Use after measuring:**
- `architecture-blueprint` - Redesign if needed
- `performance-optimization` - Optimize hotspots

---

**Remember:** Measure consistently, act on data, balance metrics, automate checks, and improve continuously. Quality is a journey, not a destination.
