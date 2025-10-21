# Pull Request

## Type of Change
- [ ] New mode (specialist or orchestrator)
- [ ] Mode enhancement (improve existing mode)
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] Documentation update
- [ ] Example/workflow addition

## Description
A clear and concise description of what this PR does.

## Related Issue
Fixes #[issue number]
Related to #[issue number]

## Changes Made
List the specific changes in this PR:

- [ ] Added/modified YAML file: `[path/to/file.yaml]`
- [ ] Updated documentation: `[file names]`
- [ ] Added examples: `[file names]`
- [ ] Updated combined import file: `roocode-modes-complete.yaml`
- [ ] Other: [describe]

## For New Modes

### Mode Details
- **Name:** [Mode Name with Emoji]
- **Slug:** `mode-slug`
- **Category:** [Specialist / Orchestrator - TDD / Orchestrator - Design Review]
- **Purpose:** [One-line description]

### Testing Checklist
- [ ] Imported mode into Roo Code successfully
- [ ] Tested with 3+ representative tasks
- [ ] Verified tool access works correctly (read/edit/command/browser/mcp)
- [ ] Tested edge cases and error handling
- [ ] If orchestrator: Tested launching specialist modes
- [ ] If specialist: Tested being launched by orchestrators

### Example Usage
Provide an example of using the mode:

```
User input: "..."
Expected output: "..."
```

## For Mode Enhancements

### What Was Improved
- [ ] Better instructions/prompts
- [ ] Additional capabilities
- [ ] Fixed behavior issues
- [ ] Improved error handling
- [ ] Other: [describe]

### Before vs After
**Before:** [Describe previous behavior]
**After:** [Describe new behavior]

## Documentation Updates
- [ ] Updated README.md (if applicable)
- [ ] Updated specialist/orchestrator README (if applicable)
- [ ] Added/updated examples
- [ ] Updated CHANGELOG.md
- [ ] Updated combined import file

## Testing
How have you tested these changes?

### Test Environment
- **Roo Code Version:** [version]
- **OS:** [macOS / Windows / Linux]

### Test Cases
1. **Test 1:** [Describe what you tested]
   - **Result:** [Pass / Fail / Notes]

2. **Test 2:** [Describe what you tested]
   - **Result:** [Pass / Fail / Notes]

3. **Test 3:** [Describe what you tested]
   - **Result:** [Pass / Fail / Notes]

## Quality Checklist
- [ ] YAML syntax is valid
- [ ] Follows naming conventions (slug: lowercase-with-hyphens)
- [ ] Instructions are clear and actionable
- [ ] Mode has a well-defined, focused purpose
- [ ] Documentation is complete and accurate
- [ ] No broken links in documentation
- [ ] Examples are working and tested
- [ ] Code follows project style guidelines

## Breaking Changes
- [ ] This PR introduces breaking changes

If yes, describe what breaks and how to migrate:
[Description of breaking changes and migration path]

## Screenshots/Output (Optional)
If applicable, add screenshots or paste output demonstrating the changes.

```
[Paste output here]
```

## Additional Notes
Any additional context, concerns, or information reviewers should know.

## Reviewer Guidance
What should reviewers focus on?

- [ ] Test the mode with the provided examples
- [ ] Verify documentation accuracy
- [ ] Check for scope creep (mode doing too much)
- [ ] Validate tool access permissions are appropriate
- [ ] Other: [specific review requests]

---

**By submitting this PR, I confirm that:**
- [ ] I have read the [CONTRIBUTING.md](../CONTRIBUTING.md) guidelines
- [ ] My changes follow the project's code style
- [ ] I have tested my changes thoroughly
- [ ] I have updated relevant documentation
- [ ] This PR is ready for review
