# Contributing to Roo Code Custom Modes

Thank you for your interest in contributing! This document provides guidelines for contributing new modes, improvements, and documentation.

---

## 🎯 Ways to Contribute

1. **Add a new custom mode** - Create a new specialist or orchestrator
2. **Improve existing modes** - Enhance prompts, add capabilities, fix issues
3. **Write documentation** - Add examples, guides, or clarifications
4. **Report bugs** - Help us identify issues
5. **Share workflows** - Contribute real-world usage examples

---

## 🚀 Quick Start

### Prerequisites

- Familiarity with [Roo Code](https://roocode.com)
- Understanding of Roo Code custom modes (YAML format)
- Git and GitHub basics

### Setup

```bash
# Fork the repository on GitHub
# Clone your fork
git clone https://github.com/YOUR_USERNAME/roocode-modes.git
cd roocode-modes

# Create a branch for your changes
git checkout -b feature/my-new-mode
```

---

## 📝 Contributing a New Mode

### Step 1: Choose a Category

**Specialist Mode:**
- Single-purpose expert (e.g., Python FastAPI, Code Reviewer)
- Has full tool access (read, write, command)
- Performs specific development tasks
- Location: `specialists/`

**Orchestrator Mode:**
- Coordinates multiple specialists
- Uses Boomerang Tasks to launch other modes
- Has limited tool access (usually read-only)
- Location: `orchestrators/design-review/` or `orchestrators/tdd/`

### Step 2: Create the YAML File

#### Naming Convention

```
specialists/my-mode-name.yaml           # For specialists
orchestrators/category/my-orch-name.yaml  # For orchestrators
```

- Use lowercase with hyphens
- Be descriptive but concise
- No `-export` suffix (that's old convention)

#### File Structure

```yaml
customModes:
  - slug: my-mode-slug                  # Unique identifier (lowercase-with-hyphens)
    name: 🎯 My Mode Name                # Display name with emoji
    roleDefinition: >-
      You are a [role] specialist responsible for [primary tasks].
      You [key capabilities and responsibilities].

    whenToUse: >-
      Use this mode when [specific scenarios where this mode is appropriate].
      Examples: [list 3-5 concrete use cases].

    description: Brief one-line description

    groups:                             # Tool access permissions
      - read                            # Can read files
      - edit                            # Can write/edit files (specialists only)
      - command                         # Can run bash commands (if needed)
      - browser                         # Can use browser tools (for testing modes)
      - mcp                             # Can use MCP tools (for testing modes)

    customInstructions: >-
      ## Your Role
      [Detailed explanation of the role and responsibilities]

      ## Core Capabilities
      1. **Capability 1** - Description
      2. **Capability 2** - Description

      ## When to Use
      [Detailed scenarios]

      ## Workflow
      [Step-by-step process the mode should follow]

      ## Examples
      [Code examples, command patterns, etc.]

      ## Best Practices
      [Guidelines specific to this mode]

      ## Success Criteria
      ✅ [What "done" looks like]
      ✅ [Quality standards]

    source: project

    rulesFiles:
      - relativePath: AGENTS.md
        content: >-
          # Mode-Specific Rules

          - [Non-obvious rule 1]
          - [Non-obvious rule 2]
          - [Technical constraints]
          - [When NOT to use this mode]
```

### Step 3: Test Your Mode

1. **Import the mode** into Roo Code
   ```
   Settings → Custom Modes → Import → Select your .yaml file
   ```

2. **Test basic functionality**
   - Try 3-5 representative tasks
   - Verify it follows the instructions
   - Check tool access works correctly

3. **Test edge cases**
   - What happens with invalid input?
   - Does it handle errors gracefully?
   - Does it stay in scope?

4. **Test with other modes**
   - If orchestrator: Test launching specialists
   - If specialist: Test being launched by orchestrators

### Step 4: Add Documentation

Update relevant README files:

```markdown
# In specialists/README.md or orchestrators/*/README.md
| 🎯 **My Mode Name** | Brief description | Key features |
```

### Step 5: Add an Example

Create a usage example in `examples/`:

```markdown
# examples/my-mode-workflow.md

## Using My Mode Name

### Scenario
[Describe the use case]

### Steps
1. Select **🎯 My Mode Name** in Roo Code
2. Type: "..."
3. Expected output: ...

### Full Example
[Complete walkthrough with code snippets]
```

### Step 6: Update the Combined Import

Regenerate the combined import file:

```bash
cd /path/to/roocode-modes
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
```

### Step 7: Submit Pull Request

```bash
git add .
git commit -m "feat: add My Mode Name for [use case]"
git push origin feature/my-new-mode
```

Then create a Pull Request on GitHub with:
- **Title:** `feat: add [Mode Name] for [use case]`
- **Description:** What the mode does, why it's useful, how you tested it

---

## 🛠️ Contributing Improvements

### Bug Fixes

```bash
git checkout -b fix/issue-description
# Make your changes
git commit -m "fix: resolve issue with [specific problem]"
git push origin fix/issue-description
```

### Mode Enhancements

```bash
git checkout -b enhance/mode-name-improvement
# Improve the mode
git commit -m "enhance: improve [mode name] [what changed]"
git push origin enhance/mode-name-improvement
```

---

## 📖 Documentation Contributions

### Adding Examples

```bash
# Create a new example in examples/
examples/
  ├── my-workflow-example.md
  └── ...

# Follow this structure:
## Scenario
## Prerequisites
## Steps
## Expected Results
## Tips
```

### Improving Docs

- Fix typos, broken links
- Add clarifications
- Update outdated information
- Add diagrams or screenshots

---

## ✅ Quality Standards

### Code Quality

- **YAML Syntax:** Validate with `yamllint` or online validator
- **Emojis:** Use meaningful emojis in mode names (🎯 🔧 🎨 etc.)
- **Clarity:** Instructions should be clear and actionable
- **Scope:** Mode should have a well-defined, focused purpose

### Documentation

- **Examples:** Include at least one real-world example
- **Links:** Verify all links work
- **Formatting:** Use consistent markdown formatting
- **Completeness:** Explain what, why, and how

### Testing

- **Functionality:** Mode works as described
- **Integration:** Works with related modes
- **Edge Cases:** Handles errors and edge cases
- **Performance:** Responds in reasonable time

---

## 🎨 Style Guidelines

### Mode Names

- Use emoji that represents the mode's purpose
- Capitalize properly: "Python FastAPI" not "python fastapi"
- Be concise: 2-4 words maximum

### Slugs

- Lowercase with hyphens
- Descriptive: `python-fastapi` not `pf`
- No special characters except hyphens

### Instructions

- Use markdown formatting
- Include code examples where helpful
- Be specific: "Run `pytest tests/`" not "run tests"
- Use active voice: "You analyze code" not "Code is analyzed"

---

## 🐛 Reporting Issues

Use [GitHub Issues](https://github.com/bhuntsman/roo-crew/issues) to report:

- Bugs in modes
- Documentation errors
- Feature requests
- Questions about contribution

### Issue Template

```markdown
## Issue Type
- [ ] Bug in mode
- [ ] Documentation error
- [ ] Feature request

## Mode Affected
[Mode name]

## Description
[Clear description of the issue]

## Steps to Reproduce
1. Import mode
2. Run command: ...
3. See error: ...

## Expected Behavior
[What should happen]

## Actual Behavior
[What actually happens]

## Environment
- Roo Code version:
- OS:
```

---

## 🔄 Review Process

1. **Automated checks** - YAML validation (if CI is set up)
2. **Manual review** - Maintainer tests the mode
3. **Feedback** - Requested changes or approval
4. **Merge** - Once approved, merged to main
5. **Release** - Included in next version/release

---

## 📜 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

## 🙏 Recognition

Contributors will be:
- Listed in CHANGELOG.md for their contributions
- Acknowledged in release notes
- Added to the contributors list (if implemented)

---

## 💬 Questions?

- **GitHub Discussions:** Ask questions, share ideas
- **Issues:** Report bugs or request features
- **Documentation:** Check existing docs first

---

**Thank you for making Roo Code Custom Modes better!** 🎉
