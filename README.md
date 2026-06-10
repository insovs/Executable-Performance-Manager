<div align="center">
<img src="https://github.com/user-attachments/assets/c70c178f-d975-4a4f-b19e-33c090ec6f9f" alt="GIF" width="1080">


A lightweight PowerShell/WPF tool that **applies Windows performance tweaks to any executable** in a few clicks.

![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?logo=powershell)
[![Windows](https://img.shields.io/badge/Windows-10%2F11-0078D4?style=flat-square&logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![Discord](https://img.shields.io/badge/Support-Discord-5865F2?logo=discord&logoColor=white)](https://discord.com/invite/fayeECjdtb)
[![Preview](https://img.shields.io/badge/Video-Preview-FF0000?logo=youtube&logoColor=white)](https://youtu.be/q63XYpYXOiQ)

</div>

---

## ✨ Features

| Feature | What it does |
|---|---|
| **CPU Priority** | Forces the process to **High** CPU and I/O priority via IFEO (`PerfOptions`) automatically at launch.<br>• **Result:** Windows prioritizes the executable for CPU time and disk access over background tasks. |
| **QoS Network** | Tags outgoing network packets with **DSCP 46** (Expedited Forwarding) via Windows QoS policy.<br>• **Result:** Reduces ping, delay, spikes and jitter. |
| **Firewall** | Automates the creation of explicit **Inbound and Outbound Allow** rules within the Windows Firewall.<br>• **Result:** Resolves multiplayer issues like rubber-banding, lobby timeouts, and connection drops. |
| **Defender** | Adds the application's root folder directly to **Windows Defender's exclusion list**.<br>• **Result:** Eliminates hard-to-diagnose micro-stutters and sudden frame time spikes. |
| **Fullscreen Optimization** | Disables **Windows Fullscreen Optimizations (FSO)** to restore True Exclusive Fullscreen mode.<br>• **Result:** Significantly reduces input latency and improves overall frame pacing. |
| **GPU Preference** | Forces Windows to always utilize the **discrete GPU** (`GpuPreference=2`) instead of the integrated graphics.<br>• **Result:** Essential for hybrid laptops to ensure maximum framerates and full VRAM availability. |
| **Run As Admin** | Configures the executable to auto request **administrator privileges** at launch via `AppCompatFlags` + IFEO.<br>• **Result:** Prevents applications from failing silently due to restricted permissions. |

---

## 📥 Usage

Download `ExecutablePerformanceManager.ps1`, then **right-click** it → **Run with PowerShell**.

> [!CAUTION]
> If PowerShell scripts are blocked on your system, enable execution first:
> ```powershell
> Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> `RemoteSigned` is sufficient for local scripts — `Unrestricted` is not required.
> Or use **[EnablePowerShellScript](https://github.com/insovs/EnablePowerShellScript)** for a one-click solution.

---

## 🔧 How it works

All changes target either the Windows registry or native PowerShell cmdlets — nothing is patched, injected, or dependent on third-party tools.

| Feature | Registry / API used |
|---|---|
| CPU Priority | `HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\<exe>\PerfOptions` |
| QoS | `HKLM:\SOFTWARE\Policies\Microsoft\Windows\QoS` |
| GPU Preference | `HKCU:\SOFTWARE\Microsoft\DirectX\UserGpuPreferences` |
| Run As Admin | `HKCU:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers` |
| Firewall | `New-NetFirewallRule` cmdlet |
| Defender | `Add-MpPreference -ExclusionPath` cmdlet |
| Fullscreen — system-wide | `HKCU:\System\GameConfigStore` |
| Fullscreen — per-app | `HKCU:\System\GameConfigStore\Children\<GUID>` |

All changes are **non-destructive** and fully reversible from within the app using the **Delete selected** button on each page. No system files are modified.

---

## ⚠️ Notes

- **Run As Admin**: some apps may refuse to launch when this flag is set — remove the rule if that happens.

---

## 🤝 Contributing

Pull requests are welcome. If you find a bug or want to suggest a new optimization module, feel free to open an issue.

---

<div align="center">
<sub>Built with PowerShell + WPF · No external dependencies · 100% native Windows APIs</sub>
</div>
