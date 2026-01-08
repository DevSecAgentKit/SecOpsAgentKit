---
name: ot-security-assessment
description: >
  Operational Technology (OT) security assessment using a two-stage methodology: (1) Identification/Discovery
  of OT devices and protocols, and (2) Vulnerability Assessment using online sources and Metasploit. Use when:
  (1) Conducting authorized OT/ICS security assessments, (2) Identifying and enumerating OT protocols
  (Modbus, S7, IEC 104, DNP3, BACnet, EtherNet/IP), (3) Discovering industrial control devices and PLCs,
  (4) Assessing OT protocol vulnerabilities and security weaknesses, (5) Performing compliance scanning
  aligned with IEC 62443 standards, (6) Validating network segmentation and access controls in OT environments.
version: 0.1.0
maintainer: https://github.com/i8void/
category: offsec
tags: [ot, ics, scada, modbus, siemens, industrial-security, vulnerability-assessment, reconnaissance]
frameworks: [MITRE-ATT&CK, IEC-62443, PTES]
dependencies:
  packages: [nmap, metasploit-framework, python3]
  tools: [modbus-cli, plcscan, python-snap7, pymodbus]
references:
  - https://nmap.org/book/
  - https://docs.rapid7.com/metasploit/msf-overview/
  - https://www.cisa.gov/news-events/cybersecurity-advisories
  - https://attack.mitre.org/techniques/T1046/
  - https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards
---

# OT Security Assessment

## Overview

This skill provides a structured methodology for conducting Operational Technology (OT) and Industrial Control System (ICS) security assessments. The approach follows a two-stage methodology: (1) **Identification/Discovery** of OT devices, protocols, and services, and (2) **Vulnerability Assessment** using online vulnerability databases and Metasploit Framework for deeper analysis.

**IMPORTANT**: OT security assessments may impact critical industrial processes and must only be conducted with proper authorization. Always ensure written permission before assessing OT systems. Never test production systems without explicit authorization.

**OT Network Security Considerations**:
- Well-secured OT systems will not allow internet-connected devices (like this system) to be plugged into the network for assessment
- Most production OT assessments will be conducted offline on air-gapped networks
- This skill is suitable for:
  - Less secure or open OT/SCADA systems
  - Lab environments and test networks
  - Authorized assessment scenarios where network isolation is managed separately
- Always coordinate with operations team to ensure proper network isolation and security controls

## Quick Start

Basic OT device discovery and protocol enumeration:

```bash
# TCP Connect scan for common OT ports (no root required, safer for OT)
nmap -sT -p 502,102,2404,20000,47808,2222 <target-ip>

# Modbus enumeration (no root required)
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-ip>

# Comprehensive OT scan with service detection (no root required)
# Note: dnp3-info script must be downloaded (see DNP3 section below)
nmap -sV -p 502,102,2404,20000,47808,2222 --script modbus-read-registers,s7-info,dnp3-info,bacnet-info <target-ip>
```

## Placeholder System

When executing commands, replace these placeholders with actual values:

- `<target-ip>` - Single IP address (e.g., `192.168.1.100`)
- `<target-network>` - IP range in CIDR notation (e.g., `192.168.1.0/24`)
- `<rhost>` - Remote host (Metasploit) - IP address or hostname
- `<rport>` - Remote port (Metasploit) - Port number
- `<unit-id>` - Modbus unit ID (typically 1-255)

## Core Workflow

### Workflow Checklist (for complex operations)

Progress:
[ ] 1. Verify authorization and scope for OT assessment
[ ] 2. Perform network discovery and identify live hosts
[ ] 3. Scan for common OT protocol ports
[ ] 4. Enumerate OT protocols and identify devices
[ ] 5. Gather device information and service versions
[ ] 6. Research vulnerabilities using online sources
[ ] 7. Perform vulnerability assessment with Metasploit
[ ] 8. Document findings and generate assessment report
[ ] 9. Validate results and identify false positives

Work through each step systematically. Check off completed items.

### 1. Authorization Verification

