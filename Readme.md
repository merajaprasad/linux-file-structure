# Linux Root (`/`) Directory Structure – Complete Guide

This document explains the **Linux filesystem structure** based on the **Filesystem Hierarchy Standard (FHS)**. It is written in a **simple, clear, and professional way** so anyone—from beginners to DevOps engineers—can understand and revise it easily.

---

## What is the Root (`/`) Directory?

The root directory `/` is the **top-level directory** in Linux. Every file, directory, device, and process exists **under `/`**. There is no concept of drive letters (like C:, D:) as in Windows.

---

## Directory-by-Directory Explanation

---

## `/bin`
**Type:** Essential binaries  
**Purpose:** Contains basic user commands

### What it contains
- `ls`, `cp`, `mv`, `cat`, `bash`, `grep`

### Why it exists
- Required for system boot and recovery
- Available even if `/usr` is not mounted
- Used by **all users**

---

## `/boot`
**Type:** Boot files  
**Purpose:** Contains files required to start Linux

### What it contains
- Linux kernel (`vmlinuz`)
- Initial RAM disk (`initrd`, `initramfs`)
- Bootloader files (GRUB)

### Why it exists
- System **cannot boot** without this directory
- Loaded by BIOS/UEFI

---

## `/dev`
**Type:** Device files  
**Purpose:** Interface to hardware devices

### What it contains
- Disks: `/dev/sda`, `/dev/nvme0n1`
- USB devices, terminals, pseudo-devices

### Why it exists
- Linux treats hardware as files
- Enables programs to communicate with hardware

---

## `/etc`
**Type:** Configuration files  
**Purpose:** Stores system-wide configuration

### What it contains
- System configs: `fstab`, `hosts`, `passwd`
- Service configs: `nginx`, `ssh`, `mysql`

### Why it exists
- Centralized configuration
- Human-readable text files
- No binaries stored here

---

## `/home`
**Type:** User directories  
**Purpose:** Stores personal files for users

### What it contains
- `/home/username`

### Why it exists
- Separates user data from system files
- Easy backups and migrations
- Safer multi-user environment

---

## `/lib` and `/lib64`
**Type:** Shared libraries  
**Purpose:** Essential libraries for system commands

### What it contains
- `.so` shared libraries

### Why it exists
- Required by `/bin` and `/sbin` binaries
- `/lib64` exists for 64-bit systems

---

## `/lost+found`
**Type:** Recovery directory  
**Purpose:** Stores recovered files after filesystem issues

### What it contains
- Orphaned files recovered by `fsck`

### Why it exists
- Helps recover data after crashes or corruption
- Present on ext-based filesystems

---

## `/media`
**Type:** Mount point  
**Purpose:** Auto-mounted removable media

### What it contains
- USB drives, DVDs, external disks

### Why it exists
- Used by desktop environments
- User-friendly mounting location

---

## `/mnt`
**Type:** Mount point  
**Purpose:** Temporary manual mounts

### What it contains
- Admin-created mount directories

### Why it exists
- Testing disks and mounts
- Temporary usage

---

## `/opt`
**Type:** Optional software  
**Purpose:** Third-party applications

### What it contains
- Vendor software like `/opt/google`, `/opt/oracle`

### Why it exists
- Keeps external software isolated
- Avoids clutter in `/usr`

---

## `/proc`
**Type:** Virtual filesystem  
**Purpose:** Process and kernel information

### What it contains
- `/proc/cpuinfo`
- `/proc/meminfo`
- `/proc/<PID>`

### Why it exists
- Provides real-time system data
- Does not consume disk space
- Used by monitoring tools

---

## `/root`
**Type:** Root user home  
**Purpose:** Home directory for root user

### What it contains
- Root’s personal scripts and files

### Why it exists
- Root needs a home even if `/home` is unavailable
- Useful during recovery mode

---

## `/run`
**Type:** Runtime data  
**Purpose:** Stores temporary runtime information

### What it contains
- PID files
- Sockets
- Service runtime state

### Why it exists
- Cleared on reboot
- Stored in memory (tmpfs)
- Replaces `/var/run`

---

## `/sbin`
**Type:** System binaries  
**Purpose:** System administration commands

### What it contains
- `mount`, `reboot`, `iptables`, `fsck`

### Why it exists
- Used mainly by administrators
- Required for system maintenance

---

## `/sbin.usr-is-merged`
**Type:** Compatibility directory  
**Purpose:** Supports `/usr` merge

### Why it exists
- Backward compatibility
- Prevents breaking older scripts

---

## `/snap`
**Type:** Package system  
**Purpose:** Stores snap packages

### What it contains
- Snap-installed applications

### Why it exists
- Dependency isolation
- Self-contained apps

---

## `/srv`
**Type:** Service data  
**Purpose:** Data for system services

### What it contains
- Web, FTP, or application data

### Why it exists
- Clean separation for server content
- FHS-compliant but rarely used

---

## `/sys`
**Type:** Virtual filesystem  
**Purpose:** Kernel and hardware interface

### What it contains
- Hardware and kernel parameters

### Why it exists
- Used by udev and system tools
- Allows hardware configuration

---

## `/tmp`
**Type:** Temporary files  
**Purpose:** Short-lived files

### What it contains
- Application temporary files

### Why it exists
- Writable by all users
- Cleared on reboot

---

## `/usr`
**Type:** User software  
**Purpose:** Installed applications and libraries

### What it contains
- `/usr/bin`, `/usr/lib`, `/usr/share`

### Why it exists
- Main software installation location
- Can be mounted read-only

---

## `/var`
**Type:** Variable data  
**Purpose:** Frequently changing data

### What it contains
- Logs (`/var/log`)
- Cache, databases, mail

### Why it exists
- Keeps changing data separate from system files
- Prevents root partition issues

---

## Quick Revision Table

| Directory | Purpose |
|--------|--------|
| `/bin` | Essential commands |
| `/boot` | Bootloader and kernel |
| `/dev` | Hardware access |
| `/etc` | Configuration files |
| `/home` | User data |
| `/lib*` | Shared libraries |
| `/proc` | Process info |
| `/sys` | Hardware interface |
| `/tmp` | Temporary files |
| `/usr` | Installed software |
| `/var` | Logs & variable data |

---

## Final Notes
- Never randomly delete system directories
- Always understand purpose before modifying
- This structure is **critical for Linux stability**

---

**Use this document for interviews, daily Linux work, and DevOps fundamentals.**
