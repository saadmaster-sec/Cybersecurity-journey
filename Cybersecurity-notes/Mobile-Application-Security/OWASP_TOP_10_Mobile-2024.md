# OWASP Mobile Top 10 — 2024

The OWASP Mobile Top 10 is an awareness list of important mobile application security risks. It helps organize learning and testing; it is not a complete testing standard and does not replace a threat model or a detailed assessment.

## M1: Improper Credential Usage

**Meaning:** Credentials, tokens, or other authentication material are handled insecurely or used in the wrong place.

**Examples**
- API keys or credentials embedded in source code or app resources.
- Reusing credentials in an unsafe way.
- Storing authentication tokens where other apps or users may access them.
- Treating a client-side secret as truly secret.

**What to inspect**
- Search decompiled code and resources for credential-like strings.
- Check where tokens are stored and how they are transmitted.
- Determine whether exposed values are actual secrets or public identifiers.

**Mitigation**
- Do not embed server-side secrets in an APK.
- Keep credentials out of logs and source control.
- Use short-lived tokens, appropriate rotation, and server-side authorization.
- Store sensitive local values using platform-appropriate protected storage.

## M2: Inadequate Supply Chain Security

**Meaning:** Risks arise from third-party libraries, SDKs, build systems, dependencies, or the software delivery process.

**Examples**
- Outdated vulnerable dependencies.
- Untrusted libraries or compromised build artifacts.
- Unreviewed third-party SDKs collecting excessive data.
- Missing dependency inventory or integrity controls.

**What to inspect**
- Identify libraries and SDKs included in the app.
- Review dependency versions and known vulnerabilities.
- Consider build provenance, signing, and release processes.

**Mitigation**
- Maintain a dependency inventory and update dependencies.
- Obtain components from trusted sources.
- Use reproducible, access-controlled build and release processes where practical.
- Review third-party SDK permissions and data collection.

## M3: Insecure Authentication/Authorization

**Meaning:** Authentication may not reliably establish who a user is, or authorization may fail to restrict what that user can do.

**Examples**
- Relying only on client-side checks.
- Allowing one user to access another user's records by changing an identifier.
- Weak session handling or missing server-side permission checks.
- Treating a hidden button or screen as access control.

**What to inspect**
- Test authentication and authorization boundaries with test accounts.
- Check whether sensitive actions are validated by the server.
- Review session expiration, logout, and account-recovery behavior.

**Mitigation**
- Enforce authorization on the server for every sensitive operation.
- Use secure session lifecycle controls and appropriate authentication.
- Apply least privilege and deny access by default.

## M4: Insufficient Input/Output Validation

**Meaning:** Data crossing trust boundaries is not validated or safely handled.

**Examples**
- Unvalidated data passed to a WebView or another component.
- Unsafe parsing of deep links, intents, files, or API responses.
- Injection flaws caused by treating data as code or commands.
- Sensitive output exposed through an unintended channel.

**What to inspect**
- Trace user-controlled data into sensitive functions and components.
- Test boundary cases with harmless lab inputs.
- Review WebView settings, deep-link handlers, and exported components.

**Mitigation**
- Validate inputs with allowlists and context-aware rules.
- Encode output for its destination context.
- Avoid unsafe interpreters and APIs.
- Apply strict WebView and component configuration.

## M5: Insecure Communication

**Meaning:** Data sent between the app and other systems is not adequately protected.

**Examples**
- Cleartext HTTP carrying sensitive data.
- Incorrect TLS certificate validation.
- Sensitive information sent to unintended hosts.
- Weak network-security configuration.

**What to inspect**
- Review network-security configuration and endpoint URLs.
- Observe traffic in an authorized test environment.
- Verify TLS validation and whether sensitive data is exposed in transit.

**Mitigation**
- Use HTTPS with correct certificate and hostname validation.
- Never disable TLS validation as a production workaround.
- Minimize sensitive data sent over the network.
- Use cleartext-traffic restrictions where appropriate.

## M6: Inadequate Privacy Controls

**Meaning:** Personal or sensitive information is collected, used, retained, or shared without appropriate control or transparency.

**Examples**
- Collecting more data than necessary.
- Sending analytics or identifiers to third parties without suitable disclosure.
- Retaining sensitive data longer than needed.
- Exposing personal data in notifications, logs, backups, or screenshots.

**What to inspect**
- Review permissions, SDK behavior, data flows, and privacy disclosures.
- Identify where personal data is collected, stored, and transmitted.
- Check whether collection is necessary for the feature.

