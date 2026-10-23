# APK Files and Android App Structure

## 1. What is an APK?

An **Android Package (APK)** is an installable package format for Android applications. It contains compiled code, resources, metadata, and other files needed to install and run an app.

An APK is package-like in structure and can often be inspected as an archive, but compiled Android code and resources are not the same as ordinary source files.

## 2. Common APK contents

| File or directory | Purpose |
|---|---|
| `AndroidManifest.xml` | App metadata, components, permissions, and configuration; often compiled/binary XML |
| `classes.dex` | Dalvik Executable bytecode for app code |
| `classes2.dex`, etc. | Additional DEX files when present |
| `res/` | Compiled Android resources |
| `resources.arsc` | Compiled resource table, when present |
| `assets/` | App-provided assets |
| `lib/` | Native libraries, commonly organized by CPU architecture |
| `META-INF/` | May contain signature-related or package metadata; contents vary by build and signing scheme |

Not every APK contains every entry. Modern Android app delivery can also use Android App Bundles and split APKs.

## 3. AndroidManifest.xml

The manifest describes key app configuration, including:
- Package/application metadata.
- Activities, services, broadcast receivers, and content providers.
- Permissions.
- Intent filters and deep links.
- Component exposure and relevant security settings.
- Backup and debugging-related configuration.

### Security review checklist

- Are any components unnecessarily exported?
- Are exported components protected by suitable permissions or caller checks?
- Are intent filters and deep links handled safely?
- Are excessive permissions declared?
- Is debugging disabled in release builds?
- Are backup and cleartext network settings appropriate for the app?

**Important:** A manifest entry alone does not always prove exploitability. Check the component's behavior, Android version, and relevant defaults.

## 4. Activities, services, receivers, and providers

- **Activity:** a screen or user-interaction entry point.
- **Service:** component for work that may not have a UI.
- **Broadcast receiver:** responds to broadcast intents.
- **Content provider:** exposes structured data through a defined interface.

An exported component may be callable by other apps, subject to Android rules and any permissions or checks. Test exposure in an authorized lab and validate the impact rather than assuming every exported component is vulnerable.

## 5. Intent filters and deep links

Intent filters describe which kinds of intents a component can handle. Deep links can open app screens from URLs or other app-to-app navigation.

Potential risks include:
- Trusting caller-supplied extras without validation.
- Opening sensitive screens without appropriate authorization.
- Unsafe URL parsing or WebView navigation.
- Exposing internal functionality through unintended routes.

Mitigations include strict input validation, explicit component design where appropriate, server-side authorization, and careful handling of URLs and extras.

## 6. Useful inspection tools

- **JADX GUI:** decompiles DEX code into a readable approximation of Java and displays resources/manifest data.
- **Apktool:** decodes resources and manifest data and can disassemble/rebuild APKs.
- **ADB:** communicates with a connected device or emulator.
- **Android Studio tools:** useful for platform documentation, debugging, and app inspection.

Decompiled output is an approximation; names may be obfuscated and control flow may be difficult to interpret. Confirm important conclusions with additional evidence.

## 7. Example inspection workflow

1. Record the APK filename, app version, and source.
2. Inspect the manifest and permissions.
3. Identify exposed components and deep-link handlers.
4. Search code/resources for endpoints, credential-like strings, WebView usage, storage APIs, and cryptographic APIs.
5. Form a test hypothesis.
6. Validate behavior dynamically in an authorized emulator.
7. Record sanitized evidence and remediation.

## 8. Safe command examples

Inspect connected devices:

```bash
adb devices
```

Show package details on a test device:

```bash
adb shell pm list packages
```

Find a package's installed APK path:

```bash
adb shell pm path <package_name>
```

Pull an APK only when you are authorized to access it:

```bash
adb pull <device_apk_path> ./app.apk
```

Replace placeholders with values from your own lab. Do not assume every device permits access to every path.
