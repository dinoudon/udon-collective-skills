---
name: parallel-orchestration
description: Orchestrate multiple AI agents in parallel for complex tasks. Decompose work into independent subtasks, delegate to specialized agents, merge results, and handle failures gracefully.
tags: [orchestration, delegation, parallel, multi-agent, coordination, workflow]
version: 1.0.0
author: Enhanced from multi-agent research + Hermes delegation patterns)
---

# Parallel Orchestration

## Overview

Orchestrate multiple AI agents working in parallel to solve complex tasks faster and more effectively than a single agent.

**Key capabilities:**
- **Task Decomposition** - Break complex work into independent subtasks
- **Parallel Execution** - Run multiple agents simultaneously
- **Result Merging** - Combine outputs into coherent deliverables
- **Failure Handling** - Retry, fallback, and graceful degradation
- **Context Management** - Provide each agent with exactly what they need
- **Progress Tracking** - Monitor and report on parallel workstreams

**When to use:**
- Task has independent subtasks that can run in parallel
- Single agent would take too long
- Different subtasks need different expertise
- You need to maximize throughput

## When to Use

**Always use when:**
- Task has 3+ independent subtasks
- Subtasks don't depend on each other's results
- Time is critical (parallel is faster)
- Different subtasks need different skills

**Examples:**
- Research 10 different technologies simultaneously
- Generate multiple design variations in parallel
- Test multiple approaches to a problem
- Process multiple files independently
- Analyze multiple codebases

**Don't use when:**
- Subtasks depend on each other (use sequential instead)
- Task is simple enough for one agent
- Results need tight integration (coordination overhead > benefit)
- You have < 3 subtasks

## The 5-Phase Orchestration Process

```
┌─────────────────────────────────────────────────────────┐
│           PARALLEL ORCHESTRATION WORKFLOW                │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   PHASE 1    │  Decompose Task
    │  DECOMPOSE   │  - Identify subtasks
    │     TASK     │  - Check independence
    └──────┬───────┘  - Estimate effort
           │
           ▼
    ┌──────────────┐
    │   PHASE 2    │  Prepare Context
    │   PREPARE    │  - Gather shared context
    │   CONTEXT    │  - Prepare per-agent context
    └──────┬───────┘  - Define success criteria
           │
           ▼
    ┌──────────────┐
    │   PHASE 3    │  Delegate Tasks
    │   DELEGATE   │  - Spawn agents in parallel
    │    TASKS     │  - Assign toolsets
    └──────┬───────┘  - Monitor progress
           │
           ▼
    ┌──────────────┐
    │   PHASE 4    │  Handle Results
    │   HANDLE     │  - Collect outputs
    │   RESULTS    │  - Handle failures
    └──────┬───────┘  - Retry if needed
           │
           ▼
    ┌──────────────┐
    │   PHASE 5    │  Merge & Deliver
    │    MERGE     │  - Combine results
    │   & DELIVER  │  - Validate completeness
    └──────────────┘  - Format output
```

## Phase 1: Decompose Task

### Step 1.1: Identify Subtasks

**Break the task into independent pieces:**

<Example: Research Task>
```markdown
**Main Task:** Research the top 10 AI coding agents

**Subtasks (Independent):**
1. Research Claude Code (Anthropic)
2. Research GitHub Copilot (GitHub)
3. Research Cursor (Anysphere)
4. Research Cline (Cline)
5. Research Aider (Aider-AI)
6. Research Continue (Continue)
7. Research Roo-Code (RooCode)
8. Research OpenCode (Anomaly)
9. Research Codex (OpenAI)
10. Research CommandCode (CommandCode)

**Why independent:**
- Each agent can research one tool without needing others' results
- No shared state or dependencies
- Results can be merged at the end
```
</Example>

<Example: Code Analysis Task>
```markdown
**Main Task:** Analyze security vulnerabilities in a monorepo

**Subtasks (Independent):**
1. Scan frontend/ for XSS vulnerabilities
2. Scan backend/ for SQL injection
3. Scan api/ for authentication issues
4. Scan infrastructure/ for misconfigurations
5. Check dependencies for known CVEs
6. Review secrets and credentials
7. Analyze CORS and CSP policies
8. Check rate limiting implementation

**Why independent:**
- Each directory can be analyzed separately
- Different security concerns per component
- Results are aggregated into final report
```
</Example>