**CRITICAL**: Before any OT assessment activities:
- Confirm written authorization from system owner and operations team
- Review scope document for in-scope IP ranges and OT systems
- Verify scanning windows and rate-limiting requirements (OT systems are sensitive)
- Document emergency contact for accidental disruption
- Confirm blacklisted hosts (production PLCs, safety systems, critical infrastructure)
- Coordinate with operations team for safe testing windows

### 2. Network Discovery

Identify live hosts in target OT network:

```bash
# Ping sweep (ICMP echo)
nmap -sn <target-network>/24

# ARP scan (local network only, faster and more reliable)
nmap -sn -PR <target-network>/24

# TCP SYN ping (when ICMP blocked, use OT ports)
nmap -sn -PS502,102,2404 <target-network>/24

# Disable ping, assume all hosts alive (common in OT networks)
nmap -Pn <target-network>/24

# Output live hosts to file
nmap -sn <target-network>/24 -oG - | awk '/Up$/{print $2}' > live_hosts.txt
```

**OT Network Discovery Techniques**:
- **ICMP Echo (-PE)**: Standard ping, often blocked in OT networks
- **TCP SYN (-PS)**: Half-open connection to OT protocol ports (502, 102, etc.)
- **UDP (-PU)**: Sends UDP packets to OT UDP ports (47808 for BACnet)
- **ARP (-PR)**: Layer 2 discovery, only works on local network segment

### 3. OT Protocol Port Scanning

Scan discovered hosts for common OT protocol ports:

```bash
# TCP Connect scan for common OT protocol ports (no root required)
nmap -sT -p 502,102,2404,20000,47808,2222,161,623 -iL live_hosts.txt

# Comprehensive scan with service detection (no root required)
nmap -sV -p 502,102,2404,20000,47808,2222 -iL live_hosts.txt -oA ot_scan

# UDP scan for OT protocols (BACnet, SNMP) - requires root
sudo nmap -sU -p 47808,161,623 -iL live_hosts.txt -oA ot_udp_scan
```

**Common OT Protocol Ports**:
- **502**: Modbus TCP
- **102**: S7/Siemens
- **2404**: IEC 104
- **20000**: DNP3
- **47808**: BACnet/IP (UDP)
- **2222**: EtherNet/IP
- **161**: SNMP (UDP)
- **623**: IPMI (UDP)

**Timing and Performance for OT Networks**:

OT networks are sensitive to high traffic volumes. Use conservative timing:

```bash
# Polite (2) - Recommended for OT networks
nmap -T2 --max-rate 10 -p 502,102,2404 <target-ip>

# Scan with delays to avoid disruption
nmap --scan-delay 2s -p 502,102,2404 <target-ip>
```

### 4. OT Protocol Enumeration

Enumerate and identify OT protocols and devices:

#### Modbus TCP (Port 502)

```bash
# Basic Modbus enumeration
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-ip>

# Comprehensive Modbus enumeration
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-ip> -oA modbus_enum

# Read holding registers (unit ID 1, start 0, count 10)
modbus read <target-ip> 502 1 0 10
```

#### S7/Siemens (Port 102)

```bash
# S7 information gathering
nmap -p 102 --script s7-info <target-ip> -oA s7_info

# Python SNAP7 enumeration
python3 -c "import snap7; client = snap7.client.Client(); client.connect('<target-ip>', 0, 1); print(client.get_cpu_info()); client.disconnect()"
```

#### Other OT Protocols

```bash
# IEC 104 (Port 2404)
nmap -p 2404 -sV <target-ip> -oA iec104_scan

# DNP3 (Port 20000)
nmap -p 20000 --script dnp3-info <target-ip> -oA dnp3_info
```

**Note**: The `dnp3-info` NSE script is not included in standard Nmap installations. Download and install it:

