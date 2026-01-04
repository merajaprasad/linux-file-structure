# Linux Filesystem Hierarchy Reference

## Overview
This document breaks down the root directory structure (`/`) of a modern Linux system. It is based on the Filesystem Hierarchy Standard (FHS). Understanding this structure is critical for System Administration, troubleshooting, and security management.

## 1. The "Usr Merge" (Essential Binaries & Libraries)
On modern distributions (like Ubuntu, Debian, Fedora), the root directories for binaries and libraries are symbolic links pointing into `/usr`. This unifies the operating system resources.

| Directory | Target | Purpose | SysAdmin Usage |
| :--- | :--- | :--- | :--- |
| **`/bin`** | `/usr/bin` | **User Binaries.** Essential commands available to all users (e.g., `ls`, `cat`, `cp`). | Standard toolset location. |
| **`/sbin`** | `/usr/sbin` | **System Binaries.** Essential commands reserved for the root user/admin (e.g., `ip`, `fdisk`, `fsck`). | Tools for system repair and network config. |
| **`/lib`** | `/usr/lib` | **Libraries.** Shared library images needed by binaries in `/bin` and `/sbin`. | Equivalent to Windows `.dll` files. |
| **`/lib64`** | `/usr/lib64` | **64-bit Libraries.** Specific storage for 64-bit kernel libraries. | Dependency management. |

---

## 2. Configuration & Boot
These directories control how the system starts and behaves.

### `/etc` (Et Cetera / Editable Text Configuration)
* **Purpose:** Host-specific system-wide configuration files.
* **Key Files:** `/etc/passwd` (users), `/etc/fstab` (drives), `/etc/ssh/sshd_config` (remote access).
* **⚠ Admin Note:** This is the primary workspace for changing system settings. Always backup files here before editing (e.g., `cp config config.bak`).

### `/boot`
* **Purpose:** Static files of the boot loader.
* **Contents:** The Kernel (`vmlinuz`), Initramfs images, and `GRUB` configuration.
* **⚠ Admin Note:** If this partition fills up (common with old kernels), the system may fail to boot or update.

---

## 3. The Virtual Filesystem (Kernel Interface)
 These directories **do not exist on the hard drive**. They reside in RAM and act as an interface to the Linux Kernel.

### `/proc` (Processes)
* **Purpose:** Information about system processes and hardware.
* **Admin Note:** Use `cat /proc/meminfo` or `cat /proc/cpuinfo` for hardware stats without external tools. Numbered directories (e.g., `/proc/1234`) represent specific running Process IDs (PIDs).

### `/sys` (System)
* **Purpose:** Exposes kernel subsystems, hardware devices, and drivers.
* **Admin Note:** Think of the `/sys` directory as the interactive control panel for your hardware via terminal, In Linux, we say "everything is a file". If you want to talk to the physical hardware (like your hard drive, your screen brightness, or your battery), Linux turns that hardware into a file inside this. The `/sys` directory was created to be the bridge where you can do this. It is a Virtual Filesystem. It does not exist on your hard drive; it is created by the Kernel in RAM every time you boot up.

### `/dev` (Devices)
* **Purpose:** Location of special "device files."
* **Key Files:**
    * `/dev/sda` (First physical disk)
    * `/dev/null` (Data sink/black hole)
    * `/dev/tty` (Terminals)

---

## 4. Variable Data & Logs
Files that change in size and content while the system is running.

### `/var` (Variable)
* **Purpose:** Persistent variable data.
* **Key Subdirs:**
    * **`/var/log`**: System logs (syslog,Application log, auth.log). **Check here first when troubleshooting.**
    * **`/var/www`**: Web server content (Apache/Nginx).
    * **`/var/lib`**: Database storage (MySQL, PostgreSQL, MongoDB).

### `/tmp` (Temporary)
* **Purpose:** Temporary files created by applications or user.
* **Permissions:** `drwxrwxrwt`. The **'t' (Sticky Bit)** ensures users can write files here but only the owner (or root) can delete them.
* **Admin Note:** Usually cleared on reboot. Do not store important data here.

### `/run`
* **Purpose:** when a server is running, hundreds of processes are talking to each other, locking files, and tracking their own ID numbers. The most important thing to know about /run is that it exists only in your RAM.
* **Admin Note:** Replaces older locations like `/var/run`. Cleared completely on every reboot.
* **`Example:`**: PID Files (`.pid`), Unix Sockets (`.sock`), Lock Files (`.lock`).

