# Roo Code Custom Modes Collection

> **22 production-ready custom modes for [Roo Code](https://roocode.com)** - Transform your AI assistant into a team of specialized agents for full-stack development, test-driven development workflows, and comprehensive design review automation.

[![Modes](https://img.shields.io/badge/modes-22-blue.svg)](https://github.com/bhuntsman/roo-crew)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Roo Code](https://img.shields.io/badge/Roo_Code-Compatible-green.svg)](https://roocode.com)

---

## 🚀 Quick Start

### Import All Modes (Recommended)

1. **Download** [`roocode-modes-complete.yaml`](./roocode-modes-complete.yaml)
2. Open **Roo Code** → **Settings** → **Custom Modes**
3. Click **Import** → Select the downloaded file
4. **Done!** All 22 modes are now available

**⚡ 30 seconds to install. 22 specialized AI agents ready to use.**

### Import Individual Modes

Browse [`specialists/`](./specialists/) or [`orchestrators/`](./orchestrators/) directories and import only the modes you need.

📖 **New to Roo Code modes?** → Read the [**Quick Start Guide**](./QUICKSTART.md)

---

## 💡 What's Included

### 🔧 Specialists (12 Modes)

Individual expert modes for specific development tasks:

| Mode | Description | Key Features |
|------|-------------|--------------|
| [🐍 **Python FastAPI**](./specialists/python-fastapi.yaml) | Async REST APIs & microservices | FastAPI, Pydantic, async/await, OpenAPI |
| [🏗️ **Laravel Architect**](./specialists/laravel-architect.yaml) | PHP backend development | Laravel 11, Eloquent ORM, API resources |
| [▲ **Next.js Architect**](./specialists/nextjs-architect.yaml) | Next.js App Router apps | React Server Components, TypeScript strict |
| [⚛️ **React Architect**](./specialists/react-architect.yaml) | React SPA development | Hooks, Context, component patterns |
| [🗄️ **Database Engineer**](./specialists/database-engineer.yaml) | Database design & optimization | PostgreSQL, schema design, indexing, migrations |
| [⚡ **Cloudflare Workers**](./specialists/cloudflare-workers.yaml) | Edge compute & serverless | Workers, KV, D1, Durable Objects |
| [🧪 **TDD Engineer**](./specialists/tdd-engineer.yaml) | Test-driven development | Red-Green-Refactor, unit/integration tests |
| [🔍 **Code Reviewer**](./specialists/code-reviewer.yaml) | Code quality & security | Security audits, performance analysis, best practices |
| [🐙 **GitHub CLI Agent**](./specialists/github-cli.yaml) | Git workflow automation | PRs, issues, releases via `gh` CLI |
| [🔧 **PyATS Network Engineer**](./specialists/pyats-network-engineer.yaml) | Network automation | Cisco PyATS/Genie, testbed management, parsing |
| [🤖 **OpenRouter AI**](./specialists/openrouter-ai.yaml) | AI model selection | LLM routing, cost optimization |
| [📋 **PRD Generator**](./specialists/prd-generator.yaml) | Requirements documentation | Transform ideas into structured PRDs |

### 🎯 TDD Orchestrators (3 Modes)

Automate the Red-Green-Refactor cycle:

| Mode | Workflow | Launches |
|------|----------|----------|
| [🎯 **TDD Frontend**](./orchestrators/tdd/tdd-frontend.yaml) | Next.js/React TDD | Component → Test → Refactor → Review |
| [⚙️ **TDD Backend**](./orchestrators/tdd/tdd-backend.yaml) | Laravel/FastAPI TDD | API → Test → Refactor → Review |
| [🚀 **TDD DevOps**](./orchestrators/tdd/tdd-devops.yaml) | Infrastructure as Code TDD | IaC → Test → Deploy → Monitor |

### 🎨 Design Review Orchestrators (7 Modes)

Comprehensive UI/UX quality automation:

| Mode | Focus | Tests |
|------|-------|-------|
| [🎭 **Design Review Orchestrator**](./orchestrators/design-review/design-review-orchestrator.yaml) | Coordinates all reviewers | Launches 6 specialist reviewers in parallel |
| [♿ **Accessibility Auditor**](./orchestrators/design-review/accessibility-auditor.yaml) | WCAG 2.1 AA compliance | Keyboard nav, screen readers, ARIA |
| [📱 **Responsive Design Auditor**](./orchestrators/design-review/responsive-auditor.yaml) | Multi-device testing | Desktop (1920px), tablet (768px), mobile (375px) |
| [✨ **Visual Polish Reviewer**](./orchestrators/design-review/visual-reviewer.yaml) | Visual design QA | Spacing, alignment, typography, consistency |
| [🎮 **Interaction & Flow Tester**](./orchestrators/design-review/interaction-tester.yaml) | User interaction testing | Clicks, hovers, form flows, navigation |
| [🛡️ **Robustness Tester**](./orchestrators/design-review/robustness-tester.yaml) | Error handling | Edge cases, empty states, error boundaries |
| [🧹 **Code Health Reviewer**](./orchestrators/design-review/code-health-reviewer.yaml) | Component quality | Reusability, design tokens, TypeScript strict |

---

## 🎯 Use Cases

### Full-Stack Development
```bash
# Backend API with TDD
Use: TDD Backend Orchestrator + Python FastAPI + Database Engineer

# Frontend with design review
Use: Next.js Architect + TDD Frontend Orchestrator + Design Review Orchestrator
```

### Code Quality Automation
```bash
# Pre-merge quality gate
Use: Code Reviewer + Design Review Orchestrator + TDD Engineer

# Security-first review
Use: Code Reviewer (security focus) + Accessibility Auditor
```

### Network Automation
```bash
# PyATS test automation
Use: PyATS Network Engineer + GitHub CLI Agent + TDD Engineer

# Infrastructure testing
Use: TDD DevOps Orchestrator + PyATS Network Engineer
```

---

## 📚 Documentation

- **[Quick Start Guide](./QUICKSTART.md)** - Get up and running in 2 minutes
- **[Orchestration Guide](./docs/ORCHESTRATION_GUIDE.md)** - How orchestrators coordinate specialist modes
- **[Mode Reference](./docs/MODE_REFERENCE.md)** - Complete catalog of all 22 modes
- **[Development Guide](./docs/DEVELOPMENT.md)** - Create your own custom modes
- **[Workflow Examples](./examples/)** - Real-world usage patterns

---

## 🏗️ How It Works

### Specialists vs Orchestrators

**Specialists** are individual expert modes that perform specific tasks:
```yaml
# Example: Python FastAPI specialist
- Writes async REST API code
- Creates Pydantic models
- Implements error handling
- Has full read/write/command access
```

**Orchestrators** coordinate multiple specialists in automated workflows:
```yaml
# Example: TDD Backend Orchestrator
1. Launches Python FastAPI → writes failing test (RED)
2. Launches Python FastAPI → implements feature (GREEN)
3. Launches Code Reviewer → reviews & suggests refactoring (REFACTOR)
4. Returns aggregated results to user
```

Orchestrators use **Roo Code's Boomerang Tasks** to launch specialists, wait for results, and coordinate complex multi-step workflows automatically.

📖 **Learn more:** [Orchestration Guide](./docs/ORCHESTRATION_GUIDE.md)

---

## 🤝 Contributing

We welcome contributions! Here's how:

1. **Add a new mode** - Create a YAML file following our [Development Guide](./docs/DEVELOPMENT.md)
2. **Improve existing modes** - Submit PRs with enhancements
3. **Share workflows** - Add examples to [`examples/`](./examples/)
4. **Report bugs** - Use our [issue templates](./.github/ISSUE_TEMPLATE/)

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines.

---

## 📦 Installation Methods

### Method 1: All-in-One Import (Recommended)
```
Download: roocode-modes-complete.yaml
Import: All 22 modes at once
Time: 30 seconds
```

### Method 2: Category Import
```
Specialists only: Import all files from specialists/
Orchestrators only: Import all files from orchestrators/
Mix & match: Import specific modes you need
```

### Method 3: Individual Mode Import
```
Browse: specialists/ or orchestrators/ directories
Select: Individual .yaml files
Import: One mode at a time
```

---

## 🔧 Requirements

- **Roo Code** - [Download here](https://roocode.com)
- **Compatible with:** Roo Code v1.0+
- **No dependencies** - Modes are self-contained YAML files

---

## 📊 Stats

- **Total Modes:** 22
- **Specialists:** 12
- **Orchestrators:** 10
- **Supported Frameworks:** React, Next.js, Laravel, FastAPI, Cloudflare Workers
- **Languages:** TypeScript, Python, PHP, JavaScript
- **Testing:** Jest, Vitest, Playwright, PHPUnit, pytest
- **Special Features:** PyATS network automation, AI/LLM integration

---

## 🌟 Why Use These Modes?

✅ **Production-tested** - Used in real commercial projects
✅ **TDD-first** - Built around Red-Green-Refactor methodology
✅ **Framework-aware** - Deep knowledge of Next.js, Laravel, FastAPI patterns
✅ **Security-focused** - Code Reviewer checks for vulnerabilities
✅ **Accessibility-first** - WCAG 2.1 AA compliance built-in
✅ **DevOps-ready** - GitHub CLI integration, IaC testing
✅ **Network automation** - PyATS/Genie support for network engineers

---

## 📝 License

MIT License - see [LICENSE](./LICENSE) for details

---

## 🙏 Acknowledgments

Built on top of the [Claude Agent Framework](https://github.com/anthropics/claude-agent-framework) patterns.

Designed for use with [Roo Code](https://roocode.com) by [Roo Cline](https://github.com/RooVetGit/Roo-Code).

---

## 📞 Support

- **Documentation:** [docs/](./docs/)
- **Issues:** [GitHub Issues](https://github.com/bhuntsman/roo-crew/issues)
- **Discussions:** [GitHub Discussions](https://github.com/bhuntsman/roo-crew/discussions)

---

**Made with ❤️ by the Claude Agent Framework community**

⭐ **Star this repo** if you find these modes useful!
