---
title: Service and Process Management
updated: 2025-03-07 02:00:25Z
created: 2025-03-06 23:32:29Z
---

- **Systemd as Init System**: Most modern Linux distributions use **systemd** as their initialization system.
- **First Process on Boot**: Systemd is the first process started during boot and is assigned **Process ID (PID) 1**.
- **Process ID (PID)**: Every process in Linux is assigned a unique **PID**.
- **Process Information Location**: The `/proc/` directory contains details about each running process.
- **Parent and Child Processes**: Processes may have a **Parent Process ID (PPID)**, indicating they were started by another process.

&nbsp;

## **systemctl**

`systemctl` is a command used to manage the **systemd** init system in Linux. It allows you to control system services, manage system states, and check service statuses.

We can use `systemctl` to list all services. `systemctl list-units --type=service`

**Common Commands:**

- `systemctl start <service>` → Start a service
- `systemctl stop <service>` → Stop a service
- `systemctl restart <service>` → Restart a service
- `systemctl enable <service>` → Enable a service to start at boot
- `systemctl disable <service>` → Disable a service from starting at boot
- `systemctl status <service>` → Check service status

`journalctl` is a command used to view logs managed by **systemd's journal**. It is an alternative to traditional log files like `/var/log/syslog`.

**Common Commands:**

- `journalctl -u <service>` → View logs for a specific service
- `journalctl -f` → Follow live logs (like `tail -f`)
- `journalctl --since "2 hours ago"` → View logs from the last 2 hours
- `journalctl -b` → Show logs from the current boot

&nbsp;

### **PID (Process ID)**

A **Process ID (PID)** is a unique number assigned to a running process in Linux. You can find a process's PID using:

- `ps aux` → Lists all processes
- `pidof <process_name>` → Get the PID of a process
- `pgrep <process_name>` → Another way to find a PID  
    <br/>

&nbsp;

### **`kill`**

The `kill` command sends signals to processes using their **Process ID (PID)**. The default signal is **SIGTERM (15)**, which gracefully stops a process.

**Usage:** `kill <PID>`  Example: `kill 1234 # Gracefully stop process with PID 1234`

To **force kill** a process (using SIGKILL **9**): `kill -9 <PID>`  Example: `kill -9 5678 # Immediately terminate process with PID 5678`

To kill multiple processes: `kill 1234 5678 9012`

&nbsp;

```shell-session
kill -l

1.  SIGHUP 2) SIGINT 3) SIGQUIT 4) SIGILL 5) SIGTRAP
2.  SIGABRT 7) SIGBUS 8) SIGFPE 9) SIGKILL 10) SIGUSR1
3.  SIGSEGV 12) SIGUSR2 13) SIGPIPE 14) SIGALRM 15) SIGTERM
4.  SIGSTKFLT 17) SIGCHLD 18) SIGCONT 19) SIGSTOP 20) SIGTSTP
5.  SIGTTIN 22) SIGTTOU 23) SIGURG 24) SIGXCPU 25) SIGXFSZ
6.  SIGVTALRM 27) SIGPROF 28) SIGWINCH 29) SIGIO 30) SIGPWR
7.  SIGSYS 34) SIGRTMIN 35) SIGRTMIN+1 36) SIGRTMIN+2 37) SIGRTMIN+3
8.  SIGRTMIN+4 39) SIGRTMIN+5 40) SIGRTMIN+6 41) SIGRTMIN+7 42) SIGRTMIN+8
9.  SIGRTMIN+9 44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
10. SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
11. SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9 56) SIGRTMAX-8 57) SIGRTMAX-7
12. SIGRTMAX-6 59) SIGRTMAX-5 60) SIGRTMAX-4 61) SIGRTMAX-3 62) SIGRTMAX-2
13. SIGRTMAX-1 64) SIGRTMAX  
```

The most commonly used signals are:

| **Signal** | **Description** |
| --- | --- |
| `1` | `SIGHUP` - This is sent to a process when the terminal that controls it is closed. |
| `2` | `SIGINT` - Sent when a user presses `[Ctrl] + C` in the controlling terminal to interrupt a process. |
| `3` | `SIGQUIT` - Sent when a user presses `[Ctrl] + D` to quit. |
| `9` | `SIGKILL` - Immediately kill a process with no clean-up operations. |
| `11` | `SIGSEGV` - Indicates a segmentation fault (bad memory access). |
| `15` | `SIGTERM` - Program termination. |
| `19` | `SIGSTOP` - Stop the program. It cannot be handled anymore. |
| `20` | `SIGTSTP` - Sent when a user presses `[Ctrl] + Z` to request for a service to suspend. The user can handle it afterward. |
| `18` | `SIGCONT` - Resumes a paused process. |

### **`pkill`**

`pkill` kills processes **by name** instead of PID. It is useful when you don’t want to manually find PIDs.  **Usage:** `pkill <process_name>`

### **`pgrep`**

`pgrep` searches for processes by name and returns their PIDs. **Usage:** `pgrep <process_name>`

### **`killall`**

`killall` kills all instances of a process by name (similar to `pkill` but more aggressive). **Usage:** `killall <process_name>`

&nbsp;

**List background processes**

`jobs`   **Example output**:  `[1]+ Running ping -c 10 www.hackthebox.eu &`

**Bring a background process to the foreground**

`fg <job_id>`  **Example**: `fg 1`

&nbsp;

**Executing Multiple Commands in Linux**

There are three ways to run multiple commands sequentially in Linux:

1.  **Semicolon (;)** - Executes all commands regardless of success or failure.
    
    `command1; command2; command3` Example: `echo '1'; echo '2'; echo '3'` Output: `1 2 3`
    
    Even if a command fails, subsequent commands run: `echo '1'; ls MISSING_FILE; echo '3'`
    
    Output: `1 ls: cannot access 'MISSING_FILE': No such file or directory 3`
    
2.  **Double Ampersand (&&)** - Executes the next command only if the previous one succeeds.
    
    `command1 && command2 && command3` Example: `echo '1' && ls MISSING_FILE && echo '3'`
    
    Output: `1 ls: cannot access 'MISSING_FILE': No such file or directory`
    
    Since `ls MISSING_FILE` fails, `echo '3'` is not executed.
    
3.  **Pipe (|)** - Passes the output of one command as input to the next.
    
    `command1 | command2 | command3`
    
    Example:
    
    `ls | grep '.txt'`
    
    This filters and displays only `.txt` files from the `ls` command output.    
    Pipes (`|`) depend not only on the correct and error-free operation of the previous processes but also on the previous processes' results.
    

&nbsp;

Use the "systemctl" command to list all units of services and submit the unit name with the description "Load AppArmor profiles managed internally by snapd" as the answer.

systemctl list-units | grep Armor