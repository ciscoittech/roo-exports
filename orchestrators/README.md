# Orchestration Modes

Orchestrators coordinate multiple specialist modes automatically using Roo Code's **Boomerang Tasks** pattern.

## 📂 Folder Structure

```
orchestrators/
├── tdd/                    # TDD workflow orchestration (3 modes)
│   ├── tdd-frontend-orchestrator-export.yaml
│   ├── tdd-backend-orchestrator-export.yaml
│   └── tdd-devops-orchestrator-export.yaml
│
└── design-review/          # Design review orchestration (7 modes)
    ├── orchestrator-design-review-export.yaml  (coordinator)
    ├── interaction-tester-export.yaml
    ├── responsive-auditor-export.yaml
    ├── visual-reviewer-export.yaml
    ├── accessibility-auditor-export.yaml
    ├── robustness-tester-export.yaml
    └── code-health-reviewer-export.yaml
```

## 🎯 TDD Orchestration (3 modes)

**Purpose:** Automate the Red-Green-Refactor cycle for frontend, backend, and infrastructure development.

### [TDD Frontend Orchestrator](tdd/tdd-frontend-orchestrator-export.yaml)
- Coordinates Next.js/React development with TDD
- Launches: Architecture → Tests (RED) → Implementation (GREEN) → Review (REFACTOR)
- Optional: Design Review validation

### [TDD Backend Orchestrator](tdd/tdd-backend-orchestrator-export.yaml)
- Coordinates Laravel/FastAPI development with TDD
- Launches: Architecture + Database → Tests (RED) → Implementation (GREEN) → Security Audit (REFACTOR)
- Target: ≥80% test coverage

### [TDD DevOps Orchestrator](tdd/tdd-devops-orchestrator-export.yaml)
- Coordinates Infrastructure as Code with TDD
- Launches: Infrastructure Design → Tests (RED) → IaC Implementation (GREEN) → Security Audit → Deployment Testing
- Generates runbooks automatically

**[📖 Full TDD Documentation](tdd/README.md)**

---

## 🎨 Design Review Orchestration (7 modes)

**Purpose:** Comprehensive UI/UX validation before merge.

### [Design Review Orchestrator](design-review/orchestrator-design-review-export.yaml)
Coordinates 6 specialist reviewers in parallel:

1. **Interaction & Flow Tester** - User flows, interactive states (Playwright)
2. **Responsive Design Auditor** - 3 viewports (desktop, tablet, mobile)
3. **Visual Polish Reviewer** - Spacing, alignment, typography
4. **Accessibility Auditor** - WCAG 2.1 AA compliance
5. **Robustness Tester** - Edge cases, error handling
6. **Code Health Reviewer** - Component reuse, design tokens

Aggregates findings into priority-ordered report (Blocker → High → Medium → Nitpick).

**[📖 Full Design Review Documentation](design-review/README.md)**

---

## 🔄 How Orchestration Works

### Boomerang Tasks Pattern

Orchestrators use Roo Code's `new_task` tool to launch specialist modes:

```javascript
// Orchestrator launches a specialist
const result = new_task(
  mode: "nextjs-architect",
  message: "Design component architecture for user profile form. Use attempt_completion when done."
)

// Specialist completes work and returns via attempt_completion
// Orchestrator receives result and launches next specialist

const tddResult = new_task(
  mode: "tdd-engineer",
  message: `Write failing tests based on architecture: ${result}. Use attempt_completion when done.`
)

// Orchestrator aggregates all results into final report
```

### Key Advantages

**Automated Coordination:**
- No manual mode switching required
- Specialists launched in correct sequence
- Results automatically passed between specialists

**Parallel Execution:**
- Independent tasks run simultaneously
- 6 design reviewers complete in time of 1
- Architecture + Database design run in parallel

**Comprehensive Reporting:**
- All results aggregated into single report
- Priority-ordered findings (Blocker → High → Medium)
- Go/no-go recommendations based on findings

---

## 📦 Import Instructions

### Import Complete Orchestration System

**All Orchestrators + All Required Specialists:**
```bash
cd /Users/bhunt/development/claude/claude-saas-framework/roo-exports

# Complete system (orchestrators + specialists)
cat specialists/*.yaml \
    orchestrators/tdd/*.yaml \
    orchestrators/design-review/*.yaml \
    > complete-orchestration-system.yaml

# Import complete-orchestration-system.yaml into Roo Code
```

