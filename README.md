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
```
/plugin marketplace add https://github.com/DevSecAgentKit/SecOpsAgentKit.git
```


## Available Skills

### Application Security (appsec/)

- **[sast-semgrep](skills/appsec/sast-semgrep/SKILL.md)** - Static application security testing using Semgrep for vulnerability detection | [Tool](https://semgrep.dev/docs/)
- **[sast-bandit](skills/appsec/sast-bandit/SKILL.md)** - Python security vulnerability detection using Bandit SAST with CWE and OWASP mappings | [Tool](https://github.com/PyCQA/bandit)
- **[dast-zap](skills/appsec/dast-zap/SKILL.md)** - Dynamic application security testing using OWASP ZAP (Zed Attack Proxy) | [Tool](https://www.zaproxy.org/docs/)
- **[dast-nuclei](skills/appsec/dast-nuclei/SKILL.md)** - Fast, template-based vulnerability scanning using ProjectDiscovery's Nuclei | [Tool](https://docs.projectdiscovery.io/tools/nuclei/overview)
- **[api-mitmproxy](skills/appsec/api-mitmproxy/SKILL.md)** - Interactive HTTPS proxy for API security testing with traffic interception and modification | [Tool](https://mitmproxy.org/)
- **[dast-ffuf](skills/appsec/dast-ffuf/SKILL.md)** - Fast web fuzzer for directory enumeration and parameter fuzzing | [Tool](https://github.com/ffuf/ffuf)

### DevSecOps (devsecops/)

- **[secrets-gitleaks](skills/devsecops/secrets-gitleaks/SKILL.md)** - Hardcoded secret detection and prevention in git repositories using Gitleaks | [Tool](https://github.com/gitleaks/gitleaks)
- **[iac-checkov](skills/devsecops/iac-checkov/SKILL.md)** - Infrastructure as Code security scanning using Checkov with 750+ built-in policies | [Tool](https://www.checkov.io/)
- **[container-hadolint](skills/devsecops/container-hadolint/SKILL.md)** - Dockerfile security linting and best practice validation using Hadolint | [Tool](https://github.com/hadolint/hadolint)

### Secure SDLC (secsdlc/)

- **[reviewdog](skills/secsdlc/reviewdog/SKILL.md)** - Automated code review and security linting integration for CI/CD pipelines | [Tool](https://github.com/reviewdog/reviewdog)
- **[sast-horusec](skills/secsdlc/sast-horusec/SKILL.md)** - Multi-language static application security testing using Horusec (18+ languages, 20+ tools) | [Tool](https://github.com/ZupIT/horusec)
- **[sbom-syft](skills/secsdlc/sbom-syft/SKILL.md)** - Software Bill of Materials (SBOM) generation using Syft for container images and filesystems | [Tool](https://github.com/anchore/syft)

### Compliance (compliance/)

- **[policy-opa](skills/compliance/policy-opa/SKILL.md)** - Policy-as-code enforcement and compliance validation using Open Policy Agent (OPA) | [Tool](https://www.openpolicyagent.org/docs/latest/)

### Incident Response (incident-response/)

- **[detection-sigma](skills/incident-response/detection-sigma/SKILL.md)** - Generic detection rule creation and management using Sigma (universal SIEM rule format) | [Tool](https://github.com/SigmaHQ/sigma)

### Offensive Security (offsec/)

- **[pentest-metasploit](skills/offsec/pentest-metasploit/SKILL.md)** - Penetration testing framework for exploit development and vulnerability validation | [Tool](https://docs.metasploit.com/)
- **[recon-nmap](skills/offsec/recon-nmap/SKILL.md)** - Network reconnaissance and security auditing using Nmap for port scanning and service detection | [Tool](https://nmap.org/book/)
- **[network-netcat](skills/offsec/network-netcat/SKILL.md)** - Network utility for reading/writing data across TCP/UDP connections and port scanning | [Tool](https://nmap.org/ncat/guide/index.html)
- **[analysis-tshark](skills/offsec/analysis-tshark/SKILL.md)** - Network protocol analyzer and packet capture tool for traffic analysis | [Tool](https://www.wireshark.org/docs/man-pages/tshark.html)
- **[webapp-sqlmap](skills/offsec/webapp-sqlmap/SKILL.md)** - Automated SQL injection detection and exploitation tool for web application security testing | [Tool](https://sqlmap.org/)
- **[webapp-nikto](skills/offsec/webapp-nikto/SKILL.md)** - Web server vulnerability scanner for identifying security issues and misconfigurations | [Tool](https://cirt.net/Nikto2)
- **[crack-hashcat](skills/offsec/crack-hashcat/SKILL.md)** - Advanced password recovery and hash cracking tool supporting multiple algorithms | [Tool](https://hashcat.net/wiki/)


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

### Contributing a New Skill

To kickstart a new skill for this repo:
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

4. **Update Documentation**:
   - Add your skill to the README.md (this file) under the appropriate category
   - Update `.claude-plugin/marketplace.json` with your skill path

5. **Submit**: Open a PR with the `[skill]` tag

See [CONTRIBUTE.md](CONTRIBUTE.md) for detailed guidelines including the exact format for README.md entries.

### Skill Standards

All skills follow these requirements:

#### Required Frontmatter

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

#### Quality Standards

- **Concise**: Keep SKILL.md under 500 lines
- **Tested**: All scripts must be tested and working
- **Secure**: Include security considerations and safe defaults
- **Documented**: Clear instructions using imperative form
- **Versioned**: Follow semantic versioning (MAJOR.MINOR.PATCH)
### Tools & Scripts

- `scripts/init_skill.sh` - Initialize a new skill from template
- `scripts/validate_skill.py` - Validate skill structure and frontmatter
- `skills/_template/` - Base template for all new skills

## Resources

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Claude Code Skills Guide](https://docs.claude.com/en/docs/claude-code/skills)
- [OWASP Top 10](https://owasp.org/Top10/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [MITRE ATT&CK](https://attack.mitre.org/)

## License

This project uses dual licensing:

- **Documentation** (markdown files): [Creative Commons Attribution-ShareAlike 4.0 International (CC-BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/)
- **Code** (scripts, configurations): Dual-licensed under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) and [Mozilla Public License 2.0 (MPL 2.0)](https://www.mozilla.org/en-US/MPL/2.0/)

This means:
- You can freely use, share, and adapt all content with attribution
- Documentation must be shared under the same CC-BY-SA 4.0 license
- Code can be used under either CC-BY-SA 4.0 or MPL 2.0 (your choice)
- MPL 2.0 allows combining with proprietary code while keeping modified MPL files open

See [LICENSE.md](LICENSE.md) for full license texts and details.
