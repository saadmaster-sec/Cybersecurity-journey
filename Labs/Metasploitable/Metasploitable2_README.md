# Metasploitable 2 — VAPT Report

Vulnerability Assessment & Penetration Test conducted against Metasploitable 2 as part of a
cybersecurity course engagement (Red&White Skill Education).

📄 **[Full Report (PDF)](./Saad_Metasploitable2_Report.pdf)**

## Overview

| | |
|---|---|
| **Target** | Metasploitable 2 (Ubuntu 8.04, kernel 2.6.24) |
| **Engagement type** | Course assignment — VAPT |
| **Attacker host** | Kali Linux |
| **Methodology** | Recon → Scanning → Vulnerability Assessment → Exploitation → Reporting |
| **Tools** | Nmap 7.99, Metasploit Framework 6.4.135, smbclient, MySQL client, Telnet client, Netcat |

## Findings Summary

| ID | Finding | Service | Severity | Exploited | CVE |
|----|---------|---------|----------|-----------|-----|
| F-01 | vsFTPd 2.3.4 Backdoor | FTP | Critical | ✅ Root | CVE-2011-2523 |
| F-02 | Samba usermap_script Command Injection | SMB | Critical | ✅ Root | CVE-2007-2447 |
| F-03 | Anonymous SMB Read/Write Access | SMB | High | ✅ | N/A |
| F-04 | Tomcat Default Credentials | HTTP | High | ✅ | N/A |
| F-05 | MySQL Root Account, No Password | MySQL | Critical | ✅ | N/A |
| F-06 | Telnet Weak / Default Credentials | Telnet | High | ✅ | N/A |
| F-07 | SSL POODLE | HTTPS | Medium | Detected only | CVE-2014-3566 |
| F-08 | Logjam / Weak Diffie-Hellman | HTTPS | Medium | Detected only | CVE-2015-4000 |

## Key Takeaways

- Four findings resulted in **full root-level compromise** with zero valid credentials.
- Nessus was substituted with `nmap --script vuln` due to lab resource constraints — documented
  as a methodology adaptation rather than a gap.
- Every finding includes reproduction steps, evidence screenshots, impact analysis, and
  remediation guidance.

## Disclaimer

All testing was conducted in an isolated virtual lab environment for educational purposes only.
Techniques demonstrated here should only be used on systems you are explicitly authorized to test.

---

🔗 Part of my broader [Cybersecurity Journey](https://github.com/saadmaster-sec/Cybersecurity-journey) portfolio.

