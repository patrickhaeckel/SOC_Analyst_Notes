---
title: Useful Linux Paths
updated: 2025-04-09 02:14:00Z
created: 2025-04-09 01:55:28Z
---

### **System Config & Info**

| File/Path | Description |
| --- | --- |
| `/etc/passwd` | User account info (not passwords) |
| `/etc/shadow` | Password hashes (root-only readable) |
| `/etc/group` | Group memberships |
| `/etc/hostname` | System's hostname |
| `/etc/hosts` | Static DNS mappings (e.g., 127.0.0.1 myapp.local) |
| `/etc/resolv.conf` | DNS servers used |
| `/etc/os-release` | Distro info (e.g., ID=ubuntu, VERSION_ID=22.04) |
| `/etc/fstab` | Filesystems to mount at boot |
| `/etc/crypttab` | Encrypted filesystems configuration |
| `/etc/timezone` | System timezone |
| `/etc/localtime` | Symlink to active timezone data |
| `/etc/network/interfaces` | Network interface config (Debian) |
| `/etc/netplan/` | Network configuration (Ubuntu) |
| `/etc/ntp.conf` | NTP time server config |
| `/etc/ssh/sshd_config` | SSH server configuration |
| `/etc/ssh/ssh_config` | SSH client defaults |
| `/etc/sysctl.conf` | Kernel parameters config |
| `/etc/systemd/system/` | Systemd service unit files |
| `/etc/motd` | Message of the day shown at login |
| `/etc/issue` | Pre-login message |
| `/etc/security/limits.conf` | Resource limits for users |
| `/etc/ld.so.conf` | Dynamic linker config |
| `/etc/environment` | System-wide environment variables |
| `/etc/modules` | Kernel modules to load at boot |

* * *

### **Boot & Kernel Stuff**

| File/Path | Description |
| --- | --- |
| `/boot/grub/grub.cfg` | GRUB bootloader config |
| `/boot/vmlinuz-*` | Linux kernel binary |
| `/proc/cmdline` | Kernel boot parameters |
| `/boot/initrd.img-*` | Initial RAM disk image |
| `/boot/System.map-*` | Kernel symbol table |
| `/boot/config-*` | Kernel build configuration |
| `/etc/default/grub` | GRUB default settings |
| `/boot/efi/` | EFI boot files (on UEFI systems) |
| `/proc/version` | Running kernel version info |
| `/etc/modules-load.d/` | Kernel modules to load at boot |
| `/lib/modules/$(uname -r)/` | Kernel modules for current kernel |
| `/sys/kernel/debug/` | Kernel debugging info |
| `/proc/sys/kernel/` | Kernel tuning parameters |

* * *

### **System Runtime Info (/proc, /sys)**

| File/Path | Description |
| --- | --- |
| `/proc/cpuinfo` | CPU details |
| `/proc/meminfo` | Memory info |
| `/proc/[PID]/` | Process-specific info |
| `/proc/net/tcp`, `/udp` | Current TCP/UDP connections |
| `/sys/class/net/` | Network interfaces (eth0, lo, etc.) |
| `/proc/mounts` | Active mounts |
| `/proc/loadavg` | System load averages |
| `/proc/uptime` | System uptime |
| `/proc/stat` | System statistics |
| `/proc/interrupts` | IRQ usage |
| `/proc/devices` | Registered char/block devices |
| `/proc/filesystems` | Supported filesystems |
| `/proc/swaps` | Active swap spaces |
| `/sys/class/thermal/` | Thermal sensors |
| `/sys/class/power_supply/` | Battery/power info |
| `/sys/class/backlight/` | Screen brightness controls |
| `/sys/class/block/` | Block devices |
| `/sys/firmware/efi/` | EFI variables (if applicable) |
| `/proc/partitions` | Disk partition info |
| `/proc/diskstats` | Disk I/O statistics |
| `/proc/vmstat` | VM statistics |
| `/proc/zoneinfo` | Memory zone info |

* * *

### **Logs & System State**

| File/Path | Description |
| --- | --- |
| `/var/log/auth.log` or `/secure` | Logins & sudo attempts |
| `/var/log/syslog` or `/messages` | General system logs |
| `/var/log/dmesg` | Kernel messages |
| `/var/run/` → `/run/` | Daemon runtime files |
| `/run/systemd` | Systemd runtime info |
| `/var/log/kern.log` | Kernel logs |
| `/var/log/boot.log` | Boot logs |
| `/var/log/cron` | Cron job logs |
| `/var/log/faillog` | Failed login attempts |
| `/var/log/httpd/` or `/apache2/` | Web server logs |
| `/var/log/mail.log` | Mail server logs |
| `/var/log/mysql/` | MySQL database logs |
| `/var/log/wtmp` | Login records history |
| `/var/log/btmp` | Failed login attempts |
| `/var/log/lastlog` | Last login for each user |
| `/var/log/apt/` | Package management logs |
| `/var/log/cups/` | Printer logs |
| `/var/log/audit/` | Security audit logs |
| `/var/log/journal/` | Systemd journal logs |
| `/var/log/nginx/` | Nginx logs |
| `/var/log/firewalld` | Firewall logs |
| `/var/log/alternatives.log` | Update-alternatives logs |

* * *

### **User & Shell Stuff**