```bash
# Download dnp3-info.nse script
wget https://raw.githubusercontent.com/automayt/ICS-pcap/refs/heads/master/DNP3/dnp3-info.nse -O /usr/local/share/nmap/scripts/dnp3-info.nse

# Update Nmap script database
nmap --script-updatedb

# Verify script is available
nmap --script-help dnp3-info

# BACnet/IP (Port 47808/UDP) - requires root for UDP scan
sudo nmap -sU -p 47808 --script bacnet-info <target-ip> -oA bacnet_info

# EtherNet/IP (Port 2222)
nmap -p 2222 -sV <target-ip> -oA ethernetip_tcp
```

### 5. Service and Device Information Gathering

Identify services and extract version information:

```bash
# Service version detection for OT protocols
nmap -sV -p 502,102,2404,20000,47808,2222 <target-ip>

# OT-specific service enumeration (no root required for TCP scans)
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-ip>
nmap -p 102 --script s7-info <target-ip>
# Note: dnp3-info script must be downloaded separately (see DNP3 section)
nmap -p 20000 --script dnp3-info <target-ip>
# UDP scan requires root
sudo nmap -sU -p 47808 --script bacnet-info <target-ip>
```

### 6. Online Vulnerability Research

Research identified devices and services for known vulnerabilities:

```bash
# Query NVD for ICS/SCADA vulnerabilities
curl -s "https://services.nvd.nist.gov/rest/json/cves/2.0?keywordSearch=industrial+control+system" \
  -H "apiKey: <api-key>" -o nvd_ics_$(date +%Y%m%d).json

# Fetch latest ICS-CERT advisories
curl -s "https://www.cisa.gov/news-events/cybersecurity-advisories" \
  -o ics-cert_$(date +%Y%m%d).html

# Search CVE database
curl -s "https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=SCADA" \
  -o cve_scada_$(date +%Y%m%d).html
```

### 7. Metasploit Vulnerability Assessment

Use Metasploit Framework for deeper OT protocol analysis:

```bash
# Start Metasploit Framework console
msfconsole -q

# Search for OT/ICS modules
msf6 > search modbus
msf6 > search scada
msf6 > search siemens
```

#### Using Modbus Metasploit Modules

```bash
msf6 > use auxiliary/scanner/scada/modbus_findunitid
msf6 auxiliary(scanner/scada/modbus_findunitid) > set RHOSTS <target-ip>
msf6 auxiliary(scanner/scada/modbus_findunitid) > set RPORT 502
msf6 auxiliary(scanner/scada/modbus_findunitid) > run

# Read registers
msf6 > use auxiliary/scanner/scada/modbus_read
msf6 auxiliary(scanner/scada/modbus_read) > set RHOSTS <target-ip>
msf6 auxiliary(scanner/scada/modbus_read) > set RPORT 502
msf6 auxiliary(scanner/scada/modbus_read) > set UNIT_ID 1
msf6 auxiliary(scanner/scada/modbus_read) > set REGISTER_START 0
msf6 auxiliary(scanner/scada/modbus_read) > set REGISTER_COUNT 10
msf6 auxiliary(scanner/scada/modbus_read) > run
```

#### Using S7/Siemens Metasploit Modules

```bash
msf6 > use auxiliary/gather/s7_comm_read
msf6 auxiliary(gather/s7_comm_read) > set RHOSTS <target-ip>
msf6 auxiliary(gather/s7_comm_read) > set RPORT 102
msf6 auxiliary(gather/s7_comm_read) > run
```

#### Using Other OT Protocol Modules

```bash
# DNP3
msf6 > use auxiliary/scanner/scada/dnp3_info
msf6 auxiliary(scanner/scada/dnp3_info) > set RHOSTS <target-ip>
msf6 auxiliary(scanner/scada/dnp3_info) > set RPORT 20000
msf6 auxiliary(scanner/scada/dnp3_info) > run

# BACnet
msf6 > use auxiliary/scanner/scada/bacnet_info
msf6 auxiliary(scanner/scada/bacnet_info) > set RHOSTS <target-ip>
msf6 auxiliary(scanner/scada/bacnet_info) > set RPORT 47808
msf6 auxiliary(scanner/scada/bacnet_info) > run
```