### Step 1.2: Verify Independence

**Check that subtasks are truly independent:**

**✅ Independent (Good for parallel):**
- Subtask A doesn't need Subtask B's output
- Subtasks can run in any order
- Subtasks don't share mutable state
- Subtasks don't conflict with each other

**❌ Dependent (Use sequential instead):**
- Subtask B needs Subtask A's result
- Subtasks must run in specific order
- Subtasks modify shared resources
- Subtasks have race conditions

<Bad Example>
```markdown
**Task:** Build a user authentication system

**Subtasks (DEPENDENT - Don't parallelize):**
1. Design database schema
2. Implement user model (needs schema)
3. Create API endpoints (needs model)
4. Build frontend forms (needs API)
5. Write tests (needs everything)

**Why dependent:**
- Each step needs the previous step's output
- Must run sequentially
```
</Bad Example>

### Step 1.3: Estimate Effort

**Estimate time for each subtask:**

```markdown
## Effort Estimation

| Subtask | Estimated Time | Complexity |
|---------|---------------|------------|
| Research Claude Code | 15 min | Low |
| Research GitHub Copilot | 15 min | Low |
| Research Cursor | 15 min | Low |
| Research Cline | 15 min | Low |
| Research Aider | 15 min | Low |
| Research Continue | 15 min | Low |
| Research Roo-Code | 15 min | Low |
| Research OpenCode | 15 min | Low |
| Research Codex | 15 min | Low |
| Research CommandCode | 15 min | Low |

**Total Sequential:** 150 minutes (2.5 hours)
**Total Parallel (3 agents):** 50 minutes (0.8 hours)
**Speedup:** 3x faster
```

**Parallel execution makes sense when:**
- Total sequential time > 30 minutes
- Speedup > 2x
- Coordination overhead < 20% of time saved

## Phase 2: Prepare Context

### Step 2.1: Gather Shared Context

**Information all agents need:**

```markdown
## Shared Context

**Project:** AI Coding Agents Research
**Goal:** Compare features, pricing, and capabilities
**Output Format:** Markdown report with structured sections
**Deadline:** 1 hour
**Quality Bar:** Include pricing, key features, GitHub stars, and unique selling points

**Research Template:**
```markdown
# [Tool Name]

## Overview
- **Company:** [Company name]
- **GitHub:** [Repository URL]
- **Stars:** [Star count]
- **Website:** [Official website]

## Key Features
- [Feature 1]
- [Feature 2]
- [Feature 3]

## Pricing
- [Pricing tier 1]
- [Pricing tier 2]

## Unique Selling Points
- [USP 1]
- [USP 2]

## Limitations
- [Limitation 1]
- [Limitation 2]
```
```

### Step 2.2: Prepare Per-Agent Context

**Specific information for each agent:**

```typescript
const tasks = [
  {
    goal: "Research Claude Code (Anthropic)",
    context: `
      Shared context: ${sharedContext}
      
      Specific focus:
      - Anthropic's official Claude Code CLI
      - Integration with Claude API
      - Unique features vs competitors
      - Pricing and availability
      
      Search queries to try:
      - "claude code anthropic"
      - "anthropic claude code cli"
      - site:github.com claude code
    `
  },
  {
    goal: "Research GitHub Copilot",
    context: `
      Shared context: ${sharedContext}
      
      Specific focus:
      - GitHub's official Copilot product
      - IDE integrations
      - Business vs individual pricing
      - Recent updates and features
      
      Search queries to try:
      - "github copilot features"
      - "github copilot pricing 2026"
      - site:github.com copilot
    `
  },
  // ... more tasks
];
```

### Step 2.3: Define Success Criteria

**How to know each subtask succeeded:**

```markdown
## Success Criteria (Per Subtask)

**Required:**
- [ ] Tool name and company identified
- [ ] GitHub repository found (if open source)
- [ ] Key features listed (minimum 3)
- [ ] Pricing information found
- [ ] Output follows template format

**Optional (nice to have):**
- [ ] Star count and popularity metrics
- [ ] User reviews or testimonials
- [ ] Comparison with competitors
- [ ] Recent updates or roadmap

**Failure conditions:**
- Tool doesn't exist or is deprecated
- No reliable information found
- Output is incomplete or malformed
```