---

## 5. User Data

### `/home`
* **Purpose:** Personal directories for standard users (e.g., `/home/alice`).
* **Admin Note:** Where users store documents and configs (`.bashrc`). Often on a separate partition for easier backups.

### `/root`
* **Purpose:** The home directory for the **root** user.
* **Why separate?** If `/home` is on a different partition and fails to mount, the root user still needs access to their profile and tools on the root partition to fix the system.

---

## 6. Software Locations & Mounts

### `/usr` (Unix System Resources)
* **Purpose:** Read-only user data. contains the vast majority of all software, libraries, and documentation used by the system during normal operation.
* **`/usr/local`**: This is the most important folder for a SysAdmin. * Usage: When you install software manually (not via apt or dnf), it should go here. It has its own bin, lib, and share subfolders. This ensures that manually installed software doesn't overwrite or get deleted by the system’s official package manager. If the Admin install software manually $\rightarrow$ it goes in `/usr/local/bin`.
* **`/usr/bin`**: This is where almost all user-executable commands live (e.g., `python`, `gcc`, `vim`). On modern systems, `/bin` is just a shortcut to this folder.
* **`/usr/sbin`**: "System Binaries." These are tools intended for the Administrator (root), such as network management or system daemons.
* **`/usr/libexec`**: This contains "helper" programs. These are binaries that are meant to be run by other programs, not directly by a human in the terminal.
* **`/usr/lib & lib64`**: These hold the shared libraries (code snippets) that the programs in `/bin` and `/sbin` need to run. If a program is a "car," the libraries are the "spare parts" it needs to function.
* **`/usr/include`**: This is for C/C++ developers. it contains "header files" (`.h`). When you compile software from source, the compiler looks here to understand how to interact with system libraries.
* **`/usr/src`**: "Source." This is where the source code for the Linux kernel or other system components might be stored for reference or manual compilation.
* **`/usr/share`**: This contains architecture-independent data. This means files that don't care if your CPU is `Intel` or `ARM`.
* **`/usr/games`**: Historically, this is where small system games (like `Solitaire` or `Minesweeper`) were stored to keep them separate from serious work tools.

### `/opt` (Optional)
* **Purpose:** Add-on application software packages (often proprietary/monolithic). A program in /opt keeps everything it needs—its own binaries, libraries, and icons—inside its own single sub-directory. Companies like Google, Adobe, or Oracle often don't want to split their files across the system. They put everything in one folder for easier installation and removal.If someone download a pre-compiled `.tar.gz` tool from a vendor's website (rather than using `apt` or `yum`), the "correct" place to move it is `/opt`
* **Examples:** Google Chrome - `/opt/google/chrome`, Zoom, Oracle Database, TeamViewr - `/opt/TeamViewer`, Edge - `/opt/microsoft/msedge`.

### `/snap`
* **Purpose:** Created by Canonical (the company behind Ubuntu), a Snap is a software package that contains the application, its libraries, and its dependencies all bundled into one file. When you install a Snap (like Spotify or VS Code), the system takes the compressed snap file and mounts it as a virtual drive. Mount points for Every snap package admin install (like firefox, docker, or vlc) gets its own folder here.
* **`/snap/bin`**: This is a very important folder for your PATH. It contains symlinks to the executable files for all your installed snaps.
* **`Tips`** : When you type firefox in your terminal, the system looks in `/usr/bin` (for the old version) and then `/snap/bin` (for the snap version).

### `/mnt` vs `/media`
* **`/mnt`**: For **manual** temporary mounts by the administrator (e.g., mounting a rescue USB).
* **`/media`**: For **automatic** mounts managed by the OS (e.g., when you plug in a flash drive and a popup appears).

---

## 7. System Health

### `/lost+found`
* **Purpose:** Recovery directory for `fsck` (File System Check).
* **Usage:** If the system crashes, recovered file fragments are placed here. If this directory is empty, the filesystem is likely healthy.

---

## Quick Reference: Understanding `ls -l` Permissions

In the listing provided (`drwxr-xr-x`), the characters represent:

1.  **Type:** `d` = directory, `l` = link, `-` = file.
2.  **User (Owner):** `rwx` (Read, Write, Execute).
3.  **Group:** `r-x` (Read, Execute, No Write).
4.  **Others:** `r-x` (Read, Execute, No Write).
