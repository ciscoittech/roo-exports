# Core Specialist Modes (11)

These are individual specialist modes that can be used independently or launched by orchestrators.

## 🏗️ Backend Specialists

### [laravel-architect-export.yaml](laravel-architect-export.yaml)
- **Purpose:** Laravel API design & implementation
- **Tool Access:** Full (read + edit + browser + command + mcp)
- **Use When:** Building Laravel APIs, authentication, migrations, Eloquent models

### [python-fastapi-export.yaml](python-fastapi-export.yaml)
- **Purpose:** Async microservices & AI integration
- **Tool Access:** Full (read + edit + browser + command + mcp)
- **Use When:** Long-running tasks (>30s), ML inference, async processing

### [database-engineer-export.yaml](database-engineer-export.yaml)
- **Purpose:** PostgreSQL schema design & optimization
- **Tool Access:** Read + Edit + Command
- **Use When:** Schema design, query optimization, migration management

---

## ⚛️ Frontend Specialists

### [react-architect-export.yaml](react-architect-export.yaml)
- **Purpose:** React SPA design & implementation
- **Tool Access:** Full (read + edit + browser + command + mcp)
- **Use When:** Building React components, forms, state management

### [nextjs-architect-export.yaml](nextjs-architect-export.yaml)
- **Purpose:** Next.js App Router with strict TypeScript
- **Tool Access:** Full (read + edit + browser + command + mcp)
- **Use When:** Building Next.js apps with Server Components, Server Actions
- **Special:** NO `any` types allowed (strict TypeScript enforcement)

---

## 🛠️ Infrastructure & DevOps

### [cloudflare-workers-export.yaml](cloudflare-workers-export.yaml)
- **Purpose:** Edge routing & Workers deployment
- **Tool Access:** Read + Edit + Command
- **Use When:** Edge routing decisions, KV/R2/Queues setup

---

## 🧪 Quality & Testing

### [tdd-engineer-export.yaml](tdd-engineer-export.yaml)
- **Purpose:** Test-driven development & QA
- **Tool Access:** Read + Edit + Command
- **Use When:** Writing tests, TDD Red-Green-Refactor cycle
- **Coverage Target:** ≥80%

### [code-reviewer-export.yaml](code-reviewer-export.yaml)
- **Purpose:** Code quality, security & best practices
- **Tool Access:** Read + Edit only (no system changes)
- **Use When:** Reviewing code quality, security vulnerabilities

---

## 🤖 AI & Tooling

### [openrouter-ai-export.yaml](openrouter-ai-export.yaml)
- **Purpose:** AI model selection & cost optimization
- **Tool Access:** Read only (analysis mode)
- **Use When:** Selecting AI models, estimating costs

### [github-cli-export.yaml](github-cli-export.yaml)
- **Purpose:** Git & GitHub workflow automation
- **Tool Access:** Read + Edit + Command
- **Use When:** Branch management, PRs, semantic commits

### [prd-generator-export.yaml](prd-generator-export.yaml)
- **Purpose:** Transform ideas into executable PRDs
- **Tool Access:** Read + Edit
- **Use When:** Starting new projects, defining requirements

---

## 📦 Import Instructions

### Import Individual Modes
1. Open Roo Code in VS Code
2. Go to Custom Modes settings
3. Import the specific YAML file you need

### Import All Specialists at Once
```bash
# Combine all specialists into one file
cat specialists/*.yaml > all-specialists.yaml

# Then import all-specialists.yaml into Roo Code
```

### Recommended Starter Pack
**For Full-Stack Next.js Projects:**
- `nextjs-architect-export.yaml`
- `laravel-architect-export.yaml`
- `database-engineer-export.yaml`
- `tdd-engineer-export.yaml`
- `code-reviewer-export.yaml`

**For React SPA Projects:**
- `react-architect-export.yaml`
- `laravel-architect-export.yaml`
- `database-engineer-export.yaml`
- `tdd-engineer-export.yaml`
- `code-reviewer-export.yaml`

---

## 🔄 Used By Orchestrators

These specialists are automatically launched by:
- **TDD Frontend Orchestrator** → Next.js/React Architect + TDD Engineer + Code Reviewer
- **TDD Backend Orchestrator** → Laravel/FastAPI + Database Engineer + TDD Engineer + Code Reviewer
- **TDD DevOps Orchestrator** → Cloudflare Workers + Code Reviewer + TDD Engineer
- **Design Review Orchestrator** → Code Health Reviewer (the only specialist shared with design review)

You can use them independently OR let orchestrators coordinate them automatically.