### 8. Documentation and Reporting

Organize findings and generate assessment reports:

```bash
# Organized output with timestamps
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-ip> -oA modbus_enum_$(date +%Y%m%d_%H%M%S)

# Create summary report
cat > assessment_summary_$(date +%Y%m%d).md << EOF
# OT Security Assessment Summary

## Target Information
- IP Address: <target-ip>
- Assessment Date: $(date)

## Stage 1: Identification Results
[Insert discovery findings]

## Stage 2: Vulnerability Assessment Results
[Insert vulnerability findings]

## Recommendations
[Insert recommendations]
EOF
```

### 9. Validation and False Positive Analysis

- Manually verify findings with specific tools
- Check service version against CVE databases
- Cross-reference with authenticated vulnerability scanners
- Validate protocol-specific vulnerabilities
- Review Metasploit module outputs for accuracy

## Security Considerations

- **Authorization and Access Control**: OT security assessments require explicit written authorization from system owners and operations teams. Never test production systems without proper authorization. Coordinate with operations team for safe testing windows and rate-limiting requirements.
- **Sensitive Data Handling**: OT assessment findings may contain sensitive information about industrial control systems, network topology, and device configurations. Store assessment data securely and follow data classification requirements. Do not expose OT network details in public repositories or unsecured locations.
- **Access Control**: Commands requiring root/sudo privileges:
  - TCP SYN scans (`nmap -sS`): Requires root for raw sockets
  - UDP scans (`nmap -sU`): Requires root for raw sockets
  - Packet capture (`tcpdump`, `tshark`): Requires root or `CAP_NET_RAW` capability
  
  Commands NOT requiring root:
  - TCP Connect scans (`nmap -sT`): Safe, no root needed
  - Service version detection (`nmap -sV`): No root needed
  - Most Python tools (pymodbus, snap7): Run as regular user
  - Metasploit console: Runs as regular user

- **Claude CLI Safety Considerations**:
  - Review all commands before execution, especially those with `sudo`
  - Prefer `-sT` (TCP Connect) over `-sS` (SYN scan) when possible
  - Configure Linux capabilities instead of full sudo when available:
    ```bash
    sudo setcap cap_net_raw,cap_net_admin+eip $(which nmap)
    # Then run without sudo: nmap -sS <target-ip>
    ```
  - Log all privileged command executions for audit purposes
  - Use principle of least privilege - only elevate when necessary

- **Audit Logging**: Document all assessment activities including:
  - Authorization documents and scope
  - Scanning windows and rate limits
  - All privileged command executions
  - Discovered devices and protocols
  - Vulnerability findings and remediation status

- **Compliance**: OT assessments should align with:
  - IEC 62443 standards for industrial automation and control systems security
  - MITRE ATT&CK for ICS framework
  - PTES (Penetration Testing Execution Standard) methodology
  - Organization-specific OT security policies

## Bundled Resources

This skill does not currently include bundled scripts, references, or assets. All workflows are executed using standard command-line tools (Nmap, Metasploit Framework, Python libraries).

**Future enhancements could include**:
- Scripts for automated OT protocol enumeration
- Reference documentation for detailed OT protocol specifications
- Configuration templates for OT assessment reporting

## Common Patterns

### Pattern 1: OT Network Discovery

```bash
# Phase 1: Identify live hosts
nmap -sn -PE -PS502,102 -PA2404 <target-network>/24 -oG - | awk '/Up$/{print $2}' > ot_hosts.txt

# Phase 2: Scan common OT protocol ports (TCP Connect scan, no root required)
nmap -Pn -sT -sV -p 502,102,2404,20000,47808,2222 -iL ot_hosts.txt -oA ot_scan

# Phase 3: Protocol-specific enumeration
nmap -p 502 --script modbus-read-registers,modbus-read-coils -iL ot_hosts.txt -oA modbus_enum
nmap -p 102 --script s7-info -iL ot_hosts.txt -oA s7_enum
```

### Pattern 2: Modbus Assessment

