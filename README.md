# Host-Based Credential Dumping Case Study: LSASS Memory Scraping

This case study demonstrates a structured investigation of a suspicious host-based credential access attempt involving LSASS (Local Security Authority Subsystem Service) memory scraping. The notebook walks through a complete triage of a simulated EDR alert triggered by abnormal process behavior, culminating in detection of credential harvesting tactics used by attackers in modern enterprise environments.

## Case Summary

- **Alert Trigger**: Anomalous process chain — `explorer.exe` → `cmd.exe` → `powershell.exe` with a base64-encoded command.
- **Target System**: Remote employee workstation (Windows 10).
- **Triage Framework**: Host-based local triage using a 5-step forensic methodology.
- **Key Discovery**: Suspicious PowerShell script decoded to `PowerDump.ps1`, a known credential harvesting tool targeting `lsass.exe`.

## Technical Focus Areas

- Detection and analysis of parent-child process anomalies.
- Understanding memory scraping techniques used to extract credentials from `lsass.exe`.
- Mapping attacker actions across a six-layer Windows operating system triage model.
- Registry persistence, dropped binaries, and command-line obfuscation.
- Use of EDR telemetry and Windows Event Logs for endpoint analysis.

## Notebook Contents

| Section                         | Description |
|----------------------------------|-------------|
| Incident Description             | Overview of the alert and host context |
| System Anatomy                   | Detailed breakdown of host OS layers involved |
| Process Triage                   | Behavioral analysis of process chains |
| Memory Scraping & Credential Access | Review of in-memory data access and scraping tactics |
| Registry & Persistence Mechanisms | Analysis of autostart keys and dropped executables |
| Root Cause Analysis              | Final findings on misconfigurations and impact |
| Lessons Learned                  | Defensive takeaways and mitigation guidance |

## Skills Demonstrated

- Endpoint Detection & Response (EDR) analysis
- Windows host forensics
- Memory forensics and credential access detection
- Threat triage modeling and adversary behavior mapping
- Markdown-based case documentation and structured investigation writing

## Author

**Steven Tuschman**  
CompTIA Security+ | CySA+ (in progress)  
[GitHub: Compcode1](https://github.com/Compcode1)

---

### License

This project is for educational and professional demonstration purposes. No real user or system data is included.