## Phase 3: Delegate Tasks

### Step 3.1: Spawn Agents in Parallel

**Use delegate_task with tasks array:**

```typescript
const results = await delegate_task({
  tasks: [
    {
      goal: "Research Claude Code (Anthropic)",
      context: sharedContext + specificContext1,
      toolsets: ["web", "terminal"]
    },
    {
      goal: "Research GitHub Copilot",
      context: sharedContext + specificContext2,
      toolsets: ["web", "terminal"]
    },
    {
      goal: "Research Cursor",
      context: sharedContext + specificContext3,
      toolsets: ["web", "terminal"]
    }
    // ... up to max_concurrent_children (check config)
  ]
});
```

**Important limits:**
- Check `delegation.max_concurrent_children` in config
- Default is often 3 concurrent agents
- Batch larger tasks into groups

### Step 3.2: Batch Large Task Sets

**If you have > max_concurrent_children subtasks:**

```typescript
// Example: 10 tasks, max 3 concurrent
const allTasks = [task1, task2, task3, task4, task5, task6, task7, task8, task9, task10];
const batchSize = 3; // max_concurrent_children
const batches = [];

for (let i = 0; i < allTasks.length; i += batchSize) {
  batches.push(allTasks.slice(i, i + batchSize));
}

const allResults = [];
for (const batch of batches) {
  const batchResults = await delegate_task({ tasks: batch });
  allResults.push(...batchResults);
}
```

**Batching strategy:**
- Group similar tasks together
- Balance effort across batches
- Consider dependencies between batches

### Step 3.3: Assign Appropriate Toolsets

**Give each agent only what they need:**

```typescript
const tasks = [
  {
    goal: "Research online documentation",
    toolsets: ["web"] // Only needs web search
  },
  {
    goal: "Analyze local codebase",
    toolsets: ["file", "terminal"] // Only needs file access
  },
  {
    goal: "Test API endpoints",
    toolsets: ["web", "terminal"] // Needs both
  }
];
```

**Common toolset combinations:**
- **Research:** `["web"]`
- **Code analysis:** `["file", "terminal"]`
- **Testing:** `["web", "terminal", "file"]`
- **Full access:** `["web", "terminal", "file", "browser"]`

## Phase 4: Handle Results

### Step 4.1: Collect Outputs

**Process results array:**

```typescript
const results = await delegate_task({ tasks });

// Results structure:
// [
//   { goal: "...", summary: "...", status: "success" },
//   { goal: "...", summary: "...", status: "success" },
//   { goal: "...", summary: "...", status: "failed", error: "..." }
// ]

const successful = results.filter(r => r.status === "success");
const failed = results.filter(r => r.status === "failed");

console.log(`✅ Successful: ${successful.length}`);
console.log(`❌ Failed: ${failed.length}`);
```

### Step 4.2: Handle Failures

**Strategies for failed subtasks:**

**Strategy 1: Retry with modified context**
```typescript
const failedTasks = results
  .filter(r => r.status === "failed")
  .map(r => ({
    goal: r.goal,
    context: r.context + "\n\nPrevious attempt failed. Try a different approach."
  }));

if (failedTasks.length > 0) {
  const retryResults = await delegate_task({ tasks: failedTasks });
  // Merge retry results with successful results
}
```

**Strategy 2: Fallback to manual handling**
```typescript
if (failed.length > 0) {
  console.log("Some tasks failed. Handling manually:");
  for (const failure of failed) {
    console.log(`- ${failure.goal}: ${failure.error}`);
    // Handle manually or skip
  }
}
```

**Strategy 3: Partial success**
```typescript
if (successful.length >= minimumRequired) {
  console.log(`Proceeding with ${successful.length} successful results`);
  // Continue with partial results
} else {
  throw new Error(`Insufficient results: ${successful.length}/${minimumRequired}`);
}
```

### Step 4.3: Validate Results

**Check that results meet success criteria:**

```typescript
function validateResult(result: AgentResult): boolean {
  const summary = result.summary;
  
  // Check required fields
  const hasToolName = /# .+/.test(summary);
  const hasFeatures = /## Key Features/.test(summary);
  const hasPricing = /## Pricing/.test(summary);
  
  return hasToolName && hasFeatures && hasPricing;
}

const validResults = successful.filter(validateResult);
const invalidResults = successful.filter(r => !validateResult(r));

if (invalidResults.length > 0) {
  console.log(`⚠️ ${invalidResults.length} results are incomplete`);
  // Retry or handle manually
}
```

