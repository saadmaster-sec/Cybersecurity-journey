# DIVA Lab Notes

## 1. About DIVA

DIVA (Damn Insecure and Vulnerable App) is an intentionally vulnerable Android application designed for learning common mobile security weaknesses. It is useful for practicing a repeatable workflow: inspect, hypothesize, test, record, and recommend a fix.

**Environment:** NoxPlayer emulator and a DIVA APK in a controlled lab.

## 2. General test workflow

1. Launch DIVA and identify the lab exercise.
2. Read the prompt and identify the security property being tested.
3. Inspect the APK and manifest in JADX GUI.
4. Reproduce the behavior using dummy values.
5. Observe runtime behavior using ADB/Logcat or permitted file inspection.
6. Capture minimal sanitized evidence.
7. Explain impact and recommend a secure implementation.

## 3. Insecure logging

**Concept:** Apps may write sensitive information to system logs. Depending on the device, OS version, app configuration, and privileges, logs may be accessible to unintended parties.

**Lab method**
- Clear logs before starting the exercise.
- Reproduce the relevant action using dummy values.
- Inspect Logcat for unexpected sensitive output.
- Record only the relevant sanitized lines.

```bash
adb logcat -c
adb logcat
```

**Remediation**
- Do not log passwords, access tokens, session identifiers, or other secrets.
- Use appropriate log levels and remove diagnostic logging from production.
- Avoid logging sensitive request/response bodies.

## 4. Hard-coded secrets

**Concept:** Values embedded in an APK can often be extracted by someone who obtains the package. Obfuscation does not turn a client-side value into a server-side secret.

**Lab method**
- Open the APK in JADX GUI.
- Inspect relevant classes and resources.
- Search for credential-like strings, keys, URLs, and authentication logic.
- Determine whether a value is truly secret, a public identifier, or merely test data.

**Remediation**
- Never embed long-term server credentials in a mobile client.
- Move privileged operations to a server.
- Rotate any real secret that has been exposed.
- Avoid committing secrets to GitHub.

## 5. Insecure data storage

**Concept:** Sensitive data stored in plaintext or shared locations may be exposed if the device, app, backup, or storage boundary is compromised.

Potential locations include:
- Shared preferences.
- App-managed files.
- SQLite databases.
- Cache files.
- External/shared storage.
- Logs and backups.

**Lab method**
- Use only dummy credentials.
- Review the app's code for file and preference operations.
- Check app-private locations only when your emulator privileges and lab scope permit it.
- In the DIVA external-storage exercise, the lab may write a hidden file such as `.uinfo.txt` under shared storage. Confirm the actual behavior for the APK version you have rather than assuming the file exists.

Example shell exploration on the emulator:

```bash
adb shell
ls -la /sdcard/
```

Access to paths and files varies with Android version, scoped-storage behavior, app permissions, and emulator configuration.

**Remediation**
- Avoid storing passwords if they are not needed.
- Use app-private storage for app-owned data.
- Use platform-appropriate protected storage for tokens and keys.
- Avoid placing secrets in shared storage.
- Define secure deletion and logout behavior.

## 6. Runtime and error handling

Unexpected error messages may disclose internal paths, implementation details, or assumptions. An error message alone is not automatically a vulnerability; assess what information is exposed and whether it changes attacker capability.

Record:
- Input used (dummy value only).
- Expected behavior.
- Actual behavior.
- Relevant sanitized logs.
- Security impact and proposed fix.

## 7. Finding template

### Finding: [Short, specific title]

- **App / version:**
- **Lab exercise:**
- **OWASP category:**
- **Severity:** Informational / Low / Medium / High / Critical (justify using a consistent methodology)
- **Environment:** Emulator and Android version
- **Preconditions:**
- **Steps to reproduce:**
  1. 
  2. 
  3. 
- **Expected result:**
- **Actual result:**
- **Evidence:** Sanitized screenshot/log or concise observation
- **Impact:**
- **Root cause:**
- **Recommendation:**
- **Retest result:**

## 8. Important reporting principle

Do not report a vulnerability solely because a tool displays a suspicious string or a file exists. Explain the threat, prove the exposure in the authorized environment, and describe realistic impact.
