## ▶️ YouTube Video Tutorial
- [Step-by-Step Video Tutorial]

---

# 🖥️ How To Set Up Ubuntu Server Minimized

## 🛠️ Things You'll Need

- 🖥️ **Secondary Computer** — the machine you'll be using as your Home Server
- 🔌 **Ethernet Cable** — plugged into your router or switch
- 💾 **USB Flash Drive** (8 GB or larger) — ⚠️ All data on the drive will be erased during installation. Back up any important files first!
- 📥 **Ubuntu Server ISO File** — download the latest version from the official site: [ubuntu.com/download/server](https://ubuntu.com/download/server#release-notes)
- 🔧 **A Bootable USB Tool** — not sure which one to use? Check out [The Best Tools for Creating a Bootable USB Drive](https://github.com/sokorid/Home-Lab-Stuff/blob/d1247a35f9c2222f91a1ff189f7767ebf7f69f07/%F0%9F%92%BE%20Best%20Tools%20to%20Create%20a%20Bootable%20USB%20Drive.md)

---

## 🚀 Boot the Installer

1. Plug the USB drive into the target machine and power it on.
2. Most computers will automatically boot from USB — if yours doesn't, you'll need to open the boot menu.
3. When the computer starts, look for an on-screen message telling you which key to press for settings or the boot menu. This is usually **Escape, F2, F10, or F12** depending on your manufacturer.
4. Restart, hold that key, and select your USB drive from the boot menu.
5. You should see the Ubuntu boot message followed by the **"Welcome"** screen.

---

## 📋 Install Guide for Ubuntu Server

The three most important things to watch out for during installation:

- ⚠️ **Disable LVM** — make sure this is turned off unless you specifically need it.
- ✅ **Enable SSH** — this is essential for managing your server remotely.

---

## ✅ Once It's Finished Installing

Once the installer finishes, the server will reboot. Remove the USB drive and log in with the credentials you created during setup.

---

## 🖥️ Time To Start Setting Up The Server

Log into your account, then let's make sure the system is up to date before we do anything else.

### 🚀 Option 1: Automated Setup
> [!WARNING]
> ⚠️ **Security Tip:** Always inspect third-party scripts before running them. You can view the source [here](https://raw.githubusercontent.com/sokorid/Tools-And-Scripts/refs/heads/main/Linux/Ubuntu/Scripts/Auto_Setup_Ubuntu_Server.sh).
>
> 💡 **Want to skip the manual setup?** To simplify the installation, you can use my custom automation script. It handles the bulk of the configuration for you, requiring only a few manual 'yes/no' confirmations to complete the setup.
> ```bash
> sudo bash -c "$(wget -qLO - https://raw.githubusercontent.com/sokorid/Tools-And-Scripts/refs/heads/main/Linux/Ubuntu/Scripts/Auto_Setup_Ubuntu_Server.sh)"
> ```
> Otherwise, continue below both options are fully viable.

### 🛠️ Option 2: Manual Update

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 📝 Installing a Text Editor
⚠️ Before we go any further, we need to make sure a text editor is installed. This guide uses text editors to open and modify config files directly in the terminal.
```bash
sudo apt install -y nano neovim
```
### What Are These?
 
**Nano** — the beginner-friendly option. It's simple, straightforward, and shows you the keybinds at the bottom of the screen so you never have to guess. If you're new to the terminal, nano is what you want to use.
 
**Neovim** — a powerful, highly customisable editor built for speed and efficiency. It has a steeper learning curve since it uses keyboard-based navigation instead of a traditional point-and-click approach, but once you get the hang of it, it's extremely fast to work in. This is the more advanced option.
 
> 💡 **Not sure which to use?** Stick with `nano` it's what this guide uses throughout.

---

## 🌐 Setting Up a Static IP Address

Before we make any changes, we need to gather a few pieces of information.

### Step 1 — Find Your Default Gateway

```bash
ip r
```

Look for the line that starts with **"default via"** — the IP address right after that is your gateway. Write it down.

### Step 2 — Find Your Network Interface & Current IP

```bash
ip -4 -br addr
```

- On the **left** you'll see your network interface name — it should look something like `enp0s0`
- On the **right** you'll see your current IPv4 address

Write both of those down before moving on.

---

### Step 3 — Create the Static IP Config File

```bash
sudo nano /etc/netplan/01-network-manager-all.yaml
```

Paste the following into the file:

```yaml
network:
  version: 2
  ethernets:
    enp0s0:
      dhcp4: false
      addresses:
        - IP/24
      routes:
        - to: default
          via: IP
```

You need to change three things:

- `enp0s0` → your network interface name
- `addresses: IP/24` → the static IP you want, e.g. `- 192.168.1.100/24`
- `via: IP` → your gateway IP from Step 1

**Nano Keybinds:**

| Action | Keybind |
|--------|---------|
| Save | `Ctrl + O` |
| Confirm | `Enter` |
| Close | `Ctrl + X` |

---

### Step 4 — Apply the Changes

> 💡 **Optional — Recommended if on a remote server:**
> If you're worried about losing your connection due to a typo, use this instead:
> ```bash
> sudo netplan try
> ```
> This applies the config and waits for you to press `Enter` to confirm. If you don't confirm within 120 seconds — for example, because you accidentally locked yourself out — it automatically reverts to the previous working settings.

When you're ready, apply the changes:

```bash
sudo netplan apply
```

---

### Step 5 — Verify the IP

```bash
ip -4 -br addr
```

If everything went correctly you should see the static IP address you set up.

---

## 🔐 SSH Into Your Server

Once that's done, we can switch over to SSH on your main computer — so you can easily follow along or copy and paste commands.

**Skip to your platform:**

- 🪟 [Windows](#-windows)
- 🍎 [macOS](#-macos)
- 🐧 [Linux](#-linux)

---

### 🪟 Windows

You can SSH in by opening **CMD** and running:

```bash
ssh UserName@IPAddress
```

**Want an easier way?** There's a batch file in the repository you can download or copy — it makes SSHing in much quicker:

[📄 Download Open SSH.bat](https://github.com/sokorid/Home-Lab-Stuff/blob/main/How-To-Setup-Casaos-Through-Ubuntu-Server-For-Beginners/Open%20SSH.bat)

Just replace `UserName` with the username you created during install, and `IPAddress` with your server's static IP.

Alternatively, create a new text file, paste the following in, then save it as `.bat` instead of `.txt`:

```bat
@echo off
cls
title SSH Client
color f
cls
ssh UserName@IPAddress
```

---

### 🍎 macOS

Open **Terminal** and run:

```bash
ssh UserName@IPAddress
```

---

### 🐧 Linux

Open your **terminal** and run:

```bash
ssh UserName@IPAddress
```

---

> ✅ **SSH Open:** If everything went correctly, you'll be asked to confirm a fingerprint — just type `yes` and hit Enter.

---

## 💡 Quick Tips (Optional)

Everything below this point is optional, but handy to know.

### Switch to the Root User

If you need root access without logging out:

```bash
sudo -i
```

### Switch Back to Your Account

Replace `UserName` with your actual username:

```bash
su UserName
```

### Return to Your Home Directory

```bash
cd
```

### Remove SSH Welcome Message

This is not necessary unless you don't want to see the system info every time you SSH into your server.

```bash
sudo nano /etc/pam.d/sshd
```

Find the following two lines:

```text
session    optional     pam_motd.so  motd=/run/motd.dynamic
session    optional     pam_motd.so noupdate
```

Comment them out by placing `#` in front of each line, then save and close.

**Nano Keybinds:**

| Action | Keybind |
|--------|---------|
| Save | `Ctrl + O` |
| Confirm | `Enter` |
| Close | `Ctrl + X` |

Now restart SSH to apply the change:

```bash
sudo systemctl restart ssh
```

---

## 🔄 Automatic Updates

Keeping your server up to date is important for security. Let's install and enable automatic updates.

### Install & Enable

First, make sure everything is up to date, then install the package and run the setup wizard:

```bash
sudo apt update
```

```bash
sudo apt install unattended-upgrades
```

```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

> When the purple screen appears, select **Yes** to enable automatic updates.

---

### ⚙️ Configure for Security Updates Only (Optional)

By default, `unattended-upgrades` is already set to prioritize security patches. But if you want to make sure it's strictly limited to security updates only — and not pulling in general system updates — you can verify and tighten those settings manually.

Open the config file:

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

**Nano Keybinds:**

| Action | Keybind |
|--------|---------|
| Save | `Ctrl + O` |
| Confirm | `Enter` |
| Close | `Ctrl + X` |

Look for the `Allowed-Origins` section. Make sure the security line is active and everything else is commented out with `//`:

```text
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
//  "${distro_id}:${distro_codename}-updates";
```

**Not sure what your `distro_id` or `distro_codename` are?** Run this:

```bash
cat /etc/os-release
```

Look for these two lines — the value after the `=` is what you need:

```text
VERSION_CODENAME=distro_codename
ID=distro_id
```

---

### 🔁 Handle Automatic Reboots (Optional)

Security patches — especially kernel updates — often require a reboot to fully take effect. You can configure Ubuntu to reboot itself automatically at a time you're not using the machine.

> ⚠️ **Heads up:** If other services aren't configured to start correctly on boot, an automatic reboot can cause unexpected downtime. Only enable this if you're comfortable with how your services are set up.

In the same config file:

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```
**Nano Keybinds:**

| Action | Keybind |
|--------|---------|
| Save | `Ctrl + O` |
| Confirm | `Enter` |
| Close | `Ctrl + X` |

Find and update these lines:

```text
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "04:00";
```

Set the time to whenever you're least likely to be using the server.

---

### ✅ Test It

Let's do a dry run to make sure everything is working correctly. This simulates an update without actually changing anything:

```bash
sudo unattended-upgrade --dry-run --debug
```

---

### 📋 Monitoring Logs

Want to check what your server updated while you were away? Here's where to look:

**Summary log:**
```bash
cat /var/log/unattended-upgrades/unattended-upgrades.log
```

**Detailed dpkg log:**
```bash
cat /var/log/unattended-upgrades/unattended-upgrades-dpkg.log
```