## Phase 5: Merge & Deliver

### Step 5.1: Combine Results

**Merge individual outputs into final deliverable:**

<Example: Research Report>
```typescript
function mergeResearchResults(results: AgentResult[]): string {
  const header = `# AI Coding Agents Research Report
**Date:** ${new Date().toISOString().split('T')[0]}
**Tools Analyzed:** ${results.length}

## Executive Summary
This report compares ${results.length} AI coding agents across features, pricing, and capabilities.

---

`;

  const body = results
    .map(r => r.summary)
    .join('\n\n---\n\n');

  const footer = `
---

## Comparison Table

| Tool | Company | Stars | Pricing | Key Feature |
|------|---------|-------|---------|-------------|
${results.map(r => extractTableRow(r)).join('\n')}

## Recommendations

Based on the analysis:
1. **Best for individuals:** ${recommendForIndividuals(results)}
2. **Best for teams:** ${recommendForTeams(results)}
3. **Best free option:** ${recommendFree(results)}
`;

  return header + body + footer;
}
```
</Example>

<Example: Security Audit>
```typescript
function mergeSecurityResults(results: AgentResult[]): string {
  const criticalIssues = results.flatMap(r => extractIssues(r, 'critical'));
  const highIssues = results.flatMap(r => extractIssues(r, 'high'));
  const mediumIssues = results.flatMap(r => extractIssues(r, 'medium'));
  
  return `# Security Audit Report

## Summary
- **Critical Issues:** ${criticalIssues.length}
- **High Issues:** ${highIssues.length}
- **Medium Issues:** ${mediumIssues.length}

## Critical Issues (Fix Immediately)
${criticalIssues.map(formatIssue).join('\n\n')}

## High Issues (Fix Within 24 Hours)
${highIssues.map(formatIssue).join('\n\n')}

## Medium Issues (Fix Within 1 Week)
${mediumIssues.map(formatIssue).join('\n\n')}

## Detailed Reports by Component
${results.map(r => r.summary).join('\n\n---\n\n')}
`;
}
```
</Example>

### Step 5.2: Validate Completeness

**Ensure final output is complete:**

```typescript
function validateFinalOutput(output: string, expectedSections: string[]): boolean {
  return expectedSections.every(section => output.includes(section));
}

const expectedSections = [
  '# AI Coding Agents Research Report',
  '## Executive Summary',
  '## Comparison Table',
  '## Recommendations'
];

if (!validateFinalOutput(finalReport, expectedSections)) {
  console.error('Final report is incomplete');
  // Add missing sections or retry
}
```

### Step 5.3: Format and Deliver

**Format for the target audience:**

```typescript
// Save to file
await write_file({
  path: '/tmp/ai-coding-agents-report.md',
  content: finalReport
});

// Send to user
await send_message({
  target: 'telegram',
  message: `# Research Complete!\n\n${summary}\n\nMEDIA:/tmp/ai-coding-agents-report.md`
});
```

## Advanced Patterns

### Pattern 1: Map-Reduce

**Map phase (parallel):**
```typescript
const mapResults = await delegate_task({
  tasks: items.map(item => ({
    goal: `Process ${item.name}`,
    context: `Process this item: ${JSON.stringify(item)}`
  }))
});
```

**Reduce phase (sequential):**
```typescript
const finalResult = mapResults.reduce((acc, result) => {
  return mergeResults(acc, result);
}, initialValue);
```

### Pattern 2: Fan-Out / Fan-In

**Fan-out (parallel):**
```typescript
const [results1, results2, results3] = await Promise.all([
  delegate_task({ tasks: batch1 }),
  delegate_task({ tasks: batch2 }),
  delegate_task({ tasks: batch3 })
]);
```

**Fan-in (merge):**
```typescript
const allResults = [...results1, ...results2, ...results3];
const finalOutput = mergeAll(allResults);
```

### Pattern 3: Pipeline with Parallel Stages

**Stage 1 (parallel):**
```typescript
const stage1Results = await delegate_task({ tasks: stage1Tasks });
```

**Stage 2 (parallel, depends on stage 1):**
```typescript
const stage2Tasks = stage1Results.map(r => ({
  goal: `Process ${r.output}`,
  context: `Input from stage 1: ${r.summary}`
}));
const stage2Results = await delegate_task({ tasks: stage2Tasks });
```

### Pattern 4: Speculative Execution

**Try multiple approaches in parallel, use first success:**

```typescript
const approaches = [
  { goal: "Solve using approach A", context: "..." },
  { goal: "Solve using approach B", context: "..." },
  { goal: "Solve using approach C", context: "..." }
];

