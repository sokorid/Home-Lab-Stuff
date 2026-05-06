# 💾 Bootable USB Guide

![OS](https://img.shields.io/badge/OS-Windows%20%7C%20macOS%20%7C%20Linux-0078D4?style=for-the-badge&logo=windows&logoColor=white)
![Status](https://img.shields.io/badge/Status-Maintained-success?style=for-the-badge)
![Type](https://img.shields.io/badge/Category-Tools-FFD700?style=for-the-badge)

> [!CAUTION]
> **Data Loss Warning:** Using these tools will completely wipe the target USB drive. Always double-check your drive selection before flashing to avoid losing data on the wrong disk.

---

## 📋 Tool Overview

| Tool | Best For | Platform |
|--------|-------------|----------|
| ⚙️ Rufus | Fast, advanced Windows imaging & TPM bypass | 🪟 Windows |
| 🐙 Ventoy | Storing multiple ISOs on one drive (Drag & Drop) | 🪟 🍎 🐧 All |
| 🐳 balenaEtcher | Simple, foolproof flashing for beginners | 🪟 🍎 🐧 All |
| 🍓 Raspberry Pi Imager | SD cards and Pi-specific OS versions | 🪟 🍎 🐧 All |

---

## 🛠️ Flashing Tools

---

<details>
<summary>⚙️ Rufus (Windows Only)</summary>

<br>

`Rufus` is the gold standard for creating bootable Windows installers and Live Linux drives on Windows systems.

| Feature | Details |
|--------|---------|
| ⚡ High Speed | Significantly faster than most competitors at writing ISOs |
| 🔓 Win 11 Bypass | Can automatically remove TPM 2.0 and Secure Boot requirements |
| 🛠️ Advanced Formatting | Full control over partition schemes (GPT/MBR) and Target system (UEFI/BIOS) |
| 💾 Windows To Go | Option to install Windows directly onto the USB drive to run as a Live OS |

| Download | Link |
| :--- | :--- |
| 🌐 **Official Website** | [rufus.ie](https://rufus.ie/en/#download) |
| 🐙 **GitHub Releases** | [pbatard/rufus](https://github.com/pbatard/rufus/releases) |

**[⬆ Back to Tool Overview](#-tool-overview)**

---
</details>

<details>
<summary>🐙 Ventoy (Highly Recommended)</summary>

<br>

`Ventoy` is a "Swiss Army Knife" tool. You install it once on the USB, and then you just copy-paste ISO files like a regular flash drive.

| Feature | Details |
|--------|---------|
| 📂 Multi-Boot | Store 10+ different ISOs on one drive and choose at boot |
| 🖇️ Drag & Drop | No need to re-format the drive to change the OS; just delete/add files |
| 🔄 Persistence | Supports persistence folders so your Linux changes save between reboots |
| 💻 Cross-Platform | Tools available to install Ventoy from Windows, Linux, and macOS |

| ⚠️ **Secure Boot Note** |
| :--- |
| **BIOS Configuration:** You may need to enter your BIOS/UEFI settings and **Disable Secure Boot** to allow Ventoy to boot correctly on some machines. |

| Download | Link |
| :--- | :--- |
| 🌐 **Official Website** | [ventoy.net](https://www.ventoy.net/en/download.html) |
| 📦 **SourceForge** | [Ventoy Downloads](https://sourceforge.net/projects/ventoy/files/) |

**[⬆ Back to Tool Overview](#-tool-overview)**

---
</details>

<details>
<summary>🐳 balenaEtcher (best for beginners)</summary>

<br>

`balenaEtcher` focuses on being "flash and forget." It is a 3-step tool designed to prevent accidental drive wiping.

| Feature | Details |
|--------|---------|
| 🛡️ Drive Validation | Verifies the written data to ensure the USB isn't corrupted |
| 🚫 Accident Protection | Hidden system drives by default to prevent wiping your Hard Drive |
| 🍎 Mac Specialist | Often the most reliable way to create bootable drives on macOS |

| Download | Link |
| :--- | :--- |
| 🌐 **Official Website** | [etcher.balena.io](https://etcher.balena.io/#download-etcher) |
| 🐙 **GitHub Releases** | [balena-io/etcher](https://github.com/balena-io/etcher/releases) |

**[⬆ Back to Tool Overview](#-tool-overview)**

---
</details>

<details>
<summary>🍓 Raspberry Pi Imager</summary>

<br>

`Raspberry Pi Imager` is a specialized utility that downloads the OS for you before flashing.

| Feature | Details |
|--------|---------|
| 📥 Cloud Download | Pick an OS from a list and it downloads the latest version automatically |
| ⌨️ Pre-Config | Set up SSH, Wi-Fi, and Usernames *before* you even boot the drive |
| 📦 Repo Install | Can be installed directly via terminal on Ubuntu/Debian systems |

**Install via Terminal (Ubuntu/Debian):**
```bash
sudo apt update && sudo apt install rpi-imager