### Import by Category

**TDD Orchestration Only:**
```bash
# TDD orchestrators + specialists they need
cat specialists/nextjs-architect-export.yaml \
    specialists/react-architect-export.yaml \
    specialists/laravel-architect-export.yaml \
    specialists/python-fastapi-export.yaml \
    specialists/database-engineer-export.yaml \
    specialists/cloudflare-workers-export.yaml \
    specialists/tdd-engineer-export.yaml \
    specialists/code-reviewer-export.yaml \
    orchestrators/design-review/code-health-reviewer-export.yaml \
    orchestrators/tdd/*.yaml \
    > tdd-system.yaml
```

**Design Review Only:**
```bash
# Design review orchestrator + all specialists
cat orchestrators/design-review/*.yaml > design-review-system.yaml
```

---

## 🎯 When to Use Orchestrators vs Specialists

### Use Orchestrators When:
- ✅ Multi-step workflow (RED-GREEN-REFACTOR, comprehensive review)
- ✅ Need consistent process (TDD methodology)
- ✅ Want parallel execution (design review specialists)
- ✅ Need aggregated reporting (priority-ordered findings)
- ✅ Complex coordination required (architecture → tests → implementation)

### Use Specialists Directly When:
- ✅ Single focused task (write tests, review code)
- ✅ Quick iteration during development
- ✅ Targeted feedback needed
- ✅ Exploring options (trying different architectures)

---

## 🔗 Integration Between Orchestrators

**TDD Frontend Orchestrator can launch Design Review Orchestrator:**

```
TDD Frontend Flow:
1. Architecture → Design components
2. Tests → Write failing tests (RED)
3. Implementation → Make tests pass (GREEN)
4. Code Review → Refactor code (REFACTOR)
5. Design Review → UI/UX validation (launches Design Review Orchestrator)
   └── Launches 6 design specialists in parallel
6. Final report with test coverage + design validation
```

This creates a **nested orchestration pattern** where one orchestrator can delegate to another for specialized workflows.

---

## 📊 Orchestrator Comparison

| Feature | TDD Orchestrators | Design Review Orchestrator |
|---------|------------------|---------------------------|
| **Purpose** | Automate Red-Green-Refactor | Automate UI/UX validation |
| **Execution** | Sequential (with some parallel) | Fully parallel (6 specialists) |
| **Duration** | Longer (full implementation) | Shorter (review only) |
| **Specialists** | 3-4 specialists | 6 specialists |
| **Output** | Implementation + tests + review | Priority-ordered findings |
| **Blockers** | Security issues, broken tests | Visual bugs, WCAG violations |
| **When** | During feature development | Before merge (final validation) |

---

## 🏆 Quality Standards

### TDD Orchestrators Enforce:
- Tests written BEFORE implementation (RED first)
- All tests passing (GREEN achieved)
- Code review completed with no blockers (REFACTOR)
- ≥80% test coverage (backend)
- TypeScript strict mode (frontend)
- Security validation (all APIs)

### Design Review Orchestrator Enforces:
- WCAG 2.1 AA compliance
- 3 viewports functional (no horizontal scroll)
- All interactive states tested (hover, focus, active, disabled)
- Visual consistency (design tokens, spacing)
- Edge cases handled (empty, loading, error states)
- Zero [Blocker] findings before merge

---

## 🛠️ Technical Requirements

**For TDD Orchestrators:**
- Roo Code with Boomerang Tasks support
- All specialist modes imported
- Project must have testing framework setup

**For Design Review Orchestrator:**
- Roo Code with Boomerang Tasks support
- Playwright MCP server installed and configured
- Local dev server running (for live browser testing)
- All 7 design review modes imported

---

## 🚀 Getting Started

1. **Choose Your Workflow:**
   - Building features? → TDD Orchestrators
   - Final review? → Design Review Orchestrator

2. **Import Required Modes:**
   - Use import scripts above to get orchestrators + specialists

3. **Start Using:**
   - Switch to orchestrator mode in Roo Code
   - Provide your requirement/context
   - Let orchestrator coordinate specialists automatically

4. **Review Results:**
   - Orchestrator provides comprehensive final report
   - Aggregated findings with priorities
   - Action items clearly identified

---

**Pro Tip:** Combine TDD Frontend Orchestrator with Design Review Orchestrator for complete feature development (implementation + validation) fully automated.