const results = await delegate_task({ tasks: approaches });
const firstSuccess = results.find(r => r.status === "success");

if (firstSuccess) {
  return firstSuccess.summary;
} else {
  throw new Error("All approaches failed");
}
```

## Orchestration Checklist

**Before delegating:**
- [ ] Task decomposed into independent subtasks
- [ ] Independence verified (no dependencies)
- [ ] Shared context prepared
- [ ] Per-agent context prepared
- [ ] Success criteria defined
- [ ] Toolsets assigned appropriately
- [ ] Batch size within limits (max_concurrent_children)

**After delegation:**
- [ ] All results collected
- [ ] Failures handled (retry or fallback)
- [ ] Results validated against criteria
- [ ] Results merged into final output
- [ ] Final output validated for completeness
- [ ] Output formatted and delivered

## Common Pitfalls

### Pitfall 1: False Independence

**Symptom:** Subtasks fail because they need each other's results

**Solution:** Verify independence before parallelizing

### Pitfall 2: Context Overload

**Symptom:** Agents get too much context, waste time processing irrelevant info

**Solution:** Give each agent only what they need

### Pitfall 3: Ignoring Failures

**Symptom:** Final output is incomplete because failed subtasks were ignored

**Solution:** Always handle failures (retry, fallback, or partial success)

### Pitfall 4: Poor Merging

**Symptom:** Final output is disjointed or inconsistent

**Solution:** Design merge strategy upfront, validate completeness

### Pitfall 5: Exceeding Limits

**Symptom:** Delegation fails because too many concurrent agents

**Solution:** Batch tasks to stay within max_concurrent_children

## Integration with Other Skills

**Use before orchestration:**
- `spec-driven-development-enhanced` - Define what to build
- `architecture-blueprint` - Design the system

**Use during orchestration:**
- `tdd-iron-law` - Each agent follows TDD
- `security-review-owasp` - Each agent checks security

**Use after orchestration:**
- `github-pr-workflow` - Submit merged results as PR

## Real-World Examples

### Example 1: Multi-Repo Analysis

```typescript
const repos = [
  'facebook/react',
  'vercel/next.js',
  'vuejs/vue',
  'angular/angular',
  'sveltejs/svelte'
];

const results = await delegate_task({
  tasks: repos.map(repo => ({
    goal: `Analyze ${repo} repository`,
    context: `
      Analyze this repository: ${repo}
      
      Report on:
      - Architecture patterns
      - Testing strategy
      - Performance optimizations
      - Security practices
      - Documentation quality
    `,
    toolsets: ['web', 'terminal']
  }))
});

const report = mergeAnalysisResults(results);
```

### Example 2: Parallel Testing

```typescript
const testSuites = [
  'tests/unit/',
  'tests/integration/',
  'tests/e2e/',
  'tests/performance/'
];

const results = await delegate_task({
  tasks: testSuites.map(suite => ({
    goal: `Run tests in ${suite}`,
    context: `Run all tests in ${suite} and report results`,
    toolsets: ['terminal', 'file']
  }))
});

const allPassed = results.every(r => r.status === 'success');
```

### Example 3: Multi-Language Documentation

```typescript
const languages = ['en', 'es', 'fr', 'de', 'ja', 'zh'];

const results = await delegate_task({
  tasks: languages.map(lang => ({
    goal: `Translate documentation to ${lang}`,
    context: `
      Source: ${englishDocs}
      Target language: ${lang}
      Maintain technical accuracy and formatting
    `,
    toolsets: ['file']
  }))
});
```

---

**Remember:** Parallel orchestration is about maximizing throughput while maintaining quality. Decompose wisely, delegate clearly, and merge carefully.
