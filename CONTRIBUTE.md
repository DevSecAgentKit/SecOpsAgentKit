# Contributing to SecOpsAgentKit

Thank you for contributing security operations skills to the community! This guide will help you create high-quality skills that follow best practices.

## Table of Contents

- [Quick Start](#quick-start)
- [Skill Requirements](#skill-requirements)
- [Skill Structure](#skill-structure)
- [Frontmatter Standards](#frontmatter-standards)
- [Semantic Versioning](#semantic-versioning)
- [Submission Process](#submission-process)
- [Quality Guidelines](#quality-guidelines)

## Quick Start

1. **Use the template**: Copy `skills/_template/` as your starting point
2. **Initialize your skill**: Run `./scripts/init_skill.sh <skill-name> <category>`
3. **Follow the structure**: See [Skill Structure](#skill-structure) below
4. **Test thoroughly**: Validate your skill works in real security scenarios
5. **Submit a PR**: Open a pull request with the `[skill]` tag

## Skill Requirements

All skills MUST include:

### Required Files

- `SKILL.md` - Main skill file with YAML frontmatter and instructions
- Proper directory structure under `skills/<category>/<skill-name>/`

### Required Frontmatter Fields

```yaml
---
name: skill-name
description: >
  Comprehensive description including what the skill does and when to use it.
  Must include: (1) Primary functionality, (2) Specific use cases,
  (3) Security context
version: 0.1.0
maintainer: github-username
category: appsec|devsecops|secsdlc|threatmodel|compliance|incident-response
tags: [relevant, security, tags]
frameworks: [OWASP|CWE|MITRE-ATT&CK|NIST|SOC2]
---
```

## Skill Structure

### Directory Layout

```
skills/
├── _template/                    # Template for new skills (DO NOT MODIFY)
├── appsec/                       # Application Security skills
├── devsecops/                    # DevSecOps skills
├── secsdlc/                      # Secure SDLC skills
├── threatmodel/                  # Threat Modeling skills
├── compliance/                   # Compliance & Audit skills
└── incident-response/            # Incident Response skills

skills/<category>/<skill-name>/
├── SKILL.md                      # Required: Main skill file
├── scripts/                      # Optional: Executable scripts
│   └── tool_script.py
├── references/                   # Optional: Reference documentation
│   └── standards.md
└── assets/                       # Optional: Templates, configs, etc.
    └── template.yaml
```

### What Goes Where

**SKILL.md**: Core procedural knowledge and workflows. Keep concise (<500 lines).

**scripts/**: Executable code for deterministic, repeated operations
- Security scanning scripts
- Analysis tools
- Automation utilities
- **Must be tested before submission**

**references/**: Documentation loaded on-demand
- Security framework mappings (OWASP, CWE, MITRE ATT&CK)
- Tool documentation
- Detailed schemas or standards
- Remediation guides

**assets/**: Files used in output (not loaded into context)
- Configuration templates
- Policy templates
- Boilerplate secure code
- Report templates

## Frontmatter Standards

### Required Fields

#### `name` (string, required)
The skill identifier in kebab-case.
```yaml
name: sast-vulnerability-analyzer
```

#### `description` (string, required)
**This is critical** - it determines when Claude uses your skill. Must include:
- What the skill does
- When to use it (specific triggers)
- Security operations context

```yaml
description: >
  Static application security testing using Semgrep with OWASP and CWE mapping.
  Use when: (1) Analyzing code for security vulnerabilities, (2) Performing SAST
  scans in CI/CD, (3) Providing remediation guidance with security framework
  references, (4) Prioritizing findings by CVSS score and exploitability.
```

#### `version` (semver, required)
Semantic versioning: `MAJOR.MINOR.PATCH`
```yaml
version: 0.1.0
```

#### `maintainer` (string, required)
Your GitHub username or email.
```yaml
maintainer: github-username
```

#### `category` (string, required)
Primary security domain (choose one):
- `appsec` - Application Security
- `devsecops` - DevSecOps & CI/CD Security
- `secsdlc` - Secure Software Development Lifecycle
- `threatmodel` - Threat Modeling & Risk Analysis
- `compliance` - Compliance & Security Auditing
- `incident-response` - Security Incident Response

```yaml
category: appsec
```

#### `tags` (array, required)
Searchable tags for skill discovery:
```yaml
tags: [sast, semgrep, vulnerability-scanning, owasp, cwe]
```

#### `frameworks` (array, required if applicable)
Security frameworks referenced:
```yaml
frameworks: [OWASP, CWE, MITRE-ATT&CK, NIST, SOC2, PCI-DSS, GDPR]
```

### Optional Fields

#### `dependencies` (object, optional)
External tools or packages required:
```yaml
dependencies:
  python: ">=3.9"
  packages: [semgrep, bandit, safety]
  tools: [docker, git]
```

#### `references` (array, optional)
External documentation links:
```yaml
references:
  - https://owasp.org/Top10/
  - https://cwe.mitre.org/
  - https://semgrep.dev/docs/
```

## Semantic Versioning

Follow semantic versioning (`MAJOR.MINOR.PATCH`):

- **MAJOR** (1.0.0): Breaking changes to skill interface or workflow
  - Changed frontmatter structure
  - Removed or renamed bundled resources
  - Changed expected inputs/outputs

- **MINOR** (0.1.0): New features, backward-compatible
  - New bundled scripts or references
  - Enhanced workflows
  - Additional security framework coverage

- **PATCH** (0.0.1): Bug fixes, documentation updates
  - Fixed scripts
  - Corrected documentation
  - Updated references

**Starting version**: All new skills should start at `0.1.0`

**Version 1.0.0**: Only use when skill is battle-tested and stable

## Submission Process

### 1. Create Your Skill

```bash
# Option A: Use initialization script
./scripts/init_skill.sh my-skill-name appsec

# Option B: Manually copy template
cp -r skills/_template skills/appsec/my-skill-name
```

### 2. Implement & Test

- Fill in `SKILL.md` with clear, concise instructions
- Add any required scripts, references, or assets
- **Test all scripts** - they must execute without errors
- Validate against real security scenarios

### 3. Quality Checklist

Before submitting, ensure:

- [ ] Frontmatter has all required fields
- [ ] Description clearly explains when to use the skill
- [ ] Version follows semver (start with 0.1.0)
- [ ] Category matches skill's primary domain
- [ ] Tags are relevant and searchable
- [ ] All scripts are tested and working
- [ ] No README.md or extraneous documentation files
- [ ] References are properly linked from SKILL.md
- [ ] Security considerations are documented
- [ ] No sensitive data or credentials included

### 4. Submit Pull Request

```bash
git checkout -b skill/my-skill-name
git add skills/appsec/my-skill-name
git commit -m "Add my-skill-name skill for [brief description]"
git push origin skill/my-skill-name
```

Open a PR with:
- **Title**: `[skill] Add <skill-name> - <brief description>`
- **Labels**: `skill`, category label (e.g., `appsec`)
- **Description**:
  - What the skill does
  - Security use cases it addresses
  - Testing performed
  - Any dependencies required

### 5. Review Process

Maintainers will review for:
- Code quality and security best practices
- Documentation completeness
- Testing coverage
- Adherence to standards
- Community value

## Quality Guidelines

### Writing Effective Skills

**Concise is Key**: The context window is shared. Only include what Claude doesn't already know.

**Imperative Form**: Use command form in instructions
- ✅ "Run the security scan"
- ❌ "You should run the security scan"

**Progressive Disclosure**:
- Keep SKILL.md under 500 lines
- Move detailed content to `references/`
- Link clearly to when references should be read

**Security-First**:
- Document sensitive data handling
- Specify required permissions
- Include audit logging guidance
- Note compliance requirements

### Common Pitfalls to Avoid

❌ **Don't create auxiliary files**: No README.md, CHANGELOG.md, etc.
❌ **Don't duplicate content**: Information should live in one place
❌ **Don't include untested scripts**: All scripts must be validated
❌ **Don't use vague descriptions**: Be specific about when to use
❌ **Don't include credentials**: Use environment variables or config
❌ **Don't nest references deeply**: Keep one level from SKILL.md

### Security Considerations

All skills must address:

1. **Sensitive Data**: How to handle secrets, credentials, PII
2. **Access Control**: Required permissions and authorization
3. **Audit Logging**: What should be logged for compliance
4. **Compliance**: Relevant standards (SOC2, GDPR, PCI-DSS)
5. **Safe Defaults**: Secure-by-default configurations

## Getting Help

- Read the [Claude Code Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills)
- Check existing skills in the repository for examples
- Open a discussion for questions
- Review the `_template/` directory

## License

By contributing, you agree that your contributions will be licensed under the same license as this project.