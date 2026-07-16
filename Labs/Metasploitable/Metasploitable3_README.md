# Metasploitable 3 — VAPT Report

Full attack-chain penetration test against Metasploitable 3 — from an unauthenticated
pre-auth SQL injection to complete database compromise and downstream application takeover.

📄 **[Full Report (PDF)](./Saad_Metasploitable3_Report.pdf)**

## Overview

| | |
|---|---|
| **Target** | Metasploitable 3 (Ubuntu 14.04) |
| **Engagement type** | Authorized lab penetration test (academic / self-training) |
| **Testing type** | Black-box, unauthenticated network penetration test |
| **Methodology** | Recon → Enumeration → Vulnerability ID → Exploitation → Post-Exploitation |
| **Tools** | Nmap, Gobuster, smbclient, cURL, Metasploit Framework, Meterpreter, phpMyAdmin |

## The Attack Chain

This engagement's core finding wasn't a single vulnerability — it was a full chain:

```
Pre-auth SQLi in Drupal 7 (Drupalgeddon, CVE-2014-3704)
        ↓  RCE as www-data
Plaintext DB credentials read from settings.php / config.inc.php
        ↓
Full MySQL admin access via phpMyAdmin
        ↓
Payroll database discovered — plaintext passwords + salary data (15 records)
        ↓
Credentials reused → full account takeover of live payroll_app.php
```

## Findings Summary

| ID | Finding | Severity | CVSS | CVE |
|----|---------|----------|------|-----|
| F1 | Pre-Auth SQL Injection → RCE in Drupal Core (Drupalgeddon) | Critical | 9.8 | CVE-2014-3704 |
| F2 | Database Credentials Stored in Plaintext Config Files | Critical | 9.1 | CWE-260 / CWE-798 |
| F3 | Full MySQL Compromise via Harvested Root Credentials | Critical | 9.8 | — |
| F4 | Plaintext Passwords & PII Exposure in Payroll Database | High | 7.5 | CWE-256 |
| F5 | Credential Reuse → Payroll Application Takeover | High | 8.1 | CWE-287 / CWE-521 |
| F6 | Weak / Default FTP Credentials | Medium | 5.3 | CWE-521 |
| F7 | SMB Misconfiguration (Guest Auth, Signing Not Enforced) | Medium | 5.9 | CWE-16 |
| F8 | Apache Directory Listing Enabled | Low | 5.3 | CWE-548 |
| F9 | Drupal Version Disclosure via CHANGELOG.txt | Low | 4.3 | CWE-200 |
| F10 | Multiple SUID Binaries Present (Local PrivEsc Surface) | Informational | N/A | — |

**Overall Risk Rating: Critical**

## Key Takeaways

- A single unpatched CMS (Drupal 7.x) was the root cause of 5 of the 10 findings.
- Demonstrates real-world impact of **plaintext credential storage** and **credential reuse**
  across unrelated systems — the actual force-multiplier in this chain, more than the initial CVE.
- Full report includes reproduction steps, evidence for every finding, CVSS scoring, and
  prioritized remediation guidance.

## Disclaimer

All testing was conducted in an isolated virtual lab environment for educational purposes only.
Techniques demonstrated here should only be used on systems you are explicitly authorized to test.

---

🔗 Part of my broader [Cybersecurity Journey](https://github.com/saadmaster-sec/Cybersecurity-journey) portfolio.

