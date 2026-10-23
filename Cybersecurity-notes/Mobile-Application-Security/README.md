# Android Application Security Notes

A practical notebook for learning Android application security through static analysis, dynamic testing, and intentionally vulnerable lab applications.

## Contents

1. [OWASP Mobile Top 10 (2024)](OWASP_TOP_10_Mobile-2024.md)
2. [Android Application Security Fundamentals](Android-Application-Security-Fundamentals.md)
3. [APK Files and Android App Structure](APK-Files-and-App-Structure.md)
4. [Android Lab Setup and Tools](Android-Lab-Setup-and-Tools.md)
5. [DIVA Lab Notes](DIVA-Lab-Notes.md)
6. [Static and Dynamic Testing Workflow](Android-Testing-Workflow.md)
7. [Network Traffic, Burp Suite, Frida, and Objection](Network-Testing-and-Dynamic-Analysis.md)

## Learning objectives

- Understand the structure and runtime model of Android applications.
- Recognize common mobile application security weaknesses.
- Inspect APKs and manifests with static-analysis tools.
- Observe application behavior using ADB and Logcat.
- Test storage and network behavior in an isolated, authorized lab.
- Document findings with evidence, impact, and remediation.

## Lab scope and ethics

Use these techniques only on applications you own, intentionally vulnerable training apps, or systems for which you have explicit permission to test. DIVA is intentionally vulnerable and is suitable for a controlled practice environment. Do not use real credentials or sensitive personal data in test cases.

## Suggested workflow

1. Read the relevant OWASP Mobile Top 10 category.
2. Identify the behavior or security property to test.
3. Perform static inspection of the APK and manifest.
4. Observe runtime behavior in the emulator.
5. Record reproducible evidence without including real secrets.
6. Explain impact and recommend a secure design.
7. Retest after the fix, where possible.

> **Version note:** These notes use the OWASP Mobile Top 10 **2024** categories. OWASP lists can change, so check the official project before relying on category names in a formal report.
