# TDD Orchestration Modes (3)

These orchestrators coordinate the **Red-Green-Refactor** cycle by launching multiple specialist modes automatically using Roo Code's Boomerang Tasks.

## 🎯 TDD Frontend Orchestrator

**File:** [tdd-frontend-orchestrator-export.yaml](tdd-frontend-orchestrator-export.yaml)

### What It Does
Orchestrates test-driven development for Next.js and React applications.

### Workflow
1. **RED Phase (Architecture + Tests)**
   - Launches Next.js/React Architect to design component architecture
   - Launches TDD Engineer to write failing tests FIRST

2. **GREEN Phase (Implementation)**
   - Launches implementation specialist to make tests pass (minimum code)

3. **REFACTOR Phase (Quality)**
   - Launches Code Health Reviewer to identify refactoring opportunities
   - Re-launches implementation if blockers/high-priority issues found

4. **VALIDATE Phase (Optional)**
   - Launches Design Review Orchestrator for UI-heavy features

### When to Use
- Building new frontend features with TDD methodology
- Ensuring TypeScript strict mode compliance (no `any` types)
- Need comprehensive design review (interaction, responsive, accessibility)

### Specialists Launched
- Next.js Architect OR React Architect
- TDD Engineer
- Code Health Reviewer
- Design Review Orchestrator (optional)

---

## ⚙️ TDD Backend Orchestrator

**File:** [tdd-backend-orchestrator-export.yaml](tdd-backend-orchestrator-export.yaml)

### What It Does
Orchestrates test-driven development for Laravel and FastAPI backend applications.

### Workflow
1. **FOUNDATION Phase (Architecture + Database)**
   - Launches Backend Architect + Database Engineer in PARALLEL

2. **RED Phase (Tests)**
   - Launches TDD Engineer to write tests for API + database + business logic

3. **GREEN Phase (Implementation)**
   - Launches implementation to make all tests pass
   - Runs migrations, implements models, endpoints, services

4. **REFACTOR Phase (Quality + Security)**
   - Launches Code Reviewer for quality + security audit
   - Re-launches implementation if blockers/high-priority issues found

5. **VALIDATE Phase**
   - Validates API contract compliance (OpenAPI/Swagger)
   - Validates database performance (no N+1 queries)

### When to Use
- Building new API endpoints with TDD methodology
- Database schema design with migration testing
- Need security validation (IAM, encryption, input validation)

### Specialists Launched
- Laravel Architect OR Python FastAPI
- Database Engineer
- TDD Engineer
- Code Reviewer

### Key Metrics
- **Test Coverage Target:** ≥80%
- **Security:** All endpoints must have validation + authorization
- **Performance:** No N+1 queries allowed

---

## 🚀 TDD DevOps Orchestrator

**File:** [tdd-devops-orchestrator-export.yaml](tdd-devops-orchestrator-export.yaml)

### What It Does
Orchestrates Infrastructure as Code development with test-driven methodology.

### Workflow
1. **FOUNDATION Phase (Design + Security)**
   - Launches Infrastructure Architect + Security Analysis in PARALLEL

2. **RED Phase (Infrastructure Tests)**
   - Launches TDD Engineer to write infrastructure tests (Terraform plan tests, security compliance)

3. **GREEN Phase (IaC Implementation)**
   - Implements IaC code to make tests pass (Terraform/Pulumi/Wrangler)

4. **SECURITY Phase (Compliance)**
   - Launches Security Audit for compliance validation (IAM, encryption, SOC2/GDPR)

5. **DEPLOYMENT Phase (Testing)**
   - Tests deployment pipeline (staging deploy, rollback, health checks)

6. **DOCUMENTATION Phase**
   - Generates runbooks + architecture diagrams

### When to Use
- Infrastructure as Code with Terraform, Pulumi, or Cloudflare Workers
- CI/CD pipeline setup with testing and rollback mechanisms
- Need security compliance validation (SOC2, GDPR)

### Specialists Launched
- Cloudflare Workers (infrastructure architect)
- Code Reviewer (security audit)
- TDD Engineer (infrastructure testing + deployment validation)

### Key Requirements
- **Security:** Hardcoded secrets = BLOCKER
- **Rollback:** Must be tested and <5 minutes
- **Monitoring:** Logs + metrics + alerts are MANDATORY
- **Documentation:** Auto-generated from IaC code

---

## 📦 Import Instructions

### Import Individual Orchestrators
```bash
# Import just one orchestrator
# In Roo Code → Custom Modes → Import YAML → Select file
```

