# SecOpsAgentKit Skill Reference

Quick reference guide for creating and maintaining security operations skills.

## Frontmatter Quick Reference

### Required Fields

```yaml
---
name: skill-name                          # kebab-case, lowercase with hyphens
description: >                            # Multi-line, include "Use when" clause
  What the skill does and specific use cases.
  Use when: (1) First scenario, (2) Second scenario, (3) Third scenario
version: 0.1.0                            # Semantic versioning: MAJOR.MINOR.PATCH
maintainer: github-username               # Your GitHub username or email
category: appsec                          # One of the valid categories
tags: [sast, security, owasp]            # Array of searchable tags
frameworks: [OWASP, CWE, MITRE-ATT&CK]   # Referenced security frameworks
---
```

### Optional Fields

```yaml
dependencies:                             # External requirements
  python: ">=3.9"
  packages: [semgrep, bandit]
  tools: [docker, git]
references:                               # External documentation
  - https://owasp.org/Top10/
  - https://example.com/docs
```

## Valid Categories

- `appsec` - Application Security
- `devsecops` - DevSecOps & CI/CD Security
- `secsdlc` - Secure Software Development Lifecycle
- `threatmodel` - Threat Modeling & Risk Analysis
- `compliance` - Compliance & Security Auditing
- `incident-response` - Security Incident Response

## Common Security Frameworks

- `OWASP` - Open Web Application Security Project
- `CWE` - Common Weakness Enumeration
- `MITRE-ATT&CK` - Adversarial Tactics, Techniques & Common Knowledge
- `NIST` - National Institute of Standards and Technology
- `SOC2` - Service Organization Control 2
- `PCI-DSS` - Payment Card Industry Data Security Standard
- `GDPR` - General Data Protection Regulation
- `ISO27001` - Information Security Management
- `HIPAA` - Health Insurance Portability and Accountability Act

## Semantic Versioning Guide

### Version Format: MAJOR.MINOR.PATCH

**MAJOR** (1.0.0) - Breaking changes:
- Changed frontmatter structure
- Removed bundled resources
- Changed expected inputs/outputs
- Incompatible workflow changes

**MINOR** (0.1.0) - New features (backward-compatible):
- New bundled scripts or references
- Enhanced workflows
- Additional framework coverage
- New optional features

**PATCH** (0.0.1) - Bug fixes and docs:
- Fixed scripts
- Documentation updates
- Typo corrections
- Reference updates

### Version Lifecycle

- Start new skills at: `0.1.0`
- First stable release: `1.0.0`
- Breaking change: `1.0.0` → `2.0.0`
- New feature: `1.0.0` → `1.1.0`
- Bug fix: `1.0.0` → `1.0.1`

## Directory Structure

```
skills/<category>/<skill-name>/
├── SKILL.md                    # Required: Main skill file
├── scripts/                    # Optional: Executable code
│   ├── .gitkeep
│   └── analyzer.py
├── references/                 # Optional: Reference docs
│   ├── owasp_mapping.md
│   └── remediation_guide.md
└── assets/                     # Optional: Templates/configs
    ├── config_template.yaml
    └── report_template.md
```

## Skill Checklist

Before submitting:

- [ ] All required frontmatter fields present
- [ ] Description includes "Use when" clause
- [ ] Version follows semver (0.1.0 for new skills)
- [ ] Category is valid
- [ ] Tags are relevant and searchable
- [ ] Maintainer field updated (not placeholder)
- [ ] All scripts tested and executable
- [ ] No auxiliary files (README.md, CHANGELOG.md)
- [ ] References linked from SKILL.md
- [ ] Security considerations documented
- [ ] No credentials or sensitive data
- [ ] SKILL.md under 500 lines
- [ ] Validation script passes

## Common Patterns

### Security Considerations Section

```markdown
## Security Considerations

- **Sensitive Data Handling**: How to handle secrets, credentials, PII
- **Access Control**: Required permissions and authorization context
- **Audit Logging**: What should be logged for compliance
- **Compliance**: Relevant standards (SOC2, GDPR, PCI-DSS)
- **Safe Defaults**: Secure-by-default configurations
```

### Script Template

```python
#!/usr/bin/env python3
"""Brief description of what this script does."""

import argparse
import sys
from pathlib import Path

def main():
    parser = argparse.ArgumentParser(description="Description")
    parser.add_argument("input_file", type=Path)
    parser.add_argument("--output", type=Path)
    args = parser.parse_args()

    try:
        # Implementation
        return 0
    except Exception as e:
        print(f"Error: {e}", file=sys.stderr)
        return 1

if __name__ == "__main__":
    sys.exit(main())
```

### Reference Document Template

```markdown
# Reference Title

## Table of Contents
- [Section 1](#section-1)
- [Security Standards](#security-standards)

## Section 1
Detailed information...

## Security Standards
### OWASP Mapping
- A01: Broken Access Control
### CWE Mapping
- CWE-79: Cross-site Scripting
```

## Commands

### Initialize New Skill
```bash
./scripts/init_skill.sh <skill-name> <category>
```

### Validate Skill
```bash
./scripts/validate_skill.py skills/<category>/<skill-name>
```

### Test Script
```bash
chmod +x skills/<category>/<skill-name>/scripts/script.py
./skills/<category>/<skill-name>/scripts/script.py --help
```

## Resources

- [CONTRIBUTE.md](CONTRIBUTE.md) - Full contribution guidelines
- [Claude Code Skills Docs](https://docs.claude.com/en/docs/claude-code/skills)
- [skills/_template/](skills/_template/) - Template directory
