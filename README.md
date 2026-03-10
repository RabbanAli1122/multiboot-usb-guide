# Ultimate Multi-Boot USB — Complete Build Guide

A fully verified, multi-OS bootable USB drive built using [Ventoy](https://www.ventoy.net). This guide documents everything — the ISOs collected, their verified SHA256 checksums, download sources, issues encountered, and the exact process used to build it. Anyone can follow this guide to reproduce the exact same USB from scratch.

---

## Table of Contents

- [Background](#background)
- [Download Manager Recommendation](#download-manager-recommendation)
- [Critical Issues Encountered](#critical-issues-encountered)
- [What is Ventoy](#what-is-ventoy)
- [OS Collection](#os-collection)
- [Download Links and Verified SHA256 Checksums](#download-links-and-verified-sha256-checksums)
- [How to Build This USB](#how-to-build-this-usb)
- [How to Verify Checksums](#how-to-verify-checksums)
- [What Does NOT Work With Ventoy](#what-does-not-work-with-ventoy)
- [Notes on Specific OS Behaviour](#notes-on-specific-os-behaviour)
- [USB Storage Used](#usb-storage-used)
- [License](#license)

---

## Background

This project started with a simple goal: build one USB drive that can boot into any operating system — Windows (multiple versions), Linux distros, a privacy OS, and PC repair utilities — on any PC.

The process turned out to be more involved than expected. This README documents every decision, every issue, and every solution so that anyone attempting the same thing does not have to figure it out the hard way.

---

## Download Manager Recommendation

Before downloading any ISOs, strongly recommended to use a dedicated download manager. Large ISO files downloaded through a browser are more prone to silent corruption mid-download, especially on slow or unstable connections. A download manager handles retries, resumes interrupted downloads, and improves reliability significantly.

For non-torrent downloads:
- macOS: [Download Shuttle](https://apps.apple.com/app/download-shuttle-fast-downloader/id421474420) (free, App Store)
- Windows: [Free Download Manager](https://www.freedownloadmanager.org/) (free)

For torrent downloads (Fedora, Kali, Windows 7):
- [qBittorrent](https://www.qbittorrent.org/) (free, cross-platform)

Note: qBittorrent pre-allocates the full file size on disk immediately, so a file will appear full-size in Finder or File Explorer even at 10% progress. Trust the progress percentage inside qBittorrent, not the file size shown in the file manager.

---

## Critical Issues Encountered

### 1. Ventoy does NOT have a macOS version

The initial plan was to set up everything on a MacBook Air M1. After downloading Ventoy's .tar.gz file and trying to run it on macOS, it became clear that Ventoy only exists for Windows and Linux. The .tar.gz package contains Linux ELF binaries and relies on Linux-specific tools (losetup, /proc/mounts) that do not exist on macOS.

Note: A Linux server or desktop machine would also work perfectly fine for installing Ventoy via its terminal installer. If you have access to any Linux machine, use that instead of needing a Windows PC.

Solution used: A Windows laptop was used to install Ventoy onto the USB. The ISOs were downloaded and verified on the Mac first, then the USB was taken to the Windows machine to install Ventoy.

### 2. macOS cannot be added to Ventoy

macOS installer files are .app format and require HFS+ formatted drives — completely incompatible with Ventoy. macOS would need its own dedicated USB created via Apple's createinstallmedia command.

### 3. FAT32 file size limit

The USB was initially formatted as FAT32, which has a 4GB per-file limit. Several ISOs (Windows 11 at 7.74GB, Ubuntu Desktop at 6.66GB, etc.) exceeded this. Solution: Reformatted the USB to exFAT before copying any files.

### 4. iCloud Drive sync corruption risk

On macOS, iCloud Drive was enabled on the Desktop. Storing large ISO files in iCloud-synced folders risks corruption. Solution: Turned off iCloud Drive syncing before downloading any ISOs.

### 5. Corrupted Windows 10 ISO

An initial Windows 10 download was corrupted — the SHA256 checksum did not match. Re-downloaded and verified before proceeding. This is exactly why checksum verification matters.

### 6. Old unverified Windows 7 ISOs

The first Windows 7 ISOs found were from an unofficial Dell All-in-One upload on Archive.org with no verifiable checksums. These were deleted and replaced with the official Microsoft SP1 originals sourced from Archive.org's verified mirror.

---

## What is Ventoy

[Ventoy](https://www.ventoy.net) is an open-source tool that turns a USB drive into a multi-boot drive. Instead of flashing one OS at a time, you simply:

1. Install Ventoy onto the USB once (this formats the USB)
2. Copy any number of ISO files onto the USB like normal files
3. Boot from the USB on any PC — Ventoy shows a menu listing all ISOs
4. Pick whichever OS you want to boot

No re-flashing needed when adding new ISOs — just copy and boot.

Most ISO files follow a standard boot specification (called the El Torito standard) that Ventoy can chain-boot universally. Ventoy is not limited — it works with virtually any standard ISO. The rare exceptions are ISOs that are intentionally non-standard by design for their own reasons.

---

## OS Collection

### Understanding the Table Columns

| Term | What it means |
|------|---------------|
| Boots Live | Run the OS directly from USB without installing. Nothing is saved after shutdown. |
| Can Install | Includes an installer to permanently install the OS onto the PC's hard drive. |

Some ISOs do both — boot live AND offer to install from within the live environment.

### Full OS List

| # | Name | Purpose | Boots Live | Can Install |
|---|------|---------|------------|-------------|
| 1 | Windows 11 x64 | Latest Windows, modern hardware | No | Yes |
| 2 | Windows 10 x64 | Widest hardware compatibility | No | Yes |
| 3 | Windows 10 x32 | Windows 10 for 32-bit hardware | No | Yes |
| 4 | Windows 7 x64 | Legacy, pre-2015 hardware | No | Yes |
| 5 | Windows 7 x32 | Legacy 32-bit, very old hardware | No | Yes |
| 6 | Ubuntu Desktop 24.04.4 | Most popular Linux, beginner friendly | Yes | Yes |
| 7 | Ubuntu Server 24.04.3 | Linux for server administration | Yes | Yes |
| 8 | Linux Mint 22.3 | Most Windows-like Linux | Yes | Yes |
| 9 | Fedora Workstation 43 | Cutting edge Linux, clean GNOME | Yes | Yes |
| 10 | Kali Linux 2025.4 Live | Cybersecurity and ethical hacking | Yes | Yes |
| 11 | HBCD PE x64 | PC repair and recovery toolkit | Yes | No |
| 12 | GParted Live | Partition management for any drive | Yes | No |
| 13 | Puppy Linux | Ultralight, runs entirely in RAM | Yes | Yes |

---

## Download Links and Verified SHA256 Checksums

All checksums were verified at three stages:
1. After download on Mac
2. After copying to Windows laptop
3. After copying to Ventoy USB

Every file matched at all three stages with zero corruption.

### Windows

**Windows 11 x64** — [Download](https://www.microsoft.com/en-us/software-download/windows11)
```
d141f6030fed50f75e2b03e1eb2e53646c4b21e5386047cb860af5223f102a32
```

**Windows 10 x64** — [Download](https://www.microsoft.com/en-us/software-download/windows10ISO)
```
a6f470ca6d331eb353b815c043e327a347f594f37ff525f17764738fe812852e
```

**Windows 10 x32** — [Download](https://www.microsoft.com/en-us/software-download/windows10ISO)
```
ac0b7045b6c3a72a4d46daab0944e109a55d9ede3a11b775fdb57c2dd3fca2ef
```

**Windows 7 Pro SP1 x64** — [Download](https://archive.org/details/win-7-pro-sp1-english)
```
3DAE1A531B90FA72E59B4A86B20216188D398C8C070DA4A5C5A44FE08B1B6E55
```

**Windows 7 Pro SP1 x32** — [Download](https://archive.org/details/win-7-pro-sp1-english)
```
FD4CDF56E0087AC4A76D6858046F3EE50977D47917CA96366322E271DDD4838E
```

Windows 7 note: Microsoft no longer officially distributes Windows 7. These ISOs are the original Microsoft SP1 English builds sourced from Archive.org. They are not from an official Microsoft download page. Always verify the checksum before use.

### Linux

**Ubuntu Desktop 24.04.4** — [Download](https://ubuntu.com/download/desktop)
```
3a4c9877b483ab46d7c3fbe165a0db275e1ae3cfe56a5657e5a47c2f99a99d1e
```

**Ubuntu Server 24.04.3** — [Download](https://ubuntu.com/download/server)
```
c3514bf0056180d09376462a7a1b4f213c1d6e8ea67fae5c25099c6fd3d8274b
```

**Linux Mint 22.3 Cinnamon** — [Download](https://linuxmint.com/download.php)
```
a081ab202cfda17f6924128dbd2de8b63518ac0531bcfe3f1a1b88097c459bd4
```

**Fedora Workstation 43** — [Download](https://fedoraproject.org/workstation/download)
```
2a4a16c009244eb5ab2198700eb04103793b62407e8596f30a3e0cc8ac294d77
```

**Kali Linux 2025.4 Live** — [Download](https://www.kali.org/get-kali/)
```
21e87900f8464b8ba99ed0b4161388f896fc13cf9af976c0bfd692ffe62931c2
```

**Puppy Linux** — [Download](https://puppylinux-woof-ce.github.io/download.html)
```
fe8b9c38e2dac5a9cfd1ee9c9921b02429bf004f14eaea7be8407ef2e77a9b3b
```

Puppy Linux note: Puppy Linux is a community project and does not publish official SHA256 checksums on their download page. The hash listed above is what was produced by this specific download and is consistent across all three verification stages, but cannot be compared against an official published value.

### Utilities

**Hiren's Boot CD PE x64** — [Download](https://www.hirensbootcd.org/download/)
```
8c4c670c9c84d6c4b5a9c32e0aa5a55d8c23de851d259207d54679ea774c2498
```

**GParted Live** — [Download](https://gparted.org/download.php)
```
167a114b25b0cabb8ca921413b777d2693511bc18bc1625ae310b84597b79413
```

---

## How to Build This USB

### What you need

- A USB drive of sufficient size depending on how many ISOs you want to include (see the storage table at the bottom to calculate)
- A Windows or Linux PC to install Ventoy (Ventoy has no macOS version)
- The ISOs downloaded and verified

### Step 1 — Download Ventoy

Download the latest Windows version from [ventoy.net](https://www.ventoy.net/en/download.html). Extract the zip file.

### Step 2 — Install Ventoy onto the USB

1. Plug in your USB drive
2. Run Ventoy2Disk.exe as Administrator
3. Select your USB drive from the dropdown — double check the drive letter carefully
4. Click Install
5. Confirm the warning that it will erase the USB
6. Ventoy will format and install itself onto the USB

Warning: This will wipe everything on the USB. Back up any existing files first.

### Step 3 — Copy ISOs onto the USB

After Ventoy is installed, the USB will appear as a normal drive. Simply copy all your ISO files onto it like you would any regular file. No special process needed.

### Step 4 — Boot from the USB

1. Plug the USB into the target PC
2. Enter the boot menu (usually F12, F11, ESC, or DEL at startup — varies by PC brand)
3. Select the USB drive
4. Ventoy's menu will appear listing all ISOs
5. Select the OS you want and press Enter

---

## How to Verify Checksums

Always verify checksums after downloading and after copying to USB. A mismatch means the file is corrupted and must be re-downloaded.

### On macOS (Terminal)

```bash
shasum -a 256 /path/to/file.iso
```

To verify all files on the USB at once, paste all commands in one go:

```bash
shasum -a 256 /Volumes/OS\ USB/Fedora-Workstation-Live-43-1.6.x86_64.iso
shasum -a 256 /Volumes/OS\ USB/gparted.iso
shasum -a 256 /Volumes/OS\ USB/HBCD_PE_x64\(repair\ toolkit\).iso
shasum -a 256 /Volumes/OS\ USB/kali-linux-2025.4-live-amd64.iso
shasum -a 256 /Volumes/OS\ USB/linuxmint-22.3-cinnamon-64bit.iso
shasum -a 256 /Volumes/OS\ USB/puppylinux.iso
shasum -a 256 /Volumes/OS\ USB/ubuntu-24.04.3-live-server-amd64.iso
shasum -a 256 /Volumes/OS\ USB/ubuntu-24.04.4-desktop-amd64.iso
shasum -a 256 /Volumes/OS\ USB/Win7_Pro_SP1_English_x32.iso
shasum -a 256 /Volumes/OS\ USB/Win7_Pro_SP1_English_x64.iso
shasum -a 256 /Volumes/OS\ USB/Win10_22H2_English_x32v1.iso
shasum -a 256 /Volumes/OS\ USB/Win10_22H2_English_x64v1.iso
shasum -a 256 /Volumes/OS\ USB/Win11_25H2_English_x64.iso
```

### On Windows (Command Prompt)

Replace G: with your actual USB drive letter:

```cmd
certutil -hashfile "G:\Fedora-Workstation-Live-43-1.6.x86_64.iso" SHA256
certutil -hashfile "G:\gparted.iso" SHA256
certutil -hashfile "G:\HBCD_PE_x64(repair toolkit).iso" SHA256
certutil -hashfile "G:\kali-linux-2025.4-live-amd64.iso" SHA256
certutil -hashfile "G:\linuxmint-22.3-cinnamon-64bit.iso" SHA256
certutil -hashfile "G:\puppylinux.iso" SHA256
certutil -hashfile "G:\ubuntu-24.04.3-live-server-amd64.iso" SHA256
certutil -hashfile "G:\ubuntu-24.04.4-desktop-amd64.iso" SHA256
certutil -hashfile "G:\Win7_Pro_SP1_English_x32.iso" SHA256
certutil -hashfile "G:\Win7_Pro_SP1_English_x64.iso" SHA256
certutil -hashfile "G:\Win10_22H2_English_x32v1.iso" SHA256
certutil -hashfile "G:\Win10_22H2_English_x64v1.iso" SHA256
certutil -hashfile "G:\Win11_25H2_English_x64.iso" SHA256
```

Compare every output against the checksums listed in this README. They must match exactly, character for character.

---

## What Does NOT Work With Ventoy

| OS | Reason |
|----|--------|
| macOS | Uses .app format, requires HFS+ drive — incompatible with Ventoy |
| Chrome OS Flex | Uses a flash-only disk image format, not a standard bootable ISO. Unreliable with Ventoy across hardware |
| Hackintosh | Running macOS on non-Apple hardware via bootloader patching — considered for this build but requires custom per-hardware config and is being phased out as Apple moves away from Intel |

---

## Notes on Specific OS Behaviour

- Linux ISOs (Ubuntu, Mint, Fedora, Kali): Boot menu offers both "Try live" and "Install" — one ISO does both
- Windows ISOs: Installer only — no live mode
- HBCD PE and GParted: Always live, never installs — repair tools only
- Kali Linux: The Live version is used here — it also includes an install option from within the live environment
- Puppy Linux: Uniquely runs entirely in RAM after booting — extremely fast and works on very old hardware with as little as 256MB RAM

---

## USB Storage Used

| File | Size |
|------|------|
| Windows 11 x64 | 7.74 GB |
| Windows 10 x64 | 6.14 GB |
| Windows 10 x32 | 4.31 GB |
| Windows 7 x64 | 3.32 GB |
| Windows 7 x32 | 2.56 GB |
| Ubuntu Desktop 24.04.4 | 6.66 GB |
| Ubuntu Server 24.04.3 | 3.3 GB |
| Linux Mint 22.3 | 3.09 GB |
| Fedora Workstation 43 | 2.74 GB |
| Kali Linux 2025.4 Live | 5.33 GB |
| HBCD PE x64 | 3.29 GB |
| GParted Live | 635 MB |
| Puppy Linux | 1.33 GB |
| Total | ~52 GB |

The required USB size depends entirely on which ISOs you choose to include. Add up the sizes of your chosen ISOs from the table above and get a USB with some headroom above that total.

---

## License

This repository documents download sources and checksums for publicly available operating systems. All operating systems listed remain the property of their respective owners and are subject to their own licenses. This project does not distribute any ISO files.