**Mitigation**
- Apply data minimization and purpose limitation.
- Request only necessary permissions.
- Provide clear privacy disclosures and controls.
- Set retention limits and protect sensitive data throughout its lifecycle.

## M7: Insufficient Binary Protections

**Meaning:** The app has insufficient protections against analysis, tampering, repackaging, or unauthorized modification where those threats matter.

**Examples**
- Missing integrity checks for a high-risk use case.
- Sensitive logic exposed in the client without server-side enforcement.
- No risk assessment for repackaging or runtime manipulation.

**What to inspect**
- Review release build settings, signing, and integrity-sensitive workflows.
- Assess whether critical decisions are made entirely on the client.
- Consider whether tampering could bypass meaningful controls.

**Mitigation**
- Keep authoritative security decisions on trusted servers.
- Use signing and release integrity controls.
- Consider obfuscation or tamper detection as defense-in-depth, not as a substitute for secure design.

## M8: Security Misconfiguration

**Meaning:** Platform features, app components, or security settings are configured unsafely.

**Examples**
- Unnecessarily exported activities, services, receivers, or providers.
- Debuggable release builds.
- Overly broad permissions.
- Unsafe WebView settings or unintended backup exposure.

**What to inspect**
- Review `AndroidManifest.xml`, exported components, permissions, intent filters, and build settings.
- Compare debug and release configurations.
- Check whether each exposed component is necessary and properly protected.

**Mitigation**
- Disable debugging in production.
- Explicitly configure component exposure and permissions.
- Remove unused permissions and components.
- Use secure defaults and review release configuration.

## M9: Insecure Data Storage

**Meaning:** Sensitive data is stored in a way that exposes it to unauthorized access.

**Examples**
- Passwords or tokens stored in plaintext.
- Sensitive data written to logs.
- Private data placed in shared or external storage without adequate protection.
- Backups or cached files exposing confidential information.

**What to inspect**
- Review preferences, databases, files, caches, logs, and backups.
- Check whether sensitive values remain after logout.
- Use a test account and dummy values in the lab.

**Mitigation**
- Avoid storing data that is not needed.
- Use app-private storage and platform-appropriate cryptographic protection.
- Keep secrets out of logs and shared storage.
- Define retention and deletion behavior.

## M10: Insufficient Cryptography

**Meaning:** Cryptographic protections are missing, incorrectly implemented, or use unsuitable algorithms or key management.

**Examples**
- Weak or obsolete cryptographic algorithms.
- Hard-coded encryption keys.
- Predictable initialization vectors or insecure modes.
- Custom cryptography instead of reviewed libraries.

**What to inspect**
- Identify cryptographic APIs, algorithms, key generation, and key storage.
- Check whether encryption is authenticated and whether keys are protected.
- Distinguish encoding or hashing from encryption.

**Mitigation**
- Use well-maintained platform or established cryptographic libraries.
- Select modern algorithms and modes appropriate to the use case.
- Generate and protect keys correctly; avoid embedding long-term secrets in the APK.
- Use password hashing designed for passwords rather than fast general-purpose hashes.

---

## Quick reference table

| ID | Category | Main question |
|---|---|---|
| M1 | Improper Credential Usage | Are credentials or tokens handled safely? |
| M2 | Inadequate Supply Chain Security | Can dependencies or build processes introduce risk? |
| M3 | Insecure Authentication/Authorization | Are identity and permissions enforced correctly? |
| M4 | Insufficient Input/Output Validation | Is untrusted data handled safely? |
| M5 | Insecure Communication | Is data protected in transit? |
| M6 | Inadequate Privacy Controls | Is personal data collected and handled appropriately? |
| M7 | Insufficient Binary Protections | Can tampering or repackaging undermine security? |
| M8 | Security Misconfiguration | Are app and platform settings secure? |
| M9 | Insecure Data Storage | Is sensitive data protected at rest? |
| M10 | Insufficient Cryptography | Are cryptography and key management sound? |

## How to use this list in a finding

For each test, record:
- **Category:** OWASP Mobile Top 10 2024 category.
- **Observation:** What was actually observed.
- **Evidence:** Minimal, sanitized screenshots, logs, or commands.
- **Impact:** What an attacker could realistically achieve.
- **Remediation:** A specific secure design or configuration change.
- **Retest:** Whether the issue was resolved.

Do not assign a category based on a guess. Document the evidence and explain why the issue fits the selected category.
