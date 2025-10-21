# GitHub Release Checklist

Quick reference for publishing this repository to GitHub.

---

## ✅ Pre-Release Checklist (COMPLETED)

- [x] All YAML files renamed (removed `-export` suffix)
- [x] Combined import file renamed to `roocode-modes-complete.yaml`
- [x] Professional README.md created
- [x] QUICKSTART.md guide created
- [x] LICENSE file added (MIT)
- [x] CONTRIBUTING.md guidelines created
- [x] CHANGELOG.md with v1.0.0 created
- [x] .gitignore configured
- [x] docs/ directory structure created
- [x] examples/ directory with workflow example
- [x] .github/ templates created (issues, PR)
- [x] Git repository initialized
- [x] Initial commit created (39 files, 12,438 lines)

---

## 📝 Step 1: Create GitHub Repository

1. Go to [https://github.com/new](https://github.com/new)
2. **Repository name:** `roocode-modes` (recommended)
3. **Description:**
   ```
   22 production-ready Roo Code custom modes for full-stack development, TDD workflows, and design review automation
   ```
4. **Visibility:** Public
5. **DO NOT** initialize with README, .gitignore, or license (we already have these)
6. Click **Create repository**

---

## 🚀 Step 2: Push to GitHub

```bash
cd /Users/bhunt/development/claude/claude-saas-framework/roo-exports

# Add remote (replace USERNAME with your GitHub username)
git remote add origin git@github.com:USERNAME/roocode-modes.git

# Verify remote
git remote -v

# Push to GitHub
git branch -M main
git push -u origin main
```

---

## ⚙️ Step 3: Configure Repository Settings

### Topics/Tags
Add these topics in **Settings → Topics**:
- `roocode`
- `ai-agents`
- `tdd`
- `code-review`
- `automation`
- `development-tools`
- `claude-code`
- `roo-cline`

### Features
Enable in **Settings → Features**:
- [x] Issues
- [x] Discussions (optional but recommended)
- [x] Projects (optional)

### Branch Protection (Optional)
In **Settings → Branches → Add rule**:
- Branch name pattern: `main`
- [x] Require pull request reviews before merging
- [x] Require status checks to pass (if CI is added)

---

## 🏷️ Step 4: Create v1.0.0 Release

1. Go to **Releases** → **Draft a new release**
2. **Choose a tag:** `v1.0.0` (create on publish)
3. **Release title:** `Roo Code Modes v1.0.0 - Initial Public Release`
4. **Description:** Copy from CHANGELOG.md or use:

```markdown
## 🎉 Initial Public Release

First public release of the Roo Code Custom Modes collection.

### What's Included

**22 Production-Ready Modes:**
- 12 Specialist modes for focused development tasks
- 10 Orchestrator modes for automated workflows

**Specialists:**
- Python FastAPI, Laravel Architect, Next.js Architect
- React Architect, Database Engineer, Cloudflare Workers
- TDD Engineer, Code Reviewer, GitHub CLI Agent
- PyATS Network Engineer, OpenRouter AI, PRD Generator

**Orchestrators:**
- TDD: Frontend, Backend, DevOps (3 modes)
- Design Review: Complete UI/UX quality suite (7 modes)

### Quick Start

1. Download `roocode-modes-complete.yaml` from assets below
2. Open Roo Code → Settings → Custom Modes → Import
3. Select the downloaded file
4. All 22 modes are ready to use!

### Documentation

- [Quick Start Guide](./QUICKSTART.md)
- [Orchestration Guide](./docs/ORCHESTRATION_GUIDE.md)
- [Contributing Guidelines](./CONTRIBUTING.md)
- [TDD Workflow Example](./examples/tdd-backend-workflow.md)

### Features

✅ Production-tested modes used in real projects
✅ TDD-first methodology throughout
✅ Multi-framework support (React, Next.js, Laravel, FastAPI)
✅ WCAG 2.1 AA accessibility built-in
✅ Security-focused code review
✅ Network automation with PyATS/Genie

**Full changelog:** [CHANGELOG.md](./CHANGELOG.md)
```

5. **Attach binary:** Upload `roocode-modes-complete.yaml` as release asset
6. Click **Publish release**

---

## 📊 Step 5: Verify Release

After publishing, verify:

- [ ] Repository is public and accessible
- [ ] README.md displays correctly with badges
- [ ] All links in documentation work
- [ ] Release v1.0.0 is visible
- [ ] `roocode-modes-complete.yaml` is downloadable from release
- [ ] Issues tab is enabled
- [ ] Topics/tags are visible

---

## 🔧 Optional Enhancements

### Add Social Preview Image (Optional)
1. Create a 1280x640px image showcasing the modes
2. Upload in **Settings → Options → Social preview**

### Set Up GitHub Actions (Optional)
Create `.github/workflows/validate-yaml.yml`:

```yaml
name: Validate YAML

on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Validate YAML files
        run: |
          pip install yamllint
          yamllint -d relaxed *.yaml specialists/*.yaml orchestrators/*/*.yaml
```

### Add More Examples (Recommended)
Create additional workflow examples in `examples/`:
- `tdd-frontend-workflow.md`
- `design-review-workflow.md`
- `full-stack-project-setup.md`

### Create Additional Documentation (Recommended)
Add to `docs/`:
- `MODE_REFERENCE.md` - Complete catalog of all modes with detailed descriptions
- `DEVELOPMENT.md` - Guide for creating custom modes from scratch
- `WORKFLOWS.md` - Collection of common workflow patterns

---

## 📢 Promotion (Optional)

After release, consider sharing:

### Roo Code Community
- Share in Roo Code Discord/forums (if available)
- Post in relevant Reddit communities (r/RooCline, r/ClaudeAI, etc.)

### Social Media
- Tweet about the release with #RooCode #ClaudeCode #AIAgents
- Share on LinkedIn with professional development communities
- Post in dev.to or Hashnode

### Documentation Sites
- Add to awesome-roo-code lists (if they exist)
- Submit to AI tools directories

---

## 🐛 Post-Release Monitoring

After release, monitor:

- GitHub Issues for bug reports
- GitHub Discussions for questions
- Stars and forks as community interest metrics
- Pull requests from contributors

---

## 📋 Commands Quick Reference

```bash
# Clone for contributors
git clone git@github.com:USERNAME/roocode-modes.git

# Create feature branch
git checkout -b feature/my-new-mode

# Regenerate combined import after changes
python3 << 'EOF'
import yaml
from pathlib import Path
all_modes = []
for yaml_file in sorted(Path('.').rglob('*.yaml')):
    if yaml_file.name == 'roocode-modes-complete.yaml':
        continue
    with open(yaml_file, 'r') as f:
        data = yaml.safe_load(f)
        if data and 'customModes' in data:
            all_modes.extend(data['customModes'])
with open('roocode-modes-complete.yaml', 'w') as f:
    yaml.dump({'customModes': all_modes}, f, default_flow_style=False,
              sort_keys=False, allow_unicode=True, width=120)
print(f"✓ Combined {len(all_modes)} modes")
EOF

# Validate YAML syntax
yamllint -d relaxed *.yaml specialists/*.yaml orchestrators/*/*.yaml
```

---

## ✅ Release Complete!

Once all steps are done, your Roo Code Modes collection is live and ready for the community to use!

**Repository URL:** `https://github.com/USERNAME/roocode-modes`
**Release URL:** `https://github.com/USERNAME/roocode-modes/releases/tag/v1.0.0`

🎉 Congratulations on your public release!