```bash
# Phase 1: Discover Modbus devices
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-network>/24 -oA modbus_discovery

# Phase 2: Enumerate Modbus data
nmap -p 502 --script modbus-read-registers,modbus-read-coils <target-ip> -oA modbus_enum

# Phase 3: Vulnerability assessment with Metasploit
msfconsole -q
msf6 > use auxiliary/scanner/scada/modbus_findunitid
msf6 auxiliary(scanner/scada/modbus_findunitid) > set RHOSTS <target-ip>
msf6 auxiliary(scanner/scada/modbus_findunitid) > run
```

### Pattern 3: Multi-Protocol OT Assessment

```bash
# Phase 1: Comprehensive OT port scan (TCP Connect, no root required)
nmap -sT -sV -p 502,102,2404,20000,47808,2222,161,623 <target-network>/24 -oA ot_comprehensive

# Phase 2: Protocol-specific enumeration
nmap -p 502 --script modbus-* <target-ip> -oA modbus_full
nmap -p 102 --script s7-* <target-ip> -oA s7_full

# Phase 3: Vulnerability research
# Query NVD, ICS-CERT, manufacturer databases for identified versions

# Phase 4: Metasploit assessment
msfconsole -q
msf6 > search scada
# Use appropriate modules based on discovered protocols
```

## Integration Points

- **Metasploit**: Import Nmap results with `db_import <nmap-xml-file>` for correlation and deeper analysis
- **SIEM Integration**: Parse Nmap XML output for security monitoring and alerting on discovered OT devices
- **Asset Management**: Update CMDB with discovered OT devices, protocols, and service versions
- **Reporting**: Generate structured reports from Nmap XML output and combine with Metasploit results for comprehensive assessment documentation
- **CI/CD**: Not typically applicable for OT assessments due to air-gapped network requirements

## Troubleshooting

### Issue: Nmap scan fails with "Operation not permitted"

**Solution**: This occurs when attempting SYN scans (`-sS`) or UDP scans (`-sU`) without root privileges. Use TCP Connect scans (`-sT`) instead, which do not require root:
```bash
# Instead of: sudo nmap -sS <target-ip>
# Use: nmap -sT <target-ip>
```

### Issue: dnp3-info script not found

**Solution**: The `dnp3-info` NSE script is not included in standard Nmap installations. Download and install it:
```bash
wget https://raw.githubusercontent.com/automayt/ICS-pcap/refs/heads/master/DNP3/dnp3-info.nse -O /usr/local/share/nmap/scripts/dnp3-info.nse
nmap --script-updatedb
```

### Issue: OT devices not responding to scans

**Solution**: OT networks often have strict firewall rules and may block ICMP. Try:
- Disable ping: `nmap -Pn <target-ip>`
- Use TCP Connect scans with conservative timing: `nmap -sT -T2 --max-rate 10 <target-ip>`
- Scan during authorized maintenance windows
- Coordinate with operations team for network access

### Issue: Metasploit module not found

**Solution**: Ensure Metasploit Framework is updated and search for available modules:
```bash
msfconsole -q
msf6 > search scada
msf6 > search modbus
msf6 > search siemens
```

### Issue: High false positive rate in vulnerability assessment

**Solution**: 
- Manually verify findings with protocol-specific tools
- Cross-reference service versions with CVE databases
- Validate protocol-specific vulnerabilities with authenticated scanners
- Review Metasploit module outputs for accuracy

## References

- [Nmap Documentation](https://nmap.org/book/) - Comprehensive Nmap scanning guide
- [Metasploit Framework Modules](https://www.rapid7.com/db/modules/) - Metasploit module documentation
- [CISA ICS-CERT Advisories](https://www.cisa.gov/news-events/cybersecurity-advisories) - Industrial control system security advisories
- [MITRE ATT&CK for ICS](https://attack.mitre.org/techniques/T1046/) - Network service scanning techniques
- [IEC 62443 Standards](https://www.iec.ch/iec62443) - Industrial automation and control systems security standards