### Import All TDD Orchestrators
```bash
# Combine all TDD orchestrators into one file
cat orchestrators/tdd/*.yaml > tdd-orchestrators.yaml

# Then import tdd-orchestrators.yaml into Roo Code
```

### Import Complete System (Orchestrators + Specialists)
```bash
# If you want orchestrators to work, you MUST also import the specialists they launch
cd /Users/bhunt/development/claude/claude-saas-framework/roo-exports

# Option 1: Import everything
cat specialists/*.yaml orchestrators/tdd/*.yaml > complete-tdd-system.yaml

# Option 2: Frontend only
cat specialists/nextjs-architect-export.yaml \
    specialists/react-architect-export.yaml \
    specialists/tdd-engineer-export.yaml \
    orchestrators/design-review/code-health-reviewer-export.yaml \
    orchestrators/tdd/tdd-frontend-orchestrator-export.yaml \
    > frontend-tdd-system.yaml

# Option 3: Backend only
cat specialists/laravel-architect-export.yaml \
    specialists/python-fastapi-export.yaml \
    specialists/database-engineer-export.yaml \
    specialists/tdd-engineer-export.yaml \
    specialists/code-reviewer-export.yaml \
    orchestrators/tdd/tdd-backend-orchestrator-export.yaml \
    > backend-tdd-system.yaml

# Option 4: DevOps only
cat specialists/cloudflare-workers-export.yaml \
    specialists/code-reviewer-export.yaml \
    specialists/tdd-engineer-export.yaml \
    orchestrators/tdd/tdd-devops-orchestrator-export.yaml \
    > devops-tdd-system.yaml
```

---

## 🎯 Example Usage

### Frontend Feature with TDD
```
User: "I need to build a user profile form with validation using TDD"

1. Switch to: tdd-frontend-orchestrator mode
2. Provide requirement
3. Orchestrator automatically:
   - Designs component architecture (Next.js Architect)
   - Writes failing tests (TDD Engineer)
   - Implements to pass tests
   - Reviews and refactors (Code Health Reviewer)
   - Provides comprehensive summary
```

### Backend API with TDD
```
User: "I need to build a REST API for blog posts with TDD"

1. Switch to: tdd-backend-orchestrator mode
2. Provide requirement
3. Orchestrator automatically:
   - Designs API + database schema (Laravel Architect + Database Engineer)
   - Writes failing tests (TDD Engineer)
   - Implements API + migrations
   - Security audit (Code Reviewer)
   - Validates API contract
   - Provides test coverage report (≥80%)
```

### Infrastructure with TDD
```
User: "I need to deploy Cloudflare Workers with KV storage using TDD"

1. Switch to: tdd-devops-orchestrator mode
2. Provide requirement
3. Orchestrator automatically:
   - Designs infrastructure (Cloudflare Workers specialist)
   - Writes infrastructure tests (TDD Engineer)
   - Implements IaC code (wrangler.toml, Workers code)
   - Security audit (Code Reviewer)
   - Tests deployment + rollback
   - Generates runbook
```

---

## 🔄 How Orchestration Works

**Boomerang Tasks Pattern:**
```javascript
// Orchestrator launches specialists using new_task
const architectTask = new_task(
  mode: "nextjs-architect",
  message: "Design component architecture for user profile form..."
)

// Specialist completes and returns results via attempt_completion
// Orchestrator receives results and launches next specialist
const tddTask = new_task(
  mode: "tdd-engineer",
  message: `Write tests based on architecture: ${architectTask.result}...`
)

// Orchestrator waits for all tasks, then aggregates results
```

**Key Advantages:**
- **Automated:** No manual mode switching
- **Parallel:** Independent tasks run simultaneously
- **Sequential:** Dependent tasks wait for prerequisites
- **Comprehensive:** All results aggregated into final report

---

## 🆚 Orchestrators vs Manual Workflows

| Aspect | Orchestrator Mode | Manual Mode Switching |
|--------|-------------------|----------------------|
| Mode Switching | Automatic | Manual |
| Parallel Execution | Yes (where possible) | No |
| Result Aggregation | Automatic | Manual |
| Workflow Consistency | Enforced (RED→GREEN→REFACTOR) | Developer's discretion |
| Time to Complete | Faster (parallel tasks) | Slower (sequential) |
| Context Retention | Automatic (passed between tasks) | Manual (copy/paste) |

**Recommendation:** Use orchestrators for complex multi-step workflows. Use manual mode switching for simple single-specialist tasks.
