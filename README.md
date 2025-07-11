# 🧠 Host-Based Credential Dumping Case Study: LSASS Memory Scraping

This project analyzes a simulated host-based credential dumping attempt discovered via an anomalous process execution chain. Using Steven Tuschman’s **Cybersecurity Battlefield** framework and a six-layer Windows OS triage model, the investigation traces attacker behavior across system layers—culminating in the discovery of credential access from memory via LSASS.

---

## 🚨 Executive Summary

- **Trigger**: EDR alert flagged abnormal execution: `explorer.exe → cmd.exe → powershell.exe` with base64-encoded script.
- **Triage Method**: Full host-based forensic triage using logs, EDR, registry inspection, memory capture, and network review.
- **Outcome**: Credential harvesting via PowerDump targeting `lsass.exe` confirmed in memory; local persistence and outbound beaconing also observed.

---

## 🧩 Battlefield Mapping

| Battlefield Layer                  | Attack Surface Exploited                               |
|-----------------------------------|--------------------------------------------------------|
| **Layer 1: Process Execution**     | Obfuscated PowerShell launched from GUI shell → cmd    |
| **Layer 2: Startup & Persistence** | Registry Run key & dropped binary (`svcupdate.exe`)    |
| **Layer 3: Background Services**   | Validated service registry entries for tampering       |
| **Layer 4: Credential Management** | Credential scraping via LSASS memory access (`PROCESS_VM_READ`) |
| **Layer 5: Monitoring & Detection**| CrowdStrike Falcon EDR flagged abnormal parent-child chain |
| **Layer 6: Network Communication** | HTTPS beaconing to `auth-verifier[.]net` over TLS      |

---

## 🔬 Key Investigation Steps

### 1. **Windows Event Log Review**
- `Event ID 4688`: Traced suspicious execution chain with `-enc` flag
- `Event ID 4624`: Odd-hour interactive logon
- `Event ID 13`: Registry key created pointing to dropped binary

### 2. **EDR Telemetry Review (CrowdStrike)**
- Parent-child execution tree validated
- PowerShell memory handle to `lsass.exe` confirmed (`PROCESS_VM_READ`)
- Obfuscated script decoded to known PowerDump credential tool

### 3. **Registry & File Inspection**
- Malicious file in `C:\Users\Public\` (unsigned, unknown hash)
- Persistence via `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`

### 4. **Volatile Memory Capture**
- Tool: Magnet RAM Capture → Volatility Framework
- Retrieved PowerDump.ps1 script from memory
- LSASS access confirmed, no rootkit behavior found

### 5. **Network Artifact Review**
- SWG & firewall logs showed outbound beaconing to:
  - `auth-verifier[.]net` (new domain, self-signed TLS cert)
  - IP: `94.130.10.42` on 90-second interval

---

## 🔐 Root Cause & Threat Model

- Attacker operated entirely within GUI session — no phishing or exploit.
- Local admin rights + unrestricted PowerShell enabled credential access.
- Outbound firewall allowed TLS to untrusted domains.
- PowerShell logging was disabled — reducing script visibility.

---

## ✅ Containment Actions

- Host isolated via EDR
- svcupdate.exe quarantined
- Registry keys deleted
- Memory dump preserved
- Credentials rotated & sessions invalidated
- IP/domain block applied in firewall

---

## 🧭 Lessons Learned

- Remove local admin rights from standard users
- Enable PowerShell script block logging
- Block outbound TLS to unvetted domains
- Enforce application allowlisting
- Require MFA for local workstation logon

---

## 💡 Skills Demonstrated

- Host-based forensic triage
- EDR investigation and process chain analysis
- Memory forensics (Volatility + Magnet RAM Capture)
- Adversary behavior modeling using battlefield framework
- Structured investigation documentation

---

## 📁 Repository Contents

| File | Description |
|------|-------------|
| `ioc-lsass-memory-dump.ipynb` | Full triage workflow in Jupyter |
| `memory_sample.vmem` | Captured RAM image (for Volatility) |
| `decoded_script.ps1` | Recovered PowerDump credential script |
| `eventlog_notes.txt` | Key event IDs and triage timeline |

---

## 🔗 Related Projects

- [Splunk SwiftOnSecurity Visibility Upgrade](https://github.com/Compcode1/splunk-swift-detection)
- [Insider Threat Simulation (PowerShell & Scheduled Tasks)](https://github.com/Compcode1/insider-threat-simulation-2)
- [Credential Harvesting via PDF Redirect (IOC 11)](https://github.com/Compcode1/ioc11-credential-harvesting-pdf)

---

© 2025 Steven Tuschman – GitHub: [Compcode1](https://github.com/Compcode1)
