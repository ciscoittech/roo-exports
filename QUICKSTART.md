# Quick Start Guide

Get up and running with Roo Code Custom Modes in **2 minutes**.

---

## Step 1: Install Roo Code

If you haven't already, [download Roo Code](https://roocode.com) and install it.

---

## Step 2: Import Modes

### Option A: Import All 22 Modes (Recommended)

1. Download [`roocode-modes-complete.yaml`](./roocode-modes-complete.yaml)
2. Open **Roo Code**
3. Click **Settings** (gear icon)
4. Go to **Custom Modes** tab
5. Click **Import** button
6. Select the downloaded `roocode-modes-complete.yaml` file
7. **Done!** All 22 modes are now available

### Option B: Import Individual Modes

1. Browse the [`specialists/`](./specialists/) or [`orchestrators/`](./orchestrators/) directories
2. Download specific `.yaml` files you need
3. Follow steps 2-6 above for each file

---

## Step 3: Use Your First Mode

### Example 1: Build a FastAPI Endpoint with TDD

1. In Roo Code, click the **mode selector** (top left)
2. Choose **⚙️ TDD Backend Orchestrator**
3. Type: *"Create a FastAPI endpoint for user registration with tests"*
4. The orchestrator will:
   - Launch **Python FastAPI** mode to write a failing test (RED)
   - Launch **Python FastAPI** mode again to implement the endpoint (GREEN)
   - Launch **Code Reviewer** to suggest refactoring (REFACTOR)
   - Return the complete, tested implementation

### Example 2: Review Your React Component

1. Select **🎭 Design Review Orchestrator** mode
2. Type: *"Review my LoginForm component"*
3. The orchestrator will launch 6 specialist reviewers in parallel:
   - ♿ Accessibility Auditor (WCAG 2.1 AA)
   - 📱 Responsive Design Auditor (mobile/tablet/desktop)
   - ✨ Visual Polish Reviewer (spacing, alignment)
   - 🎮 Interaction & Flow Tester (user flows)
   - 🛡️ Robustness Tester (edge cases)
   - 🧹 Code Health Reviewer (code quality)
4. Get a comprehensive review report in minutes

### Example 3: Use a Specialist Directly

1. Select **🐍 Python FastAPI** mode
2. Type: *"Create a Pydantic model for a User with email validation"*
3. Get FastAPI-specific code immediately (no orchestration)

---

## Understanding Modes

### 🔧 Specialists (12 modes)
Individual expert modes for specific tasks:
- Use when you need a single focused capability
- Have full read/write/command access
- Examples: Python FastAPI, Next.js Architect, Code Reviewer

### 🎯 Orchestrators (10 modes)
Coordinate multiple specialists in workflows:
- Use for complex multi-step processes
- Launch specialists using Boomerang Tasks
- Examples: TDD Backend Orchestrator, Design Review Orchestrator

📖 **Learn more:** [Orchestration Guide](./docs/ORCHESTRATION_GUIDE.md)

---

## Common Workflows

### Full-Stack Development
```
1. Use: Next.js Architect (build frontend)
2. Use: Python FastAPI (build backend API)
3. Use: Database Engineer (design schema)
4. Use: Design Review Orchestrator (QA frontend)
5. Use: Code Reviewer (final security/quality check)
```

### Test-Driven Development
```
1. Use: TDD Frontend Orchestrator (for React/Next.js)
   OR: TDD Backend Orchestrator (for Laravel/FastAPI)
2. Orchestrator handles Red-Green-Refactor automatically
3. Get tested, reviewed code in one workflow
```

### Code Review & Quality
```
1. Use: Code Reviewer (security, performance, best practices)
2. Use: Design Review Orchestrator (UI/UX quality)
3. Get comprehensive feedback before merging
```

---

## Tips & Tricks

### Switch Modes Mid-Conversation
You can change modes at any time:
1. Click the mode selector
2. Choose a different mode
3. Continue the conversation with new context

### Combine Modes in Sequence
```
Step 1: Use Python FastAPI → build API
Step 2: Switch to Code Reviewer → review the code
Step 3: Switch to GitHub CLI Agent → create PR
```

### Let Orchestrators Do the Work
Instead of manually switching between modes:
```
❌ Manual: FastAPI → TDD Engineer → Code Reviewer → GitHub CLI
✅ Automated: TDD Backend Orchestrator (does all the above)
```

---

## Need Help?

- **Full Documentation:** [README.md](./README.md)
- **Mode Reference:** [docs/MODE_REFERENCE.md](./docs/MODE_REFERENCE.md)
- **Orchestration Deep Dive:** [docs/ORCHESTRATION_GUIDE.md](./docs/ORCHESTRATION_GUIDE.md)
- **Examples:** [examples/](./examples/)
- **Issues:** [GitHub Issues](https://github.com/yourusername/roocode-modes/issues)

---

## What's Next?

1. ✅ Import modes (you just did this!)
2. 📖 Read the [Orchestration Guide](./docs/ORCHESTRATION_GUIDE.md)
3. 🎯 Try the [TDD workflow examples](./examples/)
4. 🛠️ Create your own custom mode using the [Development Guide](./docs/DEVELOPMENT.md)

---

**Happy coding! 🚀**
