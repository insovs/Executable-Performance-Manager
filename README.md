<div align="center">
<img width="1343" height="617" alt="Game optimizer tool" src="https://github.com/user-attachments/assets/dfa63664-1e5b-4a08-9131-d3af5e18bc5d" />

![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?logo=powershell)
[![Windows](https://img.shields.io/badge/Windows-10%2F11-0078D4?style=flat-square&logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![Discord](https://img.shields.io/badge/Support-Discord-5865F2?logo=discord&logoColor=white)](https://discord.com/invite/fayeECjdtb)
[![Preview](https://img.shields.io/badge/Video-Preview-FF0000?logo=youtube&logoColor=white)](https://youtu.be/q63XYpYXOiQ)

A lightweight PowerShell/WPF tool that applies Windows performance tweaks to any executable in a few clicks.

</div>

---

<details>
<summary><b>📸 Preview Screenshots</b></summary>
<br>

<table>
<tr>
<td align="center" width="50%">
<img width="531" height="570" alt="Splash Screen" src="https://github.com/user-attachments/assets/82179b9d-c8cd-4fde-852f-8c8e6afb454e" />
<br/><br/>
<sub><b>Splash Screen</b> — Choose a module and get started instantly.</sub>
</td>
<td align="center" width="50%">
<img width="489" height="533" alt="Main Window" src="https://github.com/user-attachments/assets/616b7091-f9f8-4a02-8ec4-111e6332c5a5" />
<br/><br/>
<sub><b>Main Window</b> — Manage your rules, add apps, and track changes in real time.</sub>
</td>
</tr>
</table>

</details>

---

## ✨ Features

| Feature | What it does |
|---|---|
| **CPU Priority** | Elevates the process to **High** CPU and I/O priority via IFEO (`PerfOptions`). Applied automatically at every launch — no manual Task Manager step needed. Windows will consistently favor the executable for CPU time and disk access over background processes. |
| **QoS Network** | Tags outgoing packets with **DSCP 46** (Expedited Forwarding) via Windows QoS policy. Compatible routers process those packets ahead of regular traffic, reducing ping spikes and jitter. Most effective on home networks where the PC is the bottleneck. Note: ISPs typically strip DSCP tags on WAN traffic, so impact is limited to your local network. |
| **GPU Preference** | Forces the executable to run on the **discrete GPU** (`GpuPreference=2`) instead of letting Windows decide. Essential on hybrid laptops (Intel iGPU + NVIDIA/AMD dGPU) where the wrong GPU is silently selected, causing lower framerates and reduced VRAM availability. |
| **Run As Admin** | Makes the executable always request **administrator privileges** at launch via `AppCompatFlags` + IFEO — without needing to right-click every time. Useful for apps that require elevated access to function correctly or that fail silently without it. |
| **Firewall** | Creates explicit **Inbound + Outbound Allow** rules in Windows Firewall for the executable. Prevents Windows from silently blocking or throttling connections, which can cause rubber-banding, lobby timeouts, or failed multiplayer sessions. |
| **Defender** | Adds the app's folder to **Windows Defender's exclusion list**, stopping real-time scans from interrupting the process mid-session. Eliminates a common source of micro-stutters and frame time spikes that are difficult to diagnose otherwise. |
| **Fullscreen Optimization** | Disables **Windows Fullscreen Optimizations (FSO)** globally or per executable. Restores true exclusive fullscreen mode, reducing input latency and improving frame pacing by giving the app direct control over display output — bypassing the DWM compositor. |

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
