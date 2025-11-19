# SecOpsAgentKit

An assortment of security operations skills for AI coding agents. A collaborative approach to shift-left security using Claude Code skills.

## Overview

SecOpsAgentKit provides specialized Claude Code skills for security operations, covering:
- **Application Security (AppSec)**: SAST/DAST, vulnerability analysis, secure code review
- **DevSecOps**: CI/CD security, infrastructure as code security, container scanning
- **Secure SDLC**: Threat modeling, security requirements, secure design patterns
- **Compliance**: Security auditing, policy enforcement, compliance frameworks
- **Incident Response**: Security event analysis, forensics, remediation workflows

## Quick Start

### Using a Skill

Skills are automatically discovered by Claude Code when placed in the skills directory. Browse available skills by category:

```bash
skills/
├── appsec/              # Application Security
├── devsecops/           # DevSecOps & CI/CD Security
├── secsdlc/             # Secure SDLC
├── threatmodel/         # Threat Modeling
├── compliance/          # Compliance & Auditing
└── incident-response/   # Incident Response
```

### Contributing a New Skill

1. **Initialize**: Create a new skill from the template
   ```bash
   ./scripts/init_skill.sh my-skill-name appsec
   ```

2. **Develop**: Fill in `SKILL.md` and add bundled resources
   - `scripts/` - Executable security tools
   - `references/` - Security framework documentation
   - `assets/` - Templates and configurations

3. **Validate**: Run the validation script
   ```bash
   ./scripts/validate_skill.py skills/appsec/my-skill-name
   ```

4. **Submit**: Open a PR with the `[skill]` tag

See [CONTRIBUTE.md](CONTRIBUTE.md) for detailed guidelines.

## Skill Standards

All skills follow these requirements:

### Required Frontmatter

```yaml
---
name: skill-name                 # kebab-case identifier
description: >                   # Comprehensive description with use cases
  What the skill does and when to use it...
version: 0.1.0                   # Semantic versioning
maintainer: github-username      # Your GitHub username
category: appsec                 # Primary security domain
tags: [sast, owasp, security]   # Searchable tags
frameworks: [OWASP, CWE]        # Security frameworks referenced
---
```

### Quality Standards

- **Concise**: Keep SKILL.md under 500 lines
- **Tested**: All scripts must be tested and working
- **Secure**: Include security considerations and safe defaults
- **Documented**: Clear instructions using imperative form
- **Versioned**: Follow semantic versioning (MAJOR.MINOR.PATCH)

## Available Skills

### Application Security (appsec/)

*Skills will be listed here as they are added*

### DevSecOps (devsecops/)

*Skills will be listed here as they are added*

### Secure SDLC (secsdlc/)

*Skills will be listed here as they are added*

### Threat Modeling (threatmodel/)

*Skills will be listed here as they are added*

### Compliance (compliance/)

*Skills will be listed here as they are added*

### Incident Response (incident-response/)

*Skills will be listed here as they are added*

## Tools & Scripts

- `scripts/init_skill.sh` - Initialize a new skill from template
- `scripts/validate_skill.py` - Validate skill structure and frontmatter
- `skills/_template/` - Base template for all new skills

## Security Frameworks

Skills in this repository reference industry-standard security frameworks:

- **OWASP** - Open Web Application Security Project
- **CWE** - Common Weakness Enumeration
- **MITRE ATT&CK** - Adversarial Tactics, Techniques & Common Knowledge
- **NIST** - National Institute of Standards and Technology
- **SOC2** - Service Organization Control 2
- **PCI-DSS** - Payment Card Industry Data Security Standard
- **GDPR** - General Data Protection Regulation

## Contributing

We welcome contributions! Please read [CONTRIBUTE.md](CONTRIBUTE.md) for:
- Skill creation guidelines
- Frontmatter standards
- Quality requirements
- Submission process

## Resources

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Claude Code Skills Guide](https://docs.claude.com/en/docs/claude-code/skills)
- [OWASP Top 10](https://owasp.org/Top10/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [MITRE ATT&CK](https://attack.mitre.org/)

## License

[Add your license here]