| File/Path | Description |
| --- | --- |
| `~/.bashrc`, `~/.zshrc` | Shell config (aliases, exports) |
| `~/.ssh/authorized_keys` | Allowed SSH keys |
| `~/.ssh/id_rsa`, `.pub` | Default SSH keypair |
| `~/.profile`, `~/.bash_profile` | Login shell config |
| `~/.config/` | App configs (e.g., VSCode) |
| `~/.local/bin/` | User-local executables |
| `~/.bash_logout` | Shell logout commands |
| `~/.inputrc` | Readline config |
| `~/.vimrc`, `~/.vim/` | Vim config |
| `~/.emacs`, `~/.emacs.d/` | Emacs config |
| `~/.gitconfig` | Git configuration |
| `~/.gnupg/` | GPG config |
| `~/.xsession`, `~/.xinitrc` | X session startup scripts |
| `~/.vnc/` | VNC configuration |
| `~/.mozilla/` | Firefox profile |
| `~/.cache/` | App cache data |
| `~/.wget-hsts` | Wget HSTS database |
| `~/.mysql_history` | MySQL command history |
| `~/.python_history` | Python interpreter history |
| `~/.lesshst` | `less` pager history |
| `~/.viminfo` | Vim editing history |
| `~/.aws/` | AWS CLI config |
| `~/.docker/` | Docker config |

* * *

### **Exploitation / Privilege Escalation Targets**

| File/Path | Description |
| --- | --- |
| `/etc/sudoers` | Sudo rules (edit with `visudo`) |
| `/home/*/.bash_history` | Shell history (possible creds) |
| `/root/` | Root's home directory |
| `/usr/bin/sudo` | Check for misconfigs/vulns |
| `/usr/bin/find`, `/python`, etc. | GTFOBins targets |
| `/tmp/`, `/dev/shm/` | Writable dirs for exploits |
| `/etc/crontab` | System cron jobs |
| `/var/spool/cron/` | User cron jobs |
| `/etc/cron.d/` | Cron job definitions |
| `/var/backups/` | May contain sensitive data |
| `/var/www/` | Web files (LFI/RFI potential) |
| `/var/lib/mysql/` | Database files |
| `/etc/passwd.bak`, `/shadow.bak` | Backup files with looser perms |
| `/proc/self/environ` | Process env vars (webshell vector) |
| `/proc/net/arp` | ARP table (network recon) |
| `/var/mail/`, `/spool/mail/` | Mailboxes |
| `/var/log/apache2/access.log` | For log poisoning |
| `/etc/ld.so.preload` | Preloaded libs (can hijack) |
| `/opt/` | Check third-party software perms |
| `/usr/local/bin/` | Custom executables (check perms) |
| `/etc/lsb-release` | OS version (for targeted exploits) |
| `/etc/passwd-` | Backup passwd file |

* * *

### **Package Management (Debian-based)**

| File/Path | Description |
| --- | --- |
| `/var/lib/dpkg/status` | Installed package status |
| `/var/cache/apt/archives/` | Cached `.deb` files |
| `/etc/apt/sources.list` | Repository list |
| `/var/log/apt/history.log` | Install history |
| `/etc/apt/sources.list.d/` | Additional repo definitions |
| `/var/lib/apt/lists/` | Repo metadata |
| `/etc/apt/preferences` | APT pinning rules |
| `/usr/share/doc/` | Package docs |
| `/var/lib/dpkg/info/` | Package scripts/lists |
| `/etc/dpkg/dpkg.cfg` | DPKG config |
| `/var/log/unattended-upgrades/` | Auto-upgrade logs |
| `/var/lib/apt/periodic/` | Periodic APT job status |

* * *

### **System Services & Daemons**

| File/Path | Description |
| --- | --- |
| `/etc/systemd/system/` | Custom systemd service units |
| `/usr/lib/systemd/system/` | Default systemd units |
| `/etc/init.d/` | SysV init scripts |
| `/etc/init/` | Upstart configs |
| `/etc/default/` | Default service values |
| `/etc/xinetd.conf`, `/xinetd.d/` | xinetd service configs |
| `/etc/rc.local` | Legacy startup script |
| `/etc/systemd/resolved.conf` | DNS resolution config |
| `/etc/systemd/timesyncd.conf` | Time sync config |
| `/etc/systemd/journald.conf` | Journal config |
| `/etc/systemd/logind.conf` | Login manager config |
| `/etc/systemd/system.conf` | System manager config |

* * *

### **Application Configs**

| File/Path | Description |
| --- | --- |
| `/etc/nginx/` | Nginx web server config |
| `/etc/apache2/` | Apache web server config |
| `/etc/mysql/` | MySQL database config |
| `/etc/php/` | PHP configuration |
| `/etc/postgresql/` | PostgreSQL config |
| `/etc/redis/` | Redis config |
| `/etc/bind/` | BIND DNS server config |
| `/etc/postfix/` | Mail transfer agent config |
| `/etc/dovecot/` | IMAP/POP3 server config |
| `/etc/openvpn/` | OpenVPN config |
| `/etc/wireguard/` | WireGuard VPN config |
| `/etc/samba/` | Samba file sharing config |
| `/etc/cups/` | Printing system config |
| `/etc/docker/` | Docker daemon config |