---
title: Linux Security
updated: 2025-03-26 01:46:47Z
created: 2025-03-21 01:58:11Z
---

# Enhancing Linux System Security

Proper firewall configurations are critical at the network level, but if these are insufficient, we can enforce restrictions using **Linux firewall (UFW, firewalld) and iptables** to control inbound and outbound traffic at the host level.

#### **Securing SSH Access**

If **SSH is open** on a server, the following security best practices should be enforced:

- **Disable password authentication**: Only allow SSH key-based authentication.
    
- **Prevent root login via SSH**: The root user should not be able to log in remotely.
    
- **Minimize root user access**: Instead of administering the system as root, use a principle of least privilege approach.
    
- **Use sudo wisely**: If users need elevated privileges, grant access only to specific commands in the sudoers file rather than providing full sudo rights.
    
- **Implement fail2ban**: This tool monitors failed login attempts and blocks the originating IP after exceeding a defined threshold, reducing brute-force attack risks.
    

#### **System Auditing & Hardening**

Regular system audits help identify security weaknesses that could lead to **privilege escalation**. Common areas to check include:

- **Kernel updates**: Some Linux distributions require manual kernel updates to patch security vulnerabilities.
    
- **User permissions**: Ensure no unnecessary privileges are assigned to users.
    
- **World-writable files**: Files that allow write access to everyone can be a security risk.
    
- **Misconfigured cron jobs and services**: Ensure only trusted and secure jobs run automatically.
    

&nbsp;

### **TCP Wrappers: Controlling Access to Services in Linux**

**TCP Wrappers** is a security mechanism in Linux that allows administrators to **restrict access to network services** based on the client’s **hostname, domain, or IP address**. This adds an additional layer of security to services by determining whether a connection should be allowed before the service itself is engaged.

#### **How TCP Wrappers Work**

When a client attempts to connect to a service protected by TCP Wrappers:

1.  The system **checks the client’s IP or hostname** against the configuration files (`/etc/hosts.allow` and `/etc/hosts.deny`).
    
2.  If a matching rule is found in `/etc/hosts.allow`, the connection is **granted**.
    
3.  If not, the system then checks `/etc/hosts.deny`; if a match is found, the connection is **denied**.
    
4.  If the client’s IP is in neither file, the default behavior is to **allow access**, unless otherwise configured.
    

#### **Configuration Files**

TCP Wrappers use two main configuration files to define access rules:

- **`/etc/hosts.allow`** – Defines **which clients are allowed** to access specific services.
    
- **`/etc/hosts.deny`** – Defines **which clients are denied** access to specific services.
    

#### **Example Configurations**

##### `/etc/hosts.allow` (Allow Rules)

```
# Allow SSH access from the local network
sshd : 10.129.14.0/24

# Allow FTP access from a specific host
ftpd : 10.129.14.10

# Allow Telnet access from any host in the carrierfreight.local domain
telnetd : .carrierfreight.local

```

In this example:

- **SSH access** is granted to all clients in the `10.129.14.0/24` subnet.
    
- **FTP access** is allowed only from `10.129.14.10`.
    
- **Telnet access** is granted to any host in the `carrierfreight.local` domain.
    

##### `/etc/hosts.deny` (Deny Rules)

&nbsp;

```
# Deny access to all services from any host in the carrierfreight.com domain
ALL : .carrierfreight.com

# Deny SSH access from a specific host
sshd : 10.129.22.22

# Deny FTP access from the entire 10.129.22.0/24 subnet
ftpd : 10.129.22.0/24

```

Here:

- **All services** are blocked for clients from `carrierfreight.com`.
    
- **SSH access** is denied for `10.129.22.22`.
    
- **FTP access** is denied for all clients in `10.129.22.0/24`.
    

### **Important Considerations**

- **Rule Order Matters**: The **first matching rule** in `/etc/hosts.allow` or `/etc/hosts.deny` is applied.
    
- **TCP Wrappers ≠ Firewall**: Unlike firewalls, TCP Wrappers **only control access to services**, not ports. A firewall (like iptables) should still be used for full network security.
    
- **Only Works for Services Using `libwrap`**: Not all services support TCP Wrappers; modern systems often rely on firewalls instead.
    

&nbsp;

#### **Mandatory Access Controls (MAC)**

To further enhance security, **SELinux** (Security-Enhanced Linux) or **AppArmor** can be used. These tools enforce strict access control policies beyond standard UNIX permissions:

- **SELinux** assigns security labels to all files, processes, and system objects.
    
- **Policy rules define access control**: Who can modify files, execute programs, or read data.
    
- **Granular control**: For example, restricting an application from appending to specific log files or moving sensitive data.
    

#### **Additional Security Measures**

Beyond access control, various security tools and configurations should be considered:

- **Intrusion detection tools**: Snort, chkrootkit, rkhunter, and Lynis help detect security threats.
    
- **Service and software management**:
    
    - Disable unnecessary services and remove unused software.
        
    - Remove services relying on unencrypted authentication.
        
- **Logging & Time Synchronization**: Ensure **Syslog** is running and **NTP** is enabled to maintain accurate timestamps.
    
- **User account security**:
    
    - Every user should have an individual account.
        
    - Enforce strong password policies, including aging and history restrictions.
        
    - Lock accounts after multiple failed login attempts.
        
- **Set proper file permissions**: Disable unwanted **SUID/SGID binaries**, which can be exploited for privilege escalation.