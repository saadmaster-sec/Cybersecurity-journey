# SSH from Kali Linux to Windows 11 Lab

## Objective

Configure and establish an SSH connection from a Kali Linux virtual machine to a Windows 11 host using OpenSSH Server.


# Lab Environment

| Device          | Operating System | IP Address    |
| --------------- | ---------------- | ------------- |
| Host Machine    | Windows 11       | 192.x.x.x. |
| Virtual Machine | Kali Linux       | 192.x.x.x |

Network Configuration:

* Bridged Adapter
* Same subnet communication


# Step 1: Install OpenSSH Server on Windows

1. Open **Settings**
2. Navigate to **System → Optional Features**
3. Select **View Features**
4. Search for:

```text
OpenSSH Server
```

5. Install OpenSSH Server

### Screenshot

Add screenshot here:

```text
screenshots/01-openssh-install.png
```

---

# Step 2: Configure SSH Service

1. Press:

```text
Win + R
```

2. Type:

```text
services.msc
```

3. Locate:

```text
OpenSSH SSH Server
```

4. Open Properties
5. Change Startup Type:

```text
Manual → Automatic
```

6. Start the service

### Screenshot

Add screenshot here:

```text
screenshots/02-ssh-service-automatic.png
```

---

# Step 3: Verify SSH Server is Listening

Run PowerShell as Administrator:

```powershell
netstat -ano | findstr :22
```

Expected Output:

```text
TCP    0.0.0.0:22      LISTENING
TCP    [::]:22         LISTENING
```

### Screenshot

Add screenshot here:

```text
screenshots/03-port-22-listening.png
```

---

# Step 4: Verify Firewall Rule

Run:

```powershell
Get-NetFirewallRule -DisplayName "*SSH*"
```

Expected Output:

```text
OpenSSH SSH Server (sshd)
Enabled : True
```

### Screenshot

Add screenshot here:

```text
screenshots/04-firewall-rule.png
```

---

# Step 5: Verify Network Connectivity

From Kali:

```bash
ping -c 4 192.168.0.139
```

Initial Result:

```text
100% packet loss
```

This indicated that the issue was not SSH itself but network communication.

---

# Troubleshooting Process

## Problem

SSH connection was hanging:

```bash
ssh -v sabah@192.168.0.139
```

Output stopped at:

```text
Connecting to 192.168.0.139 port 22
```

No password prompt appeared.

---

## Investigation

### Verified OpenSSH Installation

Confirmed OpenSSH Server was installed.

### Verified Port 22

Confirmed SSH service was listening on port 22.

### Verified IP Addresses

Windows:

```text
192.168.0.139
```

Kali:

```text
192.168.0.151
```

### Ping Testing

Windows → Kali:

```text
Success
```

Kali → Windows:

```text
Failed
```

This indicated a filtering issue.

---

## Root Cause

Windows Firewall was blocking inbound traffic from the Kali VM.

After temporarily disabling Windows Firewall:

```powershell
Set-NetFirewallProfile -Profile Domain,Private,Public -Enabled False
```

Ping succeeded:

```bash
ping -c 4 192.168.0.139
```

Result:

```text
0% packet loss
```

SSH immediately progressed to authentication.

### Screenshot

Add screenshot here:

```text
screenshots/05-successful-ping.png
```

---

# Authentication Issue

SSH reached the login prompt:

```text
sabah@192.168.0.139's password:
```

However, authentication failed.

Reason:

The Windows account used a PIN (Windows Hello).

SSH requires an actual account password and does not accept:

* PIN
* Fingerprint
* Facial Recognition

---

# Solution

Created a dedicated local SSH user:

```powershell
net user sshtest StrongPass123! /add
net localgroup Administrators sshtest /add
```

---

# Successful SSH Connection

From Kali:

```bash
ssh sshtest@192.168.0.139
```

Successful login:

```text
Microsoft Windows [Version 10.0.xxxxx]

sshtest@SAAD C:\Users\sshtest>
```

### Screenshot

Add screenshot here:

```text
screenshots/06-successful-ssh-login.png
```

---

# Lessons Learned

1. Not every SSH issue is an SSH issue.
2. Verify network connectivity before troubleshooting services.
3. Windows Firewall can block communication even when services are running.
4. SSH authentication requires a password, not a Windows PIN.
5. Systematic troubleshooting is more effective than guessing.

---

# Skills Practiced

* OpenSSH Configuration
* Windows Services
* Windows Firewall
* PowerShell Administration
* Network Troubleshooting
* SSH Authentication
* Kali Linux Networking
* Virtual Machine Configuration
