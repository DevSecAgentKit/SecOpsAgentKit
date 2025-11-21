## Description

<!-- Provide a brief description of the changes in this PR -->

## Type of Change

<!-- Mark the relevant option with an 'x' -->

- [ ] New skill
- [ ] Bug fix
- [ ] Documentation update
- [ ] Enhancement to existing skill
- [ ] Infrastructure/tooling improvement

## Checklist

<!-- For new skills, ensure all items are checked before submitting -->

### General Requirements

- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation

### For New Skills

- [ ] Skill initialized using `./scripts/init_skill.sh` or from `_template/`
- [ ] `SKILL.md` frontmatter has all required fields (name, description, version, maintainer, category, tags, frameworks)
- [ ] Description includes specific "Use when:" clause with use cases
- [ ] Version follows semantic versioning (new skills start at 0.1.0)
- [ ] Category matches skill's primary domain
- [ ] All bundled scripts are tested and executable
- [ ] Security considerations are documented
- [ ] No sensitive data or credentials included
- [ ] Validation passes: `./scripts/validate_skill.py skills/<category>/<skill-name>`
- [ ] **README.md updated** with skill entry under appropriate category section
- [ ] **marketplace.json updated** with skill path under appropriate plugin

### For Documentation Updates

- [ ] Changes are accurate and clear
- [ ] Links are valid and working
- [ ] Formatting is consistent with existing documentation

## Testing Performed

<!-- Describe the testing you performed to verify your changes -->

- [ ] Tested all scripts execute without errors
- [ ] Validated against real security scenarios
- [ ] Ran validation script successfully

## Additional Notes

<!-- Add any additional context, screenshots, or information about the PR here -->

## Related Issues

<!-- Link any related issues here using #issue-number -->

Closes #
