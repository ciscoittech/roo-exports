# Design Review Orchestration Modes (7)

These modes coordinate comprehensive design reviews by launching 6 specialist reviewers automatically using Roo Code's Boomerang Tasks.

## 🎨 Design Review Orchestrator

**File:** [orchestrator-design-review-export.yaml](orchestrator-design-review-export.yaml)

### What It Does
Coordinates 6 specialist design reviewers to perform comprehensive UI/UX validation.

### Workflow
Launches all specialists in **PARALLEL** (since they're independent):

1. **Interaction & Flow Tester** - Tests user flows and interactive states
2. **Responsive Design Auditor** - Tests 3 viewports (desktop, tablet, mobile)
3. **Visual Polish Reviewer** - Checks spacing, alignment, typography consistency
4. **Accessibility Auditor** - WCAG 2.1 AA compliance testing
5. **Robustness Tester** - Edge cases, error handling, empty states
6. **Code Health Reviewer** - Component reuse, design tokens, TypeScript quality

### Final Report Structure
```markdown
# Design Review Summary

**Status:** [Ready for Merge / Needs Revisions / Blocked]

## Priority Findings

### [Blocker] (Must Fix)
- Issue 1 with screenshot evidence
- Issue 2 with reproduction steps

### [High-Priority]
- Issue 3 with recommendations

### [Medium-Priority]
- Issue 4 (defer to backlog)

### [Nitpick]
- Minor polish suggestions

## Specialist Summaries
- Interaction Testing: PASS/FAIL
- Responsive Design: PASS/FAIL
- Visual Polish: PASS/FAIL
- Accessibility (WCAG AA): PASS/FAIL
- Robustness: PASS/FAIL
- Code Health: PASS/FAIL
```

### When to Use
- Final design validation before merge
- Comprehensive UI/UX quality assurance
- Accessibility compliance verification
- Need evidence (screenshots) for visual issues

---

## 🎮 Interaction & Flow Tester

**File:** [interaction-tester-export.yaml](interaction-tester-export.yaml)

### What It Tests
- User flows (happy path + error paths)
- Interactive states: hover, focus, active, disabled
- Loading states and transitions
- Form submission flows
- Navigation patterns

### Tools Used
- **Playwright MCP** for live browser testing
- Screenshot capture for visual evidence

### Blockers
- Focus states not visible (WCAG requirement)
- Broken user flows (can't complete core actions)
- Interactive elements not keyboard accessible

---

## 📱 Responsive Design Auditor

**File:** [responsive-auditor-export.yaml](responsive-auditor-export.yaml)

### What It Tests
Tests 3 standard viewports:
- **Desktop:** 1440px (large screens)
- **Tablet:** 768px (iPad)
- **Mobile:** 375px (iPhone)

### Checks
- No horizontal scrolling at ANY viewport
- Touch targets ≥44px (mobile requirement)
- Text remains readable (min 16px)
- Content reflows correctly
- Images scale appropriately
- Navigation collapses/expands properly

### Blockers
- Horizontal scrolling at any viewport
- Touch targets <44px on mobile
- Unreadable text (<16px)

---

## ✨ Visual Polish Reviewer

**File:** [visual-reviewer-export.yaml](visual-reviewer-export.yaml)

### What It Reviews
- Spacing consistency (8px grid system)
- Alignment issues (left/right/center)
- Typography consistency (font sizes, weights, line heights)
- Color usage (design system adherence)
- Visual hierarchy
- Border radius consistency
- Shadow consistency

### Methodology
- Uses Playwright to capture screenshots
- Pixel-perfect comparison against design
- Identifies "off by 2px" issues

### Blockers
- Critical visual bugs (overlapping content, broken layout)
- Completely broken typography (unreadable)

---

## ♿ Accessibility Auditor

**File:** [accessibility-auditor-export.yaml](accessibility-auditor-export.yaml)

### What It Tests
**WCAG 2.1 Level AA Compliance:**

1. **Keyboard Navigation**
   - All interactive elements reachable via Tab
   - Focus states visible (3:1 contrast ratio)
   - Logical tab order

2. **Color Contrast**
   - Normal text: 4.5:1 minimum
   - Large text (≥18px or ≥14px bold): 3:1 minimum
   - Interactive elements: 3:1 minimum

3. **Semantic HTML & ARIA**
   - Proper heading hierarchy (h1 → h2 → h3)
   - Form labels for all inputs
   - ARIA labels for custom components
   - Role attributes correct

4. **Screen Reader Testing**
   - Alt text for images
   - Announcements for dynamic content
   - Form error announcements

### Blockers
- Missing focus states
- Contrast ratios below minimums
- Form inputs without labels
- Keyboard trap issues

---

## 🛡️ Robustness Tester

**File:** [robustness-tester-export.yaml](robustness-tester-export.yaml)

### What It Tests
**Edge Cases:**
- Empty states (no data)
- Loading states (slow network)
- Error states (API failures)
- Large datasets (pagination, infinite scroll)
- Long strings (text overflow handling)
- Special characters (emoji, Unicode)
- Network offline scenarios

**Error Handling:**
- User-friendly error messages
- Recovery mechanisms (retry buttons)
- Error boundaries (no white screen crashes)
- Validation messages clear and actionable

### Blockers
- White screen crashes (no error boundary)
- No error messages (silent failures)
- Broken UI with empty data
- Text overflow breaking layout

---

## 🧹 Code Health Reviewer

**File:** [code-health-reviewer-export.yaml](code-health-reviewer-export.yaml)

### What It Reviews
1. **Component Reuse vs Duplication**
   - Similar components that should be unified
   - Prop variations vs separate components

2. **Design Token Usage**
   - No magic numbers in CSS (use variables)
   - Consistent spacing (design system)
   - Consistent colors (design tokens)

3. **TypeScript Quality**
   - No `any` types
   - Proper return types
   - Interface definitions

4. **CSS Organization**
   - No style duplication
   - Proper scoping (CSS modules / Tailwind)
   - No !important overrides

5. **Performance Patterns**
   - Memoization for expensive calculations
   - Lazy loading for heavy components
   - Image optimization

### Blockers
- `any` types in TypeScript (strict mode violation)
- Critical performance issues (blocking render)

---

## 📦 Import Instructions

### Import Complete Design Review System
```bash
# You MUST import all 7 files for the orchestrator to work
cat orchestrators/design-review/*.yaml > design-review-system.yaml

# Then import design-review-system.yaml into Roo Code
```

### Import Individual Specialists Only
```bash
# If you want to use specialists independently (no orchestrator)
# Import just the specific reviewers you need
```

**Recommended:** Import the complete system so the orchestrator can coordinate all specialists automatically.

---

## 🎯 Example Usage

### Complete Design Review
```
User: "Run a complete design review on the user dashboard"

1. Switch to: orchestrator-design-review mode
2. Provide context: "Review the user dashboard at /dashboard"
3. Orchestrator automatically:
   - Launches all 6 specialists in parallel
   - Collects results from each
   - Aggregates findings by priority
   - Provides comprehensive report with screenshots
   - Gives go/no-go recommendation
```

### Single Specialist Review
```
User: "Check if my form is accessible"

1. Switch to: accessibility-auditor mode
2. Provide context: "Review the form at /signup"
3. Specialist tests:
   - Keyboard navigation
   - Color contrast
   - ARIA labels
   - Screen reader compatibility
   - Returns WCAG 2.1 AA compliance report
```

---

## 🔧 Playwright MCP Configuration

Most design review specialists use **Playwright MCP** for live browser testing.

### Setup Required
```bash
# Install Playwright MCP server
npm install -g @modelcontextprotocol/server-playwright

# Configure in Roo Code settings
# MCP servers will be auto-detected
```

### What Playwright Enables
- Launch browser in different viewports
- Navigate to pages
- Click elements, fill forms
- Capture screenshots for evidence
- Test keyboard navigation
- Measure color contrast ratios
- Test responsive breakpoints

---

## 🆚 Orchestrator vs Manual Review

| Aspect | Design Review Orchestrator | Manual Specialist Usage |
|--------|---------------------------|------------------------|
| Specialists Run | All 6 in parallel | One at a time (manual switch) |
| Review Time | Faster (parallel) | Slower (sequential) |
| Report Format | Aggregated by priority | Individual reports |
| Go/No-Go Decision | Automatic recommendation | Manual synthesis |
| Screenshot Evidence | Consolidated | Scattered across reports |

**Recommendation:** Use orchestrator for final pre-merge review. Use individual specialists during active development for targeted feedback.

---

## 📊 Design Review Checklist

Before running the orchestrator, ensure:
- [ ] Feature is implemented and tests pass
- [ ] Local dev server is running (for Playwright testing)
- [ ] You can navigate to the pages/components to review
- [ ] Design mockups are available (for visual comparison)
- [ ] You're ready to act on findings (fix blockers before merge)

---

## 🎨 Integration with TDD Orchestrators

**Design Review is the final validation step in TDD Frontend Orchestrator:**

```
TDD Frontend Orchestrator Flow:
1. Architecture → Design component
2. Tests → Write failing tests (RED)
3. Implementation → Make tests pass (GREEN)
4. Code Review → Refactor (REFACTOR)
5. Design Review → Final UI/UX validation ← USES THIS ORCHESTRATOR
```

You can run design review independently OR let TDD Frontend Orchestrator trigger it automatically for UI-heavy features.

---

## 🔄 How It Works (Technical)

**Parallel Execution Pattern:**
```javascript
// Orchestrator launches all 6 specialists at once
const results = await Promise.all([
  new_task(mode: "interaction-tester", message: "Test user flows..."),
  new_task(mode: "responsive-auditor", message: "Test viewports..."),
  new_task(mode: "visual-reviewer", message: "Check visual polish..."),
  new_task(mode: "accessibility-auditor", message: "WCAG compliance..."),
  new_task(mode: "robustness-tester", message: "Test edge cases..."),
  new_task(mode: "code-health-reviewer", message: "Review code quality...")
])

// Aggregates results into priority-ordered report
const report = aggregateFindings(results)
```

**Advantages:**
- 6 specialists complete in time of 1 (parallel execution)
- Consistent reporting format across all specialists
- Automatic prioritization (Blocker → High → Medium → Nitpick)
- Go/no-go recommendation based on blocker count

---

## 🏆 Quality Gates

**Ready for Merge Criteria:**
- ✅ Zero [Blocker] findings
- ✅ Zero [High-Priority] findings (or all accepted as technical debt)
- ✅ WCAG 2.1 AA compliance achieved
- ✅ All 3 viewports functional (no horizontal scroll)
- ✅ User flows complete successfully

**Needs Revisions:**
- ⚠️ [High-Priority] findings require fixes
- ⚠️ WCAG violations need resolution

**Blocked:**
- 🚫 [Blocker] findings MUST be fixed before merge
- 🚫 Broken user flows (cannot complete core actions)
- 🚫 Critical visual bugs (overlapping/broken layouts)
