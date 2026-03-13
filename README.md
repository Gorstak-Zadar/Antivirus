<div align="center">

# 🛡️ Antivirus & EDR Protection

### A Comprehensive PowerShell-Based Endpoint Detection & Response System

[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?style=for-the-badge&logo=powershell&logoColor=white)](https://docs.microsoft.com/en-us/powershell/)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Maintenance](https://img.shields.io/badge/Maintained-Yes-brightgreen?style=for-the-badge)](https://github.com/)

<br/>

> 🔒 **Single-file, zero-dependency** real-time antivirus and EDR solution for Windows,  
> featuring **67+ detection modules**, MITRE ATT&CK mapping, and automated threat response.

<br/>

[📥 Installation](#-installation) · [⚙️ Configuration](#%EF%B8%8F-configuration) · [📖 Modules](#-detection-modules) · [🗺️ Architecture](#%EF%B8%8F-architecture) · [❓ FAQ](#-faq)

</div>

---

## 📋 Table of Contents

- [✨ Features](#-features)
- [🚀 Quick Start](#-quick-start)
- [📥 Installation](#-installation)
- [⚙️ Configuration](#%EF%B8%8F-configuration)
- [📖 Detection Modules](#-detection-modules)
  - [🔍 Malware & Threat Detection](#-malware--threat-detection)
  - [🧠 Behavioral Analysis](#-behavioral-analysis)
  - [🌐 Network Security](#-network-security)
  - [🔐 Credential & Identity Protection](#-credential--identity-protection)
  - [💾 Persistence Detection](#-persistence-detection)
  - [🖥️ System Integrity](#%EF%B8%8F-system-integrity)
  - [📊 Monitoring & Logging](#-monitoring--logging)
  - [🛠️ Hardening & Mitigation](#%EF%B8%8F-hardening--mitigation)
  - [🕵️ Privacy & Anti-Fingerprinting](#%EF%B8%8F-privacy--anti-fingerprinting)
- [🗺️ Architecture](#%EF%B8%8F-architecture)
- [📁 Directory Structure](#-directory-structure)
- [🔄 Job Scheduler](#-job-scheduler)
- [📊 Threat Intelligence Feeds](#-threat-intelligence-feeds)
- [🗺️ MITRE ATT&CK Coverage](#%EF%B8%8F-mitre-attck-coverage)
- [⚡ Performance](#-performance)
- [🔧 Troubleshooting](#-troubleshooting)
- [❓ FAQ](#-faq)
- [🤝 Contributing](#-contributing)
- [📜 License](#-license)

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🎯 Core Capabilities
- 🔁 **67+ detection modules** running concurrently
- ⏱️ **Configurable intervals** (2s – 86400s per module)
- 🚨 **Automated threat response** (kill, quarantine, alert)
- 📝 **Windows Event Log** integration
- 🔒 **Anti-termination** safeguards & process watchdog
- 🔄 **Auto-restart** on crash / termination
- 🧬 **YARA rule** scanning support
- 🗺️ **MITRE ATT&CK** technique mapping

</td>
<td width="50%">

### 🏗️ Design Principles
- 📄 **Single-file deployment** — no dependencies
- 🛡️ **Runs as Administrator** with graceful elevation
- 🔐 **HMAC integrity** verification on databases
- 📊 **Stability logging** for diagnostics
- 🧩 **Modular architecture** — easy to extend
- 💾 **Persistent quarantine** with metadata
- 🔑 **Mutex-based** single-instance enforcement
- 🌐 **Multi-feed threat intelligence** lookups

</td>
</tr>
</table>

---

## 🚀 Quick Start

```powershell
# 1️⃣  Open PowerShell as Administrator
# 2️⃣  Allow script execution (if not already set)
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

# 3️⃣  Run the antivirus
.\Antivirus.ps1
```

That's it! The script will:
- ✅ Auto-elevate to Administrator if needed
- ✅ Create working directories under `C:\ProgramData\AntivirusProtection`
- ✅ Register and start all 67+ detection modules
- ✅ Enter the real-time monitoring loop

---

## 📥 Installation

### 🔹 Method 1: Direct Download

```powershell
# Download the script
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/<your-repo>/main/Antivirus.ps1" -OutFile "Antivirus.ps1"

# Run it
.\Antivirus.ps1
```

### 🔹 Method 2: Clone the Repository

```bash
git clone https://github.com/<your-repo>/antivirus-edr.git
cd antivirus-edr
```

```powershell
.\Antivirus.ps1
```

### 🔹 Uninstall

```powershell
.\Antivirus.ps1 -Uninstall
```

> 🧹 This removes all installed files, scheduled tasks, and protection data from `C:\ProgramData\AntivirusProtection`.

---

## ⚙️ Configuration

All configuration is defined at the top of `Antivirus.ps1` in the `$Config` hashtable:

| Parameter | Default | Description |
|---|---|---|
| `EDRName` | `"MalwareDetector"` | Display name for the EDR engine |
| `LogPath` | `...\Logs` | Directory for log files |
| `QuarantinePath` | `...\Quarantine` | Quarantine storage location |
| `DatabasePath` | `...\Data` | Hash databases & state files |
| `WhitelistPath` | `...\Data\whitelist.json` | Trusted file whitelist |
| `EnableUnsignedDLLScanner` | `$true` | Scan for unsigned DLLs in processes |
| `AutoKillThreats` | `$true` | Automatically terminate threat processes |
| `AutoQuarantine` | `$true` | Automatically quarantine malicious files |
| `MaxMemoryUsageMB` | `500` | Memory usage cap for the script |

### 🕐 Module Intervals

Each detection module's scan interval is configured in `$Script:ManagedJobConfig`:

```powershell
$Script:ManagedJobConfig = @{
    HashDetectionIntervalSeconds           = 15     # ⚡ High frequency
    RansomwareDetectionIntervalSeconds     = 15     # ⚡ High frequency
    GFocusIntervalSeconds                  = 2      # ⚡ Real-time
    NeuroBehaviorMonitorIntervalSeconds    = 15     # ⚡ High frequency
    BeaconDetectionIntervalSeconds         = 60     # 🔄 Standard
    BrowserExtensionMonitoringIntervalSeconds = 300 # 🐢 Low frequency
    ScriptBlockLoggingCheckIntervalSeconds = 86400  # 📅 Daily
    # ... 60+ more modules
}
```

---

## 📖 Detection Modules

### 🔍 Malware & Threat Detection

| Module | Interval | Description |
|---|---|---|
| 🔢 `HashDetection` | 15s | SHA-256 hash lookups against CIRCL, Cymru & MalwareBazaar |
| 🧬 `YaraDetection` | 120s | YARA rule-based signature scanning across temp directories |
| 🎯 `AdvancedThreatDetection` | 20s | Multi-vector system-wide threat analysis |
| 🔧 `AttackToolsDetection` | 30s | Detects known offensive security tools (Mimikatz, Cobalt Strike, etc.) |
| 📈 `FileEntropyDetection` | 120s | Shannon entropy analysis for packed/encrypted binaries |
| 🍯 `HoneypotMonitoring` | 30s | Deploys & monitors decoy files for unauthorized access |
| 🦠 `ElfCatcher` | 30s | Detects ELF binaries (Linux malware) on Windows systems |
| 💥 `CrudePayloadGuard` | 60s | Scans process command lines for XSS/injection payloads |

### 🧠 Behavioral Analysis

| Module | Interval | Description |
|---|---|---|
| 🧩 `ProcessAnomalyDetection` | 15s | Detects anomalous process behavior patterns |
| 🎭 `ProcessHollowingDetection` | 30s | Identifies process hollowing / PE injection |
| 💉 `CodeInjectionDetection` | 30s | Detects cross-process code injection |
| 🪞 `ReflectiveDLLInjectionDetection` | 30s | Identifies reflective DLL loading in memory |
| 🔗 `DLLHijackingDetection` | 90s | Scans for DLL search-order hijacking |
| 👨‍👦 `SuspiciousParentChildDetection` | 45s | Flags unusual parent-child process trees (e.g., Word → cmd) |
| 🧠 `NeuroBehaviorMonitor` | 15s | Screen/cursor/focus behavioral anomaly scoring engine |
| 📜 `ScriptContentScan` | 120s | Scans script file contents for threat patterns |
| 📜 `ScriptHostDetection` | 60s | Detects abuse of wscript/cscript/mshta |
| 🧪 `LOLBinDetection` | 15s | Identifies Living-off-the-Land binary abuse |
| 👻 `FilelessDetection` | 20s | Detects in-memory / fileless malware techniques |
| 🧠 `MemoryScanning` | 90s | Deep memory scanning for injected code |
| 🏭 `ProcessCreationDetection` | 10s | Real-time process creation monitoring |

### 🌐 Network Security

| Module | Interval | Description |
|---|---|---|
| 🌐 `NetworkAnomalyDetection` | 30s | Anomalous network connection detection |
| 📡 `NetworkTrafficMonitoring` | 45s | Continuous traffic pattern analysis |
| 📻 `BeaconDetection` | 60s | C2 beacon interval pattern detection |
| 🕳️ `DNSExfiltrationDetection` | 30s | DNS tunneling / data exfiltration detection |
| 📤 `DataExfiltrationDetection` | 30s | Outbound data leak analysis |
| 🔀 `LateralMovementDetection` | 30s | Detects lateral movement (PsExec, WMI, RDP) |
| 🛡️ `IdsDetection` | 60s | Signature-based intrusion detection scanning |
| 🔥 `GRulesC2Block` | 3600s | Firewall rules blocking known C2 IPs from threat intel feeds |
| 🕶️ `LocalProxyDetection` | 60s | Detects malicious local proxy / MITM setups |
| 🔍 `GFocus` | 2s | High-frequency browser connection tracker |

### 🔐 Credential & Identity Protection

| Module | Interval | Description |
|---|---|---|
| 🔑 `CredentialDumpDetection` | 15s | Detects Mimikatz, LSASS dumping, credential harvesting |
| 🛡️ `CredentialProtection` | 300s | Enforces LSA protection, detects credential-theft tools |
| 🔓 `TokenManipulationDetection` | 60s | Identifies token impersonation / privilege escalation |
| 🔐 `PasswordManagement` | 120s | Secure password store & credential monitoring |
| ⌨️ `KeyloggerDetection` | 45s | Detects keylogger hooks and keyboard intercepts |
| 🔀 `KeyScramblerManagement` | 60s | Manages kernel-level keystroke encryption |
| 💾 `MemoryAcquisitionDetection` | 90s | Detects memory dumping tools (WinPMem, etc.) |
| 🛡️ `AMSIBypassDetection` | 15s | Detects AMSI (Antimalware Scan Interface) bypasses |

### 💾 Persistence Detection

| Module | Interval | Description |
|---|---|---|
| 📋 `RegistryPersistenceDetection` | 120s | Scans Run/RunOnce registry keys for persistence |
| ⏰ `ScheduledTaskDetection` | 120s | Flags suspicious scheduled tasks |
| 🔧 `WMIPersistenceDetection` | 120s | Detects WMI event subscription persistence |
| 📂 `StartupPersistenceDetection` | 120s | Monitors startup folders & registry autoruns |
| 🔌 `COMMonitoring` | 120s | Detects COM object hijacking / persistence |
| 🛎️ `ServiceMonitoring` | 60s | Monitors for unauthorized service installations |

### 🖥️ System Integrity

| Module | Interval | Description |
|---|---|---|
| 👑 `RootkitDetection` | 180s | Multi-layer rootkit detection (SSDT, IDT, DKOM) |
| 💿 `BCDSecurity` | 300s | Boot Configuration Data integrity verification |
| 🖥️ `DriverWatcher` | 60s | Monitors for unauthorized / non-whitelisted drivers |
| 🔐 `ScriptBlockLoggingCheck` | 24h | Ensures PowerShell script block logging is enabled |
| 📝 `ProcessAuditing` | 24h | Enables & verifies Windows process creation auditing |
| 📁 `RealTimeFileMonitor` | 60s | FileSystemWatcher-based real-time file change detection |
| 🔓 `UnsignedDLLRemover` | 300s | Removes unsigned DLLs from protected processes |
| 🔓 `ElfDLLUnloader` | 10s | Unloads suspicious DLLs from protected processes |

### 📊 Monitoring & Logging

| Module | Interval | Description |
|---|---|---|
| 📜 `EventLogMonitoring` | 60s | Windows Event Log anomaly detection |
| 🔥 `FirewallRuleMonitoring` | 120s | Tracks unauthorized firewall rule changes |
| 📋 `ClipboardMonitoring` | 30s | Monitors clipboard for sensitive data theft |
| 📡 `NamedPipeMonitoring` | 45s | Detects suspicious named pipe usage (C2 comms) |
| 🔌 `USBMonitoring` | 20s | USB device insertion tracking & alerting |
| 📱 `MobileDeviceMonitoring` | 15s | Connected mobile device monitoring |
| 🌐 `BrowserExtensionMonitoring` | 300s | Detects malicious browser extensions |
| 📷 `WebcamGuardian` | 5s | Monitors & controls webcam access |
| 📊 `ShadowCopyMonitoring` | 30s | Protects Volume Shadow Copies from deletion |
| 🗺️ `MitreMapping` | 300s | Maps all detections to MITRE ATT&CK techniques |
| 🚨 `ResponseEngine` | 10s | Centralized automated threat response orchestrator |
| 🗃️ `QuarantineManagement` | 300s | Manages quarantine lifecycle & cleanup |

### 🛠️ Hardening & Mitigation

| Module | Interval | Description |
|---|---|---|
| 🩹 `CVE-MitigationPatcher` | 1h | Fetches CISA KEV & auto-applies known CVE mitigations |
| 🛡️ `AsrRules` | 24h | Enforces Attack Surface Reduction rules via Defender |
| 🔥 `GRulesC2Block` | 1h | Blocks C2 IPs via Windows Firewall from threat feeds |
| 📝 `ProcessAuditing` | 24h | Hardens process creation auditing & command-line logging |
| 🔐 `ScriptBlockLoggingCheck` | 24h | Hardens PowerShell logging configuration |
| ⌨️ `HidMacroGuard` | 60s | Detects HID/USB macro injection attacks (Rubber Ducky) |

### 🕵️ Privacy & Anti-Fingerprinting

| Module | Interval | Description |
|---|---|---|
| 🎭 `PrivacyForgeSpoofing` | 60s | Master privacy spoofing orchestrator |
| 🆔 Identity Generation | — | Generates & rotates fake system identities |
| 📊 Software Metadata Spoofing | — | Spoofs installed software telemetry |
| 🎮 Game Telemetry Spoofing | — | Spoofs gaming platform fingerprints |
| 📡 Sensor Spoofing | — | Spoofs hardware sensor readings |
| 📋 Clipboard Spoofing | — | Inserts decoy data into clipboard monitoring |
| 📈 System Metrics Spoofing | — | Spoofs system performance metrics |

---

## 🗺️ Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                        Antivirus.ps1                             │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────────┐ │
│  │ 🔧 Config   │  │ 🔒 Elevation │  │ 🔑 Mutex / Single-Inst │ │
│  └─────────────┘  └──────────────┘  └─────────────────────────┘ │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────────┐│
│  │                  ⏱️ Job Scheduler (Start-ManagedJob)         ││
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐          ││
│  │  │ Module 1│ │ Module 2│ │ Module 3│ │ ...×67  │          ││
│  │  │ (15s)   │ │ (60s)   │ │ (120s)  │ │         │          ││
│  │  └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘          ││
│  └───────┼──────────┼──────────┼──────────┼───────────────────┘│
│          │          │          │          │                      │
│  ┌───────▼──────────▼──────────▼──────────▼───────────────────┐ │
│  │                  🚨 Response Engine                         │ │
│  │  ┌──────────┐ ┌──────────┐ ┌───────────┐ ┌─────────────┐ │ │
│  │  │  Kill    │ │ Quarant. │ │  Log      │ │  MITRE Map  │ │ │
│  │  └──────────┘ └──────────┘ └───────────┘ └─────────────┘ │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │ 🛡️ Protection Layer                                       │  │
│  │  Anti-Termination │ Process Watchdog │ Auto-Restart        │  │
│  └────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

---

## 📁 Directory Structure

```
C:\ProgramData\AntivirusProtection\
├── 📂 Data/
│   ├── 📄 whitelist.json          # Trusted file hashes
│   ├── 📄 db_integrity.hmac       # Database HMAC key
│   ├── 📄 antivirus.pid           # Process ID file
│   └── 📄 CVE-PatcherState.json   # CVE mitigation state
├── 📂 Logs/
│   ├── 📄 antivirus_log.txt       # Main application log
│   ├── 📄 stability_log.txt       # Stability / diagnostics log
│   ├── 📄 ids_detection_*.log     # IDS detection logs
│   ├── 📄 yara_detection_*.log    # YARA scan results
│   ├── 📄 mitre_mapping_*.log     # MITRE ATT&CK mappings
│   └── 📄 ...                     # Per-module daily logs
├── 📂 Quarantine/
│   └── 📄 <sha256>.quarantined    # Quarantined threat files
├── 📂 Reports/
│   └── 📄 <timestamp>.json        # Threat report exports
└── 📂 YaraRules/                  # (Optional) Custom YARA rules
    └── 📄 *.yar
```

---

## 🔄 Job Scheduler

The script uses a custom managed-job scheduler that:

1. 📋 Registers each module with its configured interval
2. ⏱️ Tracks `LastRun` timestamps per module
3. 🔁 Calls `Invoke-ManagedJobsTick` in the main loop
4. ✅ Only fires a module when its interval has elapsed
5. 📊 Reports status: loaded vs. failed modules at startup

```
⚡ 2s   │ GFocus
⚡ 5s   │ WebcamGuardian
⚡ 10s  │ ProcessCreation, ResponseEngine, ElfDLLUnloader
⚡ 15s  │ Hash, LOLBin, ProcessAnomaly, AMSI, CredentialDump, Ransomware, NeuroBehavior, Mobile
🔄 20s  │ AdvancedThreat, Fileless, USB
🔄 30s  │ TokenManip, NetworkAnomaly, Clipboard, ShadowCopy, CodeInjection, DataExfil, ...
🔄 45s  │ KeyloggerDet, NamedPipe, NetworkTraffic, SuspiciousParentChild
🔄 60s  │ BeaconDet, EventLog, Service, KeyScrambler, IDS, HidMacro, LocalProxy, ScriptHost, ...
🔄 90s  │ DLLHijacking, MemoryScanning, MemoryAcquisition
🔄 120s │ WMIPersistence, ScheduledTask, RegistryPersist, Firewall, FileEntropy, YARA, ScriptContent, Startup
🐢 300s │ BrowserExt, Quarantine, PasswordMgmt, PrivacyForge, UnsignedDLL, BCD, Credential, MITRE
🐢 1h   │ CVE-MitigationPatcher, GRulesC2Block
📅 24h  │ ScriptBlockLogging, AsrRules, ProcessAuditing
```

---

## 📊 Threat Intelligence Feeds

The script integrates with multiple external threat intelligence sources:

| Feed | Usage |
|---|---|
| 🔵 [CIRCL hashlookup](https://hashlookup.circl.lu/) | SHA-256 hash reputation lookups |
| 🟢 [Team Cymru MHR](https://www.team-cymru.com/mhr) | Malware hash registry queries |
| 🔴 [MalwareBazaar](https://bazaar.abuse.ch/) | Malware sample database lookups |
| 🟠 [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) | Known Exploited Vulnerabilities feed |
| 🟡 [Emerging Threats](https://rules.emergingthreats.net/) | Compromised IP blocklists |

---

## 🗺️ MITRE ATT&CK Coverage

The `MitreMapping` module automatically tags all detections with their corresponding MITRE ATT&CK technique IDs:

| Technique | ID | Detection Module(s) |
|---|---|---|
| 🎯 User Execution | T1204 | HashDetection, AdvancedThreat, FileEntropy, Honeypot |
| 💉 Process Injection | T1055 | ProcessAnomaly, CodeInjection, ProcessHollowing (T1055.012) |
| 🔑 OS Credential Dumping | T1003 | CredentialDump, MemoryScanning |
| 📜 Command & Scripting | T1059 | Fileless, ProcessCreation, IdsDetection |
| 🔗 DLL Side-Loading | T1574.001 | DLLHijacking |
| 🎫 Access Token Manipulation | T1134 | TokenManipulation |
| ⌨️ Input Capture: Keylogging | T1056.001 | KeyloggerDetection |
| 🔒 Data Encrypted for Impact | T1486 | RansomwareDetection |
| 📡 Application Layer Protocol | T1071 | BeaconDetection |
| 🕳️ Exfiltration Over Alternative Protocol | T1048 | DNSExfiltration, DataExfiltration |
| 👻 Rootkit | T1014 | RootkitDetection |
| 📋 Clipboard Data | T1115 | ClipboardMonitoring |
| 📸 Video Capture | T1125 | WebcamGuardian |
| 💿 Inhibit System Recovery | T1490 | ShadowCopyMonitoring |
| 🔥 Impair Defenses | T1562 | AMSIBypass (T1562.006), EventLog (T1562.006), FirewallRule (T1562.004) |
| 🔀 Remote Services | T1021 | LateralMovementDetection |
| 📦 Exfiltration Over Physical Medium | T1052 | USBMonitoring |
| ⏰ Scheduled Task/Job | T1053.005 | ScheduledTaskDetection |
| 📋 Boot or Logon Autostart | T1547 | RegistryPersistence (T1547.001), WMIPersistence (T1547.003) |
| 🔧 Obtain Capabilities | T1588 | AttackToolsDetection |

---

## ⚡ Performance

| Metric | Value |
|---|---|
| 💾 Memory cap | 500 MB (configurable) |
| 🧵 Concurrency | 67+ modules, single-threaded tick scheduler |
| ⚡ Fastest tick | 2 seconds (GFocus) |
| 🐢 Slowest tick | 86400 seconds / 24h (daily hardening checks) |
| 📦 Script size | ~12,000 lines, single `.ps1` file |
| 🔧 Dependencies | None (PowerShell 5.1+ only) |

---

## 🔧 Troubleshooting

### 🔴 "Script is not running as Administrator"

```powershell
# Right-click PowerShell → "Run as Administrator", then:
.\Antivirus.ps1
```

> The script will auto-elevate, but some environments block UAC prompts.

### 🔴 Module fails to start

Check the stability log:

```powershell
Get-Content "C:\ProgramData\AntivirusProtection\Logs\stability_log.txt" -Tail 50
```

### 🔴 YARA detection not working

Ensure YARA is installed and `.yar` rule files exist:

```
C:\ProgramData\AntivirusProtection\YaraRules\*.yar
   — or —
C:\Program Files\Yara\yara64.exe
```

### 🔴 High CPU usage

Lower the frequency of high-interval modules in `$Script:ManagedJobConfig`:

```powershell
GFocusIntervalSeconds = 10        # Was 2
NeuroBehaviorMonitorIntervalSeconds = 30  # Was 15
ProcessCreationDetectionIntervalSeconds = 30  # Was 10
```

---

## ❓ FAQ

<details>
<summary>🤔 Is this a replacement for commercial antivirus?</summary>

> This is a **defense-in-depth** layer designed to complement (not replace) commercial AV solutions. It excels at **EDR-level behavioral detection** and **custom threat hunting** that many traditional AVs miss.
</details>

<details>
<summary>🤔 Does it conflict with Windows Defender?</summary>

> No. The script coexists with Windows Defender and other AV products. In fact, the `AsrRules` module leverages Defender's Attack Surface Reduction capabilities. The script adds its own process to the exclusion list to prevent false positives.
</details>

<details>
<summary>🤔 Can I run it as a Windows Service?</summary>

> The script includes built-in **auto-restart**, **process watchdog**, and **anti-termination** safeguards that provide service-like resilience. You can also wrap it with tools like [NSSM](https://nssm.cc/) for true service registration.
</details>

<details>
<summary>🤔 How do I whitelist a trusted file?</summary>

> Add the SHA-256 hash to `C:\ProgramData\AntivirusProtection\Data\whitelist.json`:
> ```json
> {
>   "hashes": ["ABC123...", "DEF456..."]
> }
> ```
</details>

<details>
<summary>🤔 What PowerShell version is required?</summary>

> **PowerShell 5.1+** (included with Windows 10/11). PowerShell 7 (pwsh) is also supported.
</details>

---

## 🔗 Companion Tools

| Tool | Description |
|------|--------------|
| **[SysmonFull](../SysmonFull)** | Sysmon-style event logging to Windows Event Log (no MSFT skips). Run alongside for full audit; GEDR consumes via JobSysmonLogIngestion. |
| **[AutorunsEnhanced](../AutorunsEnhanced)** | Autoruns + auto-remove unverified entries + untrusted font check. `-RemoveUnverified` to remediate. |

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. 🍴 **Fork** the repository
2. 🌿 **Create** a feature branch: `git checkout -b feature/amazing-detection`
3. 💻 **Write** your detection module following the `Invoke-<ModuleName>` convention
4. 📝 **Add** interval config to `$Script:ManagedJobConfig`
5. ✅ **Register** the module name in `$moduleNames`
6. 📬 **Submit** a pull request

### 📐 Module Template

```powershell
# --- MyNewDetection ---
function Invoke-MyNewDetection {
    $detections = 0
    try {
        # Your detection logic here
        # ...

        if ($detections -gt 0) {
            Write-AVLog "MyNewDetection: Found $detections issues" "THREAT"
            Write-EventLog -LogName Application -Source "AntivirusEDR" `
                -EntryType Warning -EventId 2099 `
                -Message "MyNewDetection: $detections threats found" `
                -ErrorAction SilentlyContinue
        }
    } catch {
        Write-AVLog "MyNewDetection error: $_" "ERROR"
    }
    return $detections
}
```

---

## 📜 License

This project is open source. See [LICENSE](LICENSE) for details.

---

<div align="center">

### 🛡️ Stay Protected

**Built with ❤️ by [Gorstak](https://github.com/)**

⭐ Star this repo if you find it useful! ⭐

</div>

