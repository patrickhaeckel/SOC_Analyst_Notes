---
title: 'File System '
updated: 2025-02-25 15:09:58Z
created: 2025-02-20 15:26:52Z
---

| **Path** | **Description** |
| --- | --- |
| `/` | The top-level directory is the root filesystem and contains all of the files required to boot the operating system before other filesystems are mounted, as well as the files required to boot the other filesystems. After boot, all of the other filesystems are mounted at standard mount points as subdirectories of the root. |
| `/bin` | Contains essential user command binaries like `ls`, `cp`, `mv`, `cat`, `echo`, etc. These commands are required for system booting and basic operation. |
| `/boot` | Stores bootloader files, including the Linux kernel (`vmlinuz`), `grub` configuration files, and other boot-related files. |
| `/dev` | Contains device files representing hardware like hard drives (`/dev/sda`), partitions, USB devices, and system pseudo-devices like `/dev/null`. |
| `/etc` | Stores system-wide configuration files and scripts. Examples: `/etc/passwd` (user accounts), `/etc/fstab` (filesystem mounts), `/etc/ssh/sshd_config` (SSH settings). |
| `/home` | Each user on the system has a subdirectory here for storage. Stores personal files, documents, downloads, and user-specific settings. |
| `/lib` | Holds shared libraries required by essential system binaries in `/bin` and `/sbin`. Includes kernel modules (`/lib/modules`). |
| `/media` | External removable media devices such as USB drives are mounted here. |
| `/mnt` | Used for manually mounting filesystems, like external hard drives or network shares. |
| `/opt` | Contains optional third-party software packages. Used by some applications like Google Chrome or proprietary software. |
| `/root` | The home directory for the root user. |
| `/sbin` | This directory contains executables used for system administration (binary system files). |
| `/tmp` | The operating system and many programs use this directory to store temporary files. This directory is generally cleared upon system boot and may be deleted at other times without any warning. |
| `/usr` | Contains executables, libraries, man files, etc. |
| `/var` | This directory contains variable data files such as log files, email in-boxes, web application related files, cron files, and more. |
| `/run` | Stores runtime information like process IDs (PID files), sockets, and temporary files. It is a tmpfs, meaning it is cleared on reboot |
| `/proc` | A virtual filesystem providing system and process information. Examples:`/proc/cpuinfo` (CPU details) `/proc/meminfo` (Memory details) `/proc/[PID]` (Process details) |
| `/sys` | Another virtual filesystem exposing kernel and hardware information. Provides interfaces to interact with devices, drivers, and kernel settings. |