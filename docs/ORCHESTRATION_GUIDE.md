# Roo Code Orchestration Guide

**Complete Guide to TDD & Design Review Orchestration Modes**

**Version:** 1.0
**Date:** October 2025
**For:** Roo Code 3.15+ with Boomerang Tasks support

---

## Table of Contents

1. [Roo Code Features Overview](#roo-code-features-overview)
2. [How Orchestration Works](#how-orchestration-works)
3. [Roo Code Limitations & Constraints](#roo-code-limitations--constraints)
4. [Maximizing Effectiveness](#maximizing-effectiveness)
5. [Practical Workflows](#practical-workflows)
6. [Advanced Patterns](#advanced-patterns)
7. [Troubleshooting](#troubleshooting)

---

## Roo Code Features Overview

### Custom Modes

Roo Code allows you to create specialized modes that tailor the AI's behavior for specific tasks. Each custom mode is defined in YAML format with these key components:

```yaml
customModes:
  - slug: my-mode-name              # Unique identifier
    name: 🎯 Display Name           # Name shown in UI
    roleDefinition: >               # Who the mode is
      You are a specialized...
    whenToUse: >                    # When to use this mode
      Use this mode when...
    description: Short summary      # Brief description
    groups:                         # Tool access permissions
      - read
      - edit
    customInstructions: >           # Detailed instructions
      Your detailed instructions...
    source: project                 # global or project
    rulesFiles:                     # Project-specific rules
      - relativePath: AGENTS.md
        content: >
          Non-obvious rules...
```

### Tool Access Groups

Roo Code has 5 tool groups that control what a mode can do:

| Group | Capabilities | Example Tools |
|-------|-------------|---------------|
| `read` | Read files, search code | Read, Grep, Glob |
| `edit` | Edit files | Edit, Write, NotebookEdit |
| `browser` | Fetch web content | WebFetch, WebSearch |
| `command` | Execute terminal commands | Bash |
| `mcp` | Use Model Context Protocol integrations | Playwright MCP, custom MCPs |

**Tool Group Combinations:**
- **Full Access:** `[read, edit, browser, command, mcp]` - Can do everything
- **Read-Only:** `[read]` - Analysis and recommendations only
- **Read + Edit:** `[read, edit]` - Can modify files but not execute commands
- **Read + Edit + Command:** `[read, edit, command]` - Development work without browser
- **Review Tools:** `[read, browser, command, mcp]` - Testing and validation (no file edits)

### Boomerang Tasks (Task Orchestration)

**Boomerang Tasks** allow one mode to launch other modes as subtasks and receive their results back.

**Key Concepts:**
1. **Orchestrator Mode:** Launches subtasks using `new_task` tool
2. **Specialist Modes:** Execute specific tasks, return results via `attempt_completion`
3. **Result Passing:** Orchestrator receives specialist results and can use them in subsequent tasks

**Syntax:**

```javascript
// Orchestrator launches a specialist
const result = new_task(
  mode: "specialist-mode-slug",
  message: "Task description with context. Use attempt_completion when done."
)

// Specialist completes and returns
// (Inside specialist mode, when task is done)
attempt_completion(
  result: "Detailed results of the work performed..."
)

// Orchestrator receives result and continues
const nextResult = new_task(
  mode: "another-specialist",
  message: `Next task based on previous: ${result}...`
)
```

### Built-in Orchestrator Mode (2025 Update)

Roo Code 3.15+ includes a built-in 🪃 **Orchestrator mode** with important constraints:

**What it CAN do:**
- Launch other modes via `new_task`
- Receive results via `attempt_completion`
- Aggregate results into reports
- Make decisions about which specialists to launch

**What it CANNOT do (enforced by Roo Code):**
- Read files directly
- Write or edit files
- Execute terminal commands
- Use MCP servers

**Why this matters:** Orchestrators are **coordinators**, not **executors**. They delegate all actual work to specialist modes.

**Tool Access for Orchestrators:**
- Recommended: `groups: [read]` (minimal, for basic context)
- Some orchestrators: `groups: [read, command]` (if they need to check system state)
- Never: `[edit]` group (orchestrators should never modify files)

---

## How Orchestration Works

### Orchestrator vs Specialist Pattern

**Traditional Mode Switching (Manual):**
```
User manually switches modes:
1. Switch to Next.js Architect → Design component
2. Switch to TDD Engineer → Write tests
3. Switch to Code Reviewer → Review code
4. User synthesizes results manually
```

**Orchestrator Pattern (Automated):**
```
User uses TDD Frontend Orchestrator once:
1. Orchestrator launches Next.js Architect → Receives design
2. Orchestrator launches TDD Engineer → Receives tests
3. Orchestrator launches implementation → Receives code
4. Orchestrator launches Code Reviewer → Receives review
5. Orchestrator aggregates all results → Comprehensive report
```

### Sequential vs Parallel Execution

**Sequential Execution** (when tasks depend on each other):
```javascript
// Task 2 needs results from Task 1
const architecture = new_task(
  mode: "nextjs-architect",
  message: "Design component architecture..."
)

// Wait for architecture to complete, then use its results
const tests = new_task(
  mode: "tdd-engineer",
  message: `Write tests based on: ${architecture}...`
)
```

**Parallel Execution** (when tasks are independent):
```javascript
// These can run simultaneously
const [architecture, security] = Promise.all([
  new_task(mode: "nextjs-architect", message: "Design..."),
  new_task(mode: "code-reviewer", message: "Analyze security...")
])
```

**Example from TDD Backend Orchestrator:**
```javascript
// PARALLEL: Architecture and Database design are independent
const architectTask = new_task(mode: "laravel-architect", ...)
const databaseTask = new_task(mode: "database-engineer", ...)

// SEQUENTIAL: Tests need both architecture AND database to be complete
const tddTask = new_task(
  mode: "tdd-engineer",
  message: `Write tests based on:
    Architecture: ${architectTask.result}
    Database: ${databaseTask.result}
  `
)
```

### Result Aggregation

Orchestrators collect results from all specialists and format them into comprehensive reports:

```markdown
# TDD Frontend Development Summary

## RED Phase (Architecture + Tests)
- Architecture: [Results from Next.js Architect]
- Tests Written: [Results from TDD Engineer]

## GREEN Phase (Implementation)
- Implementation: [Results from Next.js Architect]
- Tests Passing: ✅ 25/25

## REFACTOR Phase (Review)
- Code Review: [Results from Code Reviewer]
- Blockers: 0
- High-Priority: 2 (refactored)

## Final Status: Ready for Merge ✅
```

### Context Passing Between Tasks

**Explicit Context Passing:**
```javascript
// Orchestrator explicitly passes context from previous task
const architecture = new_task(
  mode: "nextjs-architect",
  message: "Design user profile component with form validation..."
)

// Context from architecture is embedded in the message
const tests = new_task(
  mode: "tdd-engineer",
  message: `Write tests for the following architecture:

  ${architecture}

  Focus on form validation rules...`
)
```

**Best Practices:**
- Be explicit about what context specialist needs
- Include relevant results from previous tasks
- Specify what format you want results in
- Always end specialist messages with "Use attempt_completion when done."

---

## Roo Code Limitations & Constraints

### 1. Orchestrator Read-Only Constraint (2025)

**Limitation:** Orchestrators with Boomerang Tasks should use `groups: [read]` or `[read, command]` only.

**Why:** Roo Code's built-in Orchestrator mode (3.15+) enforces strict separation between coordination and execution.

**Workaround:** Delegate ALL file operations to specialists:
```javascript
// ❌ DON'T: Try to edit files in orchestrator
Edit(file_path: "...", old_string: "...", new_string: "...")

// ✅ DO: Launch specialist to edit files
new_task(
  mode: "nextjs-architect",
  message: "Implement the component based on tests. Edit src/components/..."
)
```

### 2. Tool Access Restrictions

**Limitation:** Modes can only use tools in their assigned `groups`.

**Example Issue:**
```yaml
# Orchestrator with only read access
groups: [read]

# This will FAIL because orchestrator can't execute commands
customInstructions: >
  Run tests with: npm test
```

**Workaround:** Delegate to specialist with command access:
```javascript
new_task(
  mode: "tdd-engineer",  // Has [read, edit, command] access
  message: "Run the test suite and report results..."
)
```

### 3. Context Size Limitations

**Limitation:** Very large result strings can hit context limits when passing between tasks.

**Symptoms:**
- Orchestrator truncates results
- Specialist doesn't receive full context
- "Context length exceeded" errors

**Workaround:**
```javascript
// ❌ DON'T: Pass entire file contents
const architecture = new_task(
  mode: "nextjs-architect",
  message: `Here's the entire codebase: ${allFiles}...`
)

// ✅ DO: Pass summaries and file paths
const architecture = new_task(
  mode: "nextjs-architect",
  message: `Design based on existing patterns in:
  - src/components/forms/*.tsx
  - Focus on patterns used in UserForm.tsx

  Summarize key architecture decisions.`
)
```

### 4. Specialist Must Use attempt_completion

**Limitation:** If specialist doesn't call `attempt_completion`, orchestrator never receives results.

**Best Practice:** Always include this instruction:
```javascript
new_task(
  mode: "tdd-engineer",
  message: `Your tasks...

  Use attempt_completion when done.  // ← Critical!`
)
```

### 5. No Real-Time Specialist Monitoring

**Limitation:** Orchestrator can't see specialist's progress in real-time. It waits for final result.

**Implication:** Can't interrupt or modify specialist mid-task.

**Workaround:** Break large tasks into smaller subtasks:
```javascript
// ❌ DON'T: Launch one huge task
new_task(message: "Implement entire feature with 20 components...")

// ✅ DO: Break into phases
new_task(message: "Implement core component...")
new_task(message: "Implement form validation...")
new_task(message: "Implement error handling...")
```

---

## Maximizing Effectiveness

### When to Use Orchestrators

**✅ Use Orchestrators When:**
- **Multi-step workflows:** RED-GREEN-REFACTOR, comprehensive design review
- **Consistent process needed:** Enforce TDD methodology, quality gates
- **Multiple specialists required:** Architecture + Tests + Implementation + Review
- **Parallel execution beneficial:** 6 design reviewers running simultaneously
- **Aggregated reporting desired:** Single comprehensive report from multiple sources

**❌ Use Manual Mode Switching When:**
- **Single focused task:** "Write tests for this component"
- **Exploratory work:** Trying different approaches
- **Quick iterations:** Rapid prototyping
- **Learning/debugging:** Understanding how individual modes work
- **Simple one-off tasks:** "Review this file for security issues"

### Structuring Requirements for Best Results

**Poor Requirement (vague):**
```
User: "Build a user profile feature"

Orchestrator: Unclear what to build, which framework, what tests, etc.
```

**Good Requirement (specific):**
```
User: "Build a Next.js user profile edit form with:
- Fields: name, email, bio (textarea)
- Client-side validation (Zod)
- Server Action for submission
- Loading states and error handling
- TypeScript strict mode (no 'any')
- Tests for validation and submission flow"

Orchestrator: Clear requirements, knows exactly what to coordinate
```

**Best Practices for Requirements:**
1. **Specify framework:** Next.js vs React, Laravel vs FastAPI
2. **Define scope:** What features exactly
3. **State constraints:** TypeScript rules, test coverage targets
4. **Identify edge cases:** Error states, loading, empty data
5. **Mention non-functional requirements:** Performance, accessibility

### Debugging Orchestration Workflows

**Issue:** Orchestrator didn't launch expected specialist

**Debug:**
1. Check orchestrator's `customInstructions` - does it have logic to launch that specialist?
2. Check mode slug - is the specialist mode actually imported?
3. Check conditional logic - did orchestrator decide not to launch based on context?

**Issue:** Specialist didn't return results

**Debug:**
1. Did specialist's task message include "Use attempt_completion when done"?
2. Check specialist's tool access - can it actually perform the requested task?
3. Look for errors in specialist's execution

**Issue:** Results not passed to next specialist

**Debug:**
1. Check orchestrator explicitly passes context: `message: \`Based on: ${previousResult}\``
2. Verify previous task completed before next one launched (sequential dependency)

**Issue:** Orchestrator tries to edit files and fails

**Debug:**
1. Check orchestrator's `groups` - should be `[read]` only
2. Move file operations to specialist modes
3. Update orchestrator instructions to delegate file operations

### Performance Optimization

**1. Maximize Parallel Execution:**
```javascript
// ❌ SLOW: Sequential when tasks are independent
const interaction = new_task(mode: "interaction-tester", ...)
const responsive = new_task(mode: "responsive-auditor", ...)
const visual = new_task(mode: "visual-reviewer", ...)
// Total time: T1 + T2 + T3

// ✅ FAST: Parallel execution
const results = Promise.all([
  new_task(mode: "interaction-tester", ...),
  new_task(mode: "responsive-auditor", ...),
  new_task(mode: "visual-reviewer", ...)
])
// Total time: max(T1, T2, T3)
```

**2. Minimize Context Size:**
```javascript
// ❌ SLOW: Large context passed between tasks
new_task(message: `Full file contents: ${10000_line_file}...`)

// ✅ FAST: Minimal context, let specialist read files
new_task(message: `Review patterns in src/components/forms/*.tsx`)
```

**3. Use Conditional Launching:**
```javascript
// Only launch refactor if needed
if (reviewTask.blockers.length > 0) {
  new_task(mode: "nextjs-architect", message: "Refactor blockers...")
}
// Don't launch if no blockers (saves time)
```

---

## Practical Workflows

### TDD Frontend Development Workflow

**Mode:** `tdd-frontend-orchestrator`

**User Input:**
```
"Build a product search component with:
- Search input with debounce (300ms)
- Results list with pagination
- Loading spinner
- Empty state for no results
- Error handling for API failures
Using Next.js with TypeScript strict mode"
```

**Orchestrator Execution:**

**Phase 1: RED (Architecture + Tests)**
```javascript
// Step 1: Design architecture
const architecture = new_task(
  mode: "nextjs-architect",
  message: "Design product search component architecture..."
)
// Returns: Component structure, TypeScript interfaces, props, state management

// Step 2: Write failing tests
const tests = new_task(
  mode: "tdd-engineer",
  message: `Write failing tests for: ${architecture}
  - Test debounce behavior
  - Test pagination
  - Test loading states
  - Test error handling
  - Test empty state`
)
// Returns: Test suite (all failing)
```

**Phase 2: GREEN (Implementation)**
```javascript
// Step 3: Implement to pass tests
const implementation = new_task(
  mode: "nextjs-architect",
  message: `Implement component to pass tests:
  Architecture: ${architecture}
  Tests: ${tests}

  Minimum code to pass all tests.`
)
// Returns: Component implementation, all tests passing
```

**Phase 3: REFACTOR (Review)**
```javascript
// Step 4: Code review
const review = new_task(
  mode: "code-health-reviewer",
  message: `Review implementation:
  Code: ${implementation}
  Tests: ${tests}

  Check for: TypeScript quality, component reuse, design tokens`
)
// Returns: Findings (Blocker, High, Medium)

// Step 5: Refactor if needed
if (review.blockers.length > 0 || review.highPriority.length > 0) {
  const refactored = new_task(
    mode: "nextjs-architect",
    message: `Refactor based on review: ${review}`
  )
}
```

**Phase 4: VALIDATE (Optional - UI-heavy features)**
```javascript
// Step 6: Design review (optional)
const designReview = new_task(
  mode: "orchestrator-design-review",
  message: "Run comprehensive design review on product search component"
)
// Launches 6 design specialists in parallel
```

**Final Output:**
```markdown
# TDD Frontend Development Summary

**Feature:** Product Search Component
**Framework:** Next.js with TypeScript strict mode
**Status:** ✅ Ready for Merge

## RED Phase
- Architecture: Component structure with debounce, pagination, error handling
- Tests: 12 unit tests, 3 integration tests (all failing initially)

## GREEN Phase
- Implementation: ProductSearch.tsx (150 lines)
- Tests Passing: ✅ 15/15
- TypeScript: ✅ Strict mode, 0 'any' types

## REFACTOR Phase
- Blockers: 0
- High-Priority: 1 (fixed: extracted useDebounce custom hook)
- Medium-Priority: 2 (deferred to backlog)

## VALIDATE Phase (Design Review)
- Interaction Testing: ✅ PASS
- Responsive Design: ✅ PASS
- Accessibility: ✅ PASS (WCAG 2.1 AA)

**Next Steps:** Deploy to staging, monitor performance
```

### TDD Backend Development Workflow

**Mode:** `tdd-backend-orchestrator`

**User Input:**
```
"Build REST API for blog posts with:
- CRUD endpoints (create, read, update, delete)
- Pagination for list endpoint
- Search by title/content
- Published/draft status
- User authentication required
Using Laravel with PostgreSQL"
```

**Orchestrator Execution:**

**Phase 1: FOUNDATION (Architecture + Database)**
```javascript
// Parallel execution - independent tasks
const [architecture, database] = Promise.all([
  new_task(
    mode: "laravel-architect",
    message: "Design API endpoints for blog CRUD..."
  ),
  new_task(
    mode: "database-engineer",
    message: "Design database schema for blog posts..."
  )
])
// architecture returns: API routes, controllers, services, validation rules
// database returns: Migration, model, relationships, indexes
```

**Phase 2: RED (Tests)**
```javascript
const tests = new_task(
  mode: "tdd-engineer",
  message: `Write failing tests for blog API:

  API Design: ${architecture}
  Database Schema: ${database}

  Test categories:
  - Feature tests (HTTP endpoints)
  - Unit tests (services)
  - Database tests (models, migrations)
  - Authorization tests (policies)`
)
// Returns: Complete test suite (all failing), target: 80%+ coverage
```

**Phase 3: GREEN (Implementation)**
```javascript
const implementation = new_task(
  mode: "laravel-architect",
  message: `Implement blog API to pass all tests:

  Architecture: ${architecture}
  Database: ${database}
  Tests: ${tests}

  Tasks:
  1. Run migrations
  2. Implement Post model
  3. Implement controllers
  4. Implement services (business logic)
  5. Implement validation (Form Requests)
  6. Implement policies (authorization)
  7. Verify all tests pass`
)
// Returns: Full implementation, migrations applied, tests passing
```

**Phase 4: REFACTOR (Review + Security)**
```javascript
const review = new_task(
  mode: "code-reviewer",
  message: `Review blog API implementation:

  Code: ${implementation}
  Tests: ${tests}

  Focus:
  - Security (SQL injection, XSS, authorization)
  - N+1 query issues
  - Input validation completeness
  - Error handling`
)
// Returns: Security findings, performance issues, code quality

// Refactor if critical issues found
if (review.blockers.length > 0) {
  const refactored = new_task(
    mode: "laravel-architect",
    message: `Fix critical issues: ${review.blockers}`
  )
}
```

**Phase 5: VALIDATE (API Contract + Database)**
```javascript
// Parallel validation - independent checks
const [apiValidation, dbValidation] = Promise.all([
  new_task(
    mode: "tdd-engineer",
    message: "Validate API contract compliance (OpenAPI spec)..."
  ),
  new_task(
    mode: "database-engineer",
    message: "Verify database optimization (no N+1, indexes used)..."
  )
])
```

**Final Output:**
```markdown
# TDD Backend Development Summary

**Feature:** Blog Posts REST API
**Framework:** Laravel 11 + PostgreSQL
**Status:** ✅ Ready for Staging Deployment

## FOUNDATION Phase
- API Design: 5 endpoints (POST, GET, GET/:id, PUT/:id, DELETE/:id)
- Database: posts table, users relationship, full-text search index

## RED Phase
- Feature tests: 15
- Unit tests: 8
- Database tests: 6
- Authorization tests: 4
- **Total:** 33 tests (all failing initially)

## GREEN Phase
- Implementation: ✅ Complete
- Migrations: ✅ Applied
- Tests Passing: ✅ 33/33
- Coverage: 87% (exceeds 80% target)

## REFACTOR Phase
- Security: ✅ 0 blockers
- Performance: ✅ N+1 queries fixed (eager loading)
- Validation: ✅ All inputs validated

## VALIDATE Phase
- API Contract: ✅ Matches OpenAPI spec
- Database: ✅ Indexes optimized, query time <50ms

**Next Steps:** Deploy to staging, run integration tests
```

### Design Review Workflow

**Mode:** `orchestrator-design-review`

**User Input:**
```
"Run comprehensive design review on the user dashboard at /dashboard"
```

**Orchestrator Execution:**

**Parallel Launch (All Specialists Run Simultaneously):**
```javascript
const results = Promise.all([
  // Specialist 1: Interaction testing
  new_task(
    mode: "interaction-tester",
    message: "Test user flows and interactive states on /dashboard"
  ),

  // Specialist 2: Responsive design
  new_task(
    mode: "responsive-auditor",
    message: "Test /dashboard at 3 viewports: 1440px, 768px, 375px"
  ),

  // Specialist 3: Visual polish
  new_task(
    mode: "visual-reviewer",
    message: "Check visual consistency on /dashboard"
  ),

  // Specialist 4: Accessibility
  new_task(
    mode: "accessibility-auditor",
    message: "WCAG 2.1 AA compliance testing on /dashboard"
  ),

  // Specialist 5: Robustness
  new_task(
    mode: "robustness-tester",
    message: "Test edge cases and error handling on /dashboard"
  ),

  // Specialist 6: Code health
  new_task(
    mode: "code-health-reviewer",
    message: "Review component reuse and design tokens for /dashboard"
  )
])
// All 6 specialists complete in time of 1 (parallel execution)
```

**Aggregation:**
```javascript
// Orchestrator aggregates findings by priority
const report = aggregateFindings(results)
// Sorts into: Blocker → High-Priority → Medium-Priority → Nitpicks
```

**Final Output:**
```markdown
# Design Review Summary

**Page:** User Dashboard (/dashboard)
**Status:** ⚠️ Needs Revisions (2 blockers found)

---

## Priority Findings

### [Blocker] Must Fix Before Merge

1. **Focus States Not Visible (Accessibility)**
   - Issue: Interactive elements have no visible focus indicator
   - WCAG: Violation of 2.4.7 Focus Visible (Level AA)
   - Screenshot: [evidence.png]
   - Fix: Add `:focus-visible` styles with 3:1 contrast ratio

2. **Horizontal Scrolling on Mobile (Responsive)**
   - Issue: Dashboard table causes horizontal scroll at 375px viewport
   - Impact: Unusable on mobile devices
   - Screenshot: [mobile-scroll.png]
   - Fix: Use responsive table pattern (cards on mobile)

### [High-Priority] Should Fix

3. **Color Contrast Below Minimum (Accessibility)**
   - Issue: Secondary text has 3.2:1 contrast (needs 4.5:1)
   - Location: Sidebar navigation labels
   - WCAG: Violation of 1.4.3 Contrast Minimum
   - Fix: Darken text color from #9CA3AF to #6B7280

4. **Inconsistent Spacing (Visual Polish)**
   - Issue: Card spacing varies (16px vs 20px vs 24px)
   - Impact: Visual inconsistency
   - Fix: Use 8px grid system consistently (16px or 24px only)

### [Medium-Priority] Nice to Have

5. **Component Duplication (Code Health)**
   - Issue: 3 similar card components (DashboardCard, StatCard, InfoCard)
   - Recommendation: Create unified Card component with variants
   - Impact: Easier maintenance

6. **No Empty State (Robustness)**
   - Issue: When user has no data, shows blank space
   - Recommendation: Add empty state illustration + CTA
   - Impact: Better UX

### [Nitpick] Polish

7. **Loading State Duration (Interaction)**
   - Observation: Spinner shows for <200ms (flash effect)
   - Recommendation: Delay spinner appearance until 300ms
   - Impact: Smoother perceived performance

---

## Specialist Summaries

✅ **Interaction Testing:** PASS (after blocker #1 fixed)
⚠️ **Responsive Design:** FAIL (blocker #2 must be fixed)
✅ **Visual Polish:** PASS (high-priority #4 recommended)
⚠️ **Accessibility:** FAIL (blockers #1, high-priority #3)
✅ **Robustness:** PASS (medium-priority #6 recommended)
✅ **Code Health:** PASS (medium-priority #5 recommended)

---

## Go/No-Go Decision

**Status:** 🚫 **NOT READY FOR MERGE**

**Reason:** 2 [Blocker] findings must be fixed:
1. Focus states (accessibility violation)
2. Horizontal scrolling (mobile unusable)

**Estimated Fix Time:** 2-3 hours

**Next Steps:**
1. Fix blocker #1: Add focus styles
2. Fix blocker #2: Implement responsive table
3. Re-run design review to verify fixes
4. Address high-priority findings before final merge
```

---

## Advanced Patterns

### Nested Orchestration

**Pattern:** Orchestrator launches another orchestrator as a subtask.

**Example:** TDD Frontend Orchestrator optionally launches Design Review Orchestrator

```javascript
// TDD Frontend Orchestrator
const implementation = new_task(
  mode: "nextjs-architect",
  message: "Implement component..."
)

// For UI-heavy features, launch Design Review Orchestrator
if (user.requiresDesignReview) {
  const designReview = new_task(
    mode: "orchestrator-design-review",  // ← Another orchestrator!
    message: "Run comprehensive design review..."
  )
  // Design Review Orchestrator launches 6 specialists
  // Results bubble back up to TDD Frontend Orchestrator
}
```

**Benefits:**
- Modular orchestration (compose workflows)
- Reusable sub-workflows
- Cleaner separation of concerns

**Limitations:**
- Deeper nesting = longer execution time
- Context passing across multiple levels
- Harder to debug

### Conditional Specialist Launching

**Pattern:** Orchestrator decides which specialists to launch based on context.

**Example:** TDD Backend Orchestrator conditionally launches refactor cycle

```javascript
const review = new_task(
  mode: "code-reviewer",
  message: "Review implementation..."
)

// Only launch refactor if issues found
if (review.blockers.length > 0 || review.highPriority.length > 0) {
  const refactored = new_task(
    mode: "laravel-architect",
    message: `Refactor to address: ${review.findings}`
  )
} else {
  // Skip refactor, move to validation
}
```

**Benefits:**
- Faster execution (skip unnecessary steps)
- Adaptive workflows
- Efficient resource use

### Iterative Refinement

**Pattern:** Launch specialist multiple times with refinements.

**Example:** Refactor until no high-priority issues remain

```javascript
let review = new_task(mode: "code-reviewer", message: "Review...")

while (review.highPriority.length > 0) {
  const refactored = new_task(
    mode: "nextjs-architect",
    message: `Refactor these issues: ${review.highPriority}`
  )

  // Re-review after refactoring
  review = new_task(
    mode: "code-reviewer",
    message: `Re-review refactored code: ${refactored}`
  )
}
```

**Benefits:**
- Quality gates enforced
- Iterative improvement
- Ensures standards met

**Limitations:**
- Can run indefinitely (set max iterations)
- Longer execution time
- Context can accumulate

### Error Handling and Recovery

**Pattern:** Orchestrator handles specialist failures gracefully.

**Example:** Fallback if specialist fails

```javascript
try {
  const result = new_task(
    mode: "nextjs-architect",
    message: "Implement complex feature..."
  )
} catch (error) {
  // Fallback: Launch simpler specialist
  const fallback = new_task(
    mode: "react-architect",
    message: "Implement simplified version..."
  )

  // Log warning in final report
  report.warnings.push("Fell back to simplified implementation")
}
```

**Benefits:**
- Resilient workflows
- Graceful degradation
- User still gets results

---

## Troubleshooting

### Issue: Orchestrator says "Mode not found"

**Cause:** Specialist mode not imported into Roo Code

**Solution:**
1. Check mode slug in orchestrator matches specialist YAML
2. Verify specialist YAML file is imported into Roo Code
3. Restart Roo Code / VS Code after import

**Example:**
```javascript
// Orchestrator tries to launch
new_task(mode: "nextjs-architect", ...)

// But specialist file is named "next-architect-export.yaml"
// with slug: "next-architect" (mismatch!)

// Fix: Update orchestrator to use correct slug OR rename specialist slug
```

### Issue: Specialist never returns results

**Cause:** Specialist didn't call `attempt_completion`

**Solution:**
- Always include instruction: "Use attempt_completion when done."
- Check specialist's tool access (can it perform requested task?)
- Review specialist's customInstructions for attempt_completion logic

### Issue: Context passed to specialist is truncated

**Cause:** Result string too large, hitting context limits

**Solution:**
```javascript
// ❌ DON'T: Pass huge result strings
new_task(message: `Full context: ${10000_line_result}`)

// ✅ DO: Pass summaries and references
new_task(message: `Summary: ${result.summary}
Key files: ${result.fileList}
Specialist can read files directly for details.`)
```

### Issue: Orchestrator tries to edit files and fails

**Cause:** Orchestrator has wrong tool access groups

**Solution:**
```yaml
# ❌ WRONG: Orchestrator with edit access
groups: [read, edit]

# ✅ CORRECT: Orchestrator with read-only
groups: [read]

# Then delegate file operations to specialists
```

### Issue: Parallel tasks run sequentially instead

**Cause:** Using sequential syntax instead of parallel

**Solution:**
```javascript
// ❌ WRONG: Sequential (waits for each)
const task1 = new_task(mode: "mode1", ...)
const task2 = new_task(mode: "mode2", ...)

// ✅ CORRECT: Parallel
const [task1, task2] = Promise.all([
  new_task(mode: "mode1", ...),
  new_task(mode: "mode2", ...)
])
```

### Issue: Specialist doesn't have necessary context

**Cause:** Orchestrator didn't pass context from previous tasks

**Solution:**
```javascript
// ❌ WRONG: Missing context
const tests = new_task(
  mode: "tdd-engineer",
  message: "Write tests"  // What tests? For what architecture?
)

// ✅ CORRECT: Explicit context passing
const tests = new_task(
  mode: "tdd-engineer",
  message: `Write tests for this architecture:

  ${architecture}

  Focus on: ${specificAreas}`
)
```

---

## Summary: Key Takeaways

### Orchestrator Design Principles

1. **Orchestrators coordinate, specialists execute**
   - Orchestrators: `groups: [read]` (no file edits)
   - Specialists: `groups: [read, edit, command]` (full access)

2. **Explicit is better than implicit**
   - Pass context explicitly between tasks
   - Always include "Use attempt_completion when done"
   - Specify expected output format

3. **Parallel when possible, sequential when necessary**
   - Independent tasks → Parallel execution
   - Dependent tasks → Sequential with context passing

4. **Quality gates at every phase**
   - Review results from each specialist
   - Conditional launching based on findings
   - Iterative refinement until standards met

### When Orchestration Excels

✅ Multi-step workflows (TDD, design review)
✅ Enforcing methodology (RED-GREEN-REFACTOR)
✅ Parallel specialist coordination
✅ Comprehensive reporting
✅ Quality gate enforcement

### When Manual Mode Switching is Better

✅ Single focused tasks
✅ Exploratory work
✅ Quick iterations
✅ Learning individual modes
✅ Simple one-off operations

---

## Quick Reference

### Orchestrator Template

```yaml
customModes:
  - slug: my-orchestrator
    name: 🎯 My Orchestrator
    roleDefinition: You coordinate X workflow by launching specialists...
    whenToUse: Use when X workflow needed...
    description: Orchestrates X
    groups: [read]  # Read-only for orchestrators
    customInstructions: >-
      ## Your Role

      Coordinate the X workflow by launching specialists in correct sequence.

      ## Workflow

      ### Phase 1: Setup
      ```javascript
      const setup = new_task(
        mode: "specialist-slug",
        message: "Task description. Use attempt_completion when done."
      )
      ```

      ### Phase 2: Execution
      ```javascript
      const result = new_task(
        mode: "another-specialist",
        message: `Execute based on: ${setup}. Use attempt_completion when done.`
      )
      ```

      ## Aggregation

      Synthesize results into comprehensive report.
```

### Specialist Template

```yaml
customModes:
  - slug: my-specialist
    name: 🔧 My Specialist
    roleDefinition: You are a specialist for X...
    whenToUse: Use when X task needed...
    description: Performs X
    groups: [read, edit, command]  # Full access for specialists
    customInstructions: >-
      ## Your Role

      Perform X task and return results via attempt_completion.

      ## Tasks

      1. Task A
      2. Task B
      3. Task C

      ## Output Format

      Return structured results:
      - Summary
      - Artifacts (files, code)
      - Any issues encountered

      Use attempt_completion when done.
```

---

**End of Guide**

For additional help:
- [Roo Code Documentation](https://docs.roocode.com)
- [Custom Modes Guide](https://docs.roocode.com/features/custom-modes)
- [Boomerang Tasks Guide](https://docs.roocode.com/features/boomerang-tasks)
- [Framework GitHub Issues](https://github.com/your-repo/issues)
