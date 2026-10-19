# DIVA Android Application Security Assessment

## Project Overview

This repository documents a hands-on mobile application security assessment of **DIVA (Damn Insecure and Vulnerable App)**, an intentionally vulnerable Android application designed for learning and practicing Android application security testing.

The project focuses on exploring common mobile application security weaknesses, understanding how they can be identified through practical testing, and documenting findings with supporting evidence.

The assessment was conducted in a controlled testing environment for educational and cybersecurity portfolio purposes.

## Objectives

- Practice Android application security testing techniques.
- Inspect application behavior and analyze HTTP requests and responses.
- Investigate potential security weaknesses through dynamic and static analysis.
- Document reproducible findings and their potential impact.
- Understand security improvements and remediation approaches.

## Tools and Technologies

- **Burp Suite** — Intercepting and inspecting application traffic.
- **ADB (Android Debug Bridge)** — Interacting with the Android testing environment.
- **JADX GUI** — Inspecting decompiled Android application code.
- **Android Emulator (NoxPlayer)** — Running the application in a controlled testing environment.

## Project Files

| File / Folder | Description |
|---|---|
| `DIVA_Report.pdf` | PDF report documenting the assessment and findings. |
| `screenshots/` | Additional screenshots showing testing activities, evidence, and observations. |

*More screenshots and supporting visual evidence are available in the `screenshots` folder.*

## Testing Methodology

The assessment involved a combination of dynamic application testing and static code inspection.

The general workflow included:

1. Setting up the Android application in a controlled testing environment.
2. Observing application behavior and relevant network traffic.
3. Inspecting requests and responses to investigate potential weaknesses.
4. Reviewing application code where relevant to understand implementation details.
5. Recording observations and validating findings.
6. Documenting potential impact and recommending remediation where applicable.

## Findings and Evidence

The accompanying PDF report contains the documented assessment results.

Supporting screenshots are available in the `screenshots/` directory to provide additional context and visual evidence for the testing process.

Findings should be interpreted within the scope and limitations of the testing environment described in the report.

## Security Recommendations

General recommendations for improving Android application security include:

- Validate security-sensitive inputs and business rules on the server side.
- Enforce authentication and authorization checks where required.
- Avoid storing sensitive information insecurely.
- Protect sensitive communications using appropriate transport security.
- Review application code and configuration for security weaknesses.
- Follow applicable OWASP Mobile Application Security guidance.

## Disclaimer

This project was conducted for educational purposes in a controlled environment using an intentionally vulnerable application.

The findings and techniques documented here are intended to support security education, defensive development, and authorized security testing.

## Author

Cybersecurity enthusiast focused on practical application security testing, vulnerability assessment, and penetration testing.

