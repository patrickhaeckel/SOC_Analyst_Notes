---
title: Network Configuration
updated: 2025-03-20 02:05:52Z
created: 2025-03-19 01:44:49Z
---

### **Configuring Network Interfaces in Ubuntu**

In Ubuntu, network interfaces can be configured using the `ifconfig` or `ip` commands. These tools allow users to view and modify network settings, making it easier to troubleshoot connectivity issues, assign IP addresses, and manage network configurations.

Understanding how to configure network interfaces is essential in today's digital landscape, where efficient networking is critical for both personal and enterprise-level systems.

### **Viewing Network Interface Information**

To check the status of network interfaces, including IP addresses, netmasks, and connection status, you can use the following commands:

#### **Using `ifconfig` (Deprecated)**

`ifconfig`

- Displays details about available network interfaces.
- Useful for diagnosing connection issues or verifying configurations.
- **Note**: `ifconfig` has been deprecated in favor of `ip` in modern Linux distributions but is still available on many systems.

#### **Using `ip` (Recommended)**

`ip addr show`

- Provides detailed network interface information, including IP addresses.
- More feature-rich than `ifconfig` and supports newer networking functionalities.

### **Key Differences Between `ifconfig` and `ip`**

| Feature | `ifconfig` (Deprecated) | `ip` (Modern Alternative) |
| --- | --- | --- |
| Interface status | `ifconfig` | `ip link show` |
| IP addresses | `ifconfig -a` | `ip addr show` |
| Enable interface | `ifconfig eth0 up` | `ip link set eth0 up` |
| Disable interface | `ifconfig eth0 down` | `ip link set eth0 down` |

### **Why Use `ip` Instead of `ifconfig`?**

- **More detailed output:** Supports IPv6 and advanced networking features.
- **Actively maintained:** Part of the `iproute2` package, which replaces older networking tools.
- **Better performance:** Optimized for modern Linux distributions.

By mastering network interface configuration, you can effectively manage and troubleshoot network connections, ensuring seamless communication between systems.

### **Network Configuration Commands: `ifconfig` and `route`**

The table below summarizes essential commands for configuring a network interface.

| **Task** | **Command (`ifconfig` & `route`)** | **Description** |
| --- | --- | --- |
| **Assign an IP Address** | `sudo ifconfig eth0 192.168.1.2` | Sets the IP address of `eth0` to `192.168.1.2`. |
| **Assign a Netmask** | `sudo ifconfig eth0 netmask 255.255.255.0` | Defines the subnet mask for `eth0`, restricting the network size. |
| **Assign a Default Gateway** | `sudo route add default gw 192.168.1.1 eth0` | Configures `192.168.1.1` as the default gateway for `eth0`, enabling external network access. |

**Note**: The `ifconfig` and `route` commands are deprecated in modern Linux distributions. You should use the `ip` command instead:

`sudo ip addr add 192.168.1.2/24 dev eth0`

`sudo ip route add default via 192.168.1.1 dev eth0`

### **Editing DNS Settings (`/etc/resolv.conf`)**

DNS settings allow your system to resolve domain names to IP addresses. To temporarily set DNS servers, edit the `/etc/resolv.conf` file:

`sudo vim /etc/resolv.conf`

Add the following lines:

`nameserver 8.8.8.8nameserver 8.8.4.4`

- **8.8.8.8 & 8.8.4.4** are Google's public DNS servers, ensuring fast and reliable domain resolution.

**Warning**: Changes to `/etc/resolv.conf` are not persistent. The file may be overwritten by `NetworkManager` or `systemd-resolved`. To make DNS settings permanent, modify your network configuration files (see below).

* * *

### **Making Network Settings Persistent (`/etc/network/interfaces`)**

To ensure your network settings persist after a reboot, edit the `/etc/network/interfaces` file:

`sudo vim /etc/network/interfaces`

Add the following configuration:

```txt
auto eth0
iface eth0 inet static
  address 192.168.1.2
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8 8.8.4.4
```

- **`auto eth0`**: Automatically enables the `eth0` interface on boot.
- **`iface eth0 inet static`**: Configures `eth0` to use a static IP.
- **`address`, `netmask`, `gateway`**: Defines static network settings.
- **`dns-nameservers`**: Configures permanent DNS settings.

* * *

### **Applying Changes**

After modifying network settings, restart the networking service:

`sudo systemctl restart networking`

This applies the new configuration without requiring a system reboot.

* * *

## 1\. Ping (Packet Internet Groper)

**Purpose:** Tests basic connectivity between two network points by measuring round-trip time.

**How it works:** Ping sends ICMP Echo Request packets to the target and waits for ICMP Echo Reply responses.

**Example:**

`ping google.com`

**Sample output:**

```txt
PING google.com (142.250.186.78) 56(84) bytes of data.
64 bytes from lga25s71-in-f14.1e100.net (142.250.186.78): icmp_seq=1 ttl=59 time=12.3 ms
64 bytes from lga25s71-in-f14.1e100.net (142.250.186.78): icmp_seq=2 ttl=59 time=11.8 ms
64 bytes from lga25s71-in-f14.1e100.net (142.250.186.78): icmp_seq=3 ttl=59 time=13.1 ms
```

**Advanced usage:**

- `ping -c 5 google.com` (send exactly 5 packets)
- `ping -i 0.2 google.com` (send packets every 0.2 seconds instead of default 1 second)
- `ping -s 1500 google.com` (use larger packet size to test MTU issues)

**Troubleshooting value:** Helps diagnose basic connectivity issues, network latency problems, and packet loss.

## 2\. Traceroute (tracert in Windows)

**Purpose:** Maps the entire path packets take across a network to reach their destination.

**How it works:** Traceroute sends packets with incrementing TTL (Time To Live) values, forcing routers along the path to respond when the TTL expires, revealing each hop.

**Example:**

`traceroute google.com`

**Sample output:**

```txt
traceroute to google.com (142.250.186.78), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  1.235 ms  1.148 ms  1.087 ms
 2  172.16.34.1 (172.16.34.1)  12.432 ms  12.168 ms  12.380 ms
 3  10.242.2.65 (10.242.2.65)  13.871 ms  13.468 ms  13.686 ms
 4  72.14.209.106 (72.14.209.106)  14.233 ms  13.901 ms  14.063 ms
 5  142.250.186.78 (142.250.186.78)  15.123 ms  14.889 ms  15.002 ms
```

**Advanced usage:**

- `traceroute -n google.com` (numeric output only, no DNS resolution)
- `traceroute -m 30 google.com` (increase maximum hops to 30)
- `traceroute -I google.com` (use ICMP instead of UDP packets)

**Troubleshooting value:** Identifies routing issues, network bottlenecks, and points of failure in a connection path.

## 3\. Netstat (Network Statistics)

**Purpose:** Displays active network connections, routing tables, interface statistics, and listening ports.

**How it works:** Reads various network-related data structures from the kernel.

**Example:**

`netstat -tulnp`

- t: TCP connections
- u: UDP connections
- l: Listening sockets only
- n: Show numeric addresses instead of resolving hostnames
- p: Show the process using the socket

**Sample output:**

```txt
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1234/sshd
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1456/sendmail
tcp6       0      0 :::80                   :::*                    LISTEN      2345/apache2
udp        0      0 0.0.0.0:123             0.0.0.0:*                           1789/ntpd
```

**Advanced usage:**

- `netstat -s` (display statistics for all protocols)
- `netstat -r` (display routing table)
- `netstat -ie` (display interface statistics)

**Troubleshooting value:** Identifies unexpected open ports, validates service availability, detects potentially malicious connections, and helps diagnose network-related application issues.

## 4\. Tcpdump

**Purpose:** Command-line packet analyzer that captures and displays network traffic.

**How it works:** Places the network interface in promiscuous mode to capture all packets that pass through it.

**Example:**

`sudo tcpdump -i eth0`

**Sample output:**

```txt
13:24:30.904943 IP 192.168.1.5.59102 > 142.250.186.78.443: Flags [S], seq 1013904898, win 64240, options [mss 1460,sackOK,TS val 3243255321 ecr 0,nop,wscale 7], length 0
13:24:30.918950 IP 142.250.186.78.443 > 192.168.1.5.59102: Flags [S.], seq 2873580043, ack 1013904899, win 65535, options [mss 1460], length 0
13:24:30.918982 IP 192.168.1.5.59102 > 142.250.186.78.443: Flags [.], ack 1, win 64240, length 0
```

**Advanced filtering:**

- `tcpdump -i eth0 host 192.168.1.5` (capture traffic to/from specific IP)
- `tcpdump -i eth0 port 80` (capture HTTP traffic)
- `tcpdump -i eth0 tcp` (capture only TCP traffic)
- `tcpdump -i eth0 'tcp port 80 and host 192.168.1.5'` (combine filters)
- `tcpdump -i eth0 -w capture.pcap` (save capture to file for later analysis)

**Troubleshooting value:** Provides detailed packet-level analysis, helps diagnose protocol-specific issues, captures evidence of network-based attacks, and validates network behavior.

## 5\. Wireshark

**Purpose:** Graphical network protocol analyzer that provides visual packet inspection and sophisticated filtering.

**How it works:** Captures packets from network interfaces and provides a graphical interface for analysis.

**Key features:**

- Color-coded packet identification by protocol
- Protocol dissection (breaking down packet contents by protocol layer)
- Advanced filtering with expression builder
- Statistical analysis tools
- Follow TCP/UDP stream functionality
- Decryption capabilities for several protocols

**Common filters:**

- `http` (show only HTTP traffic)
- `tcp.port == 443` (show only HTTPS traffic)
- `ip.addr == 192.168.1.5` (traffic to/from specific IP)
- `!(arp or icmp)` (exclude ARP and ICMP traffic)
- `http.request.method == "POST"` (show only HTTP POST requests)

**Troubleshooting value:** Offers the most comprehensive packet analysis, helps diagnose complex application issues, identifies malformed packets, validates encryption, and provides detailed protocol analysis.

## 6\. Nmap (Network Mapper)

**Purpose:** Network discovery and security auditing tool that scans hosts to determine services running, operating systems, and potential vulnerabilities.

**How it works:** Sends specially crafted packets to target systems and analyzes the responses.

**Example:**

`nmap -A 192.168.1.0/24`

- \-A: Aggressive scan (OS detection, version detection, script scanning, and traceroute)

**Sample output (abbreviated):**

```txt
Nmap scan report for 192.168.1.5
Host is up (0.0023s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.2p1 (protocol 2.0)
80/tcp   open  http        Apache httpd 2.4.41
443/tcp  open  https       Apache httpd 2.4.41
3306/tcp open  mysql       MySQL 8.0.27
8080/tcp open  http-proxy  Squid http proxy 4.10
MAC Address: 00:1A:2B:3C:4D:5E
Device type: general purpose
Running: Linux 5.X
OS details: Linux 5.0 - 5.4
Network Distance: 1 hop
```

**Advanced scan types:**

- `nmap -sS 192.168.1.5` (SYN stealth scan)
- `nmap -sU 192.168.1.5` (UDP scan)
- `nmap -sV 192.168.1.5` (version detection)
- `nmap -O 192.168.1.5` (OS detection)
- `nmap --script vuln 192.168.1.5` (vulnerability scanning)
- `nmap -T4 -F 192.168.1.5` (fast scan of common ports)

**Troubleshooting value:** Confirms service availability, identifies unauthorized services, validates firewall configurations, detects potential vulnerabilities, and helps create network inventories.

## Relationship Between These Tools

These tools form a comprehensive network diagnostic ecosystem:

- **Ping** provides basic connectivity testing
- **Traceroute** extends ping to map the network path
- **Netstat** examines local network connections
- **Tcpdump** captures raw packet data for analysis
- **Wireshark** provides visual analysis of tcpdump-like captures
- **Nmap** actively probes network services

&nbsp;

# Linux Security Mechanisms: SELinux, AppArmor, and TCP Wrappers

## SELinux (Security-Enhanced Linux)

**Core concept:** SELinux implements Mandatory Access Control (MAC) that goes beyond traditional Unix permissions by enforcing system-wide security policies.

**How it works:**

1.  SELinux assigns security contexts to all files, processes, and system resources
2.  Every action must comply with centrally controlled security policies
3.  It operates on the principle of "deny by default" - anything not explicitly allowed is denied

**Key components:**

- **Security contexts:** Labels containing user:role:type:level information attached to all objects
- **Policy rules:** Define which contexts can access which resources
- **Modes of operation:**
    - Enforcing: Blocks prohibited actions and logs attempts
    - Permissive: Logs would-be violations but allows them (testing mode)
    - Disabled: SELinux is turned off completely

**Real-world application:** If a web server process running as Apache is compromised, SELinux prevents the attacker from accessing files outside the web server's designated context, even if traditional Unix file permissions would allow it. For example, it can prevent a compromised Apache process from accessing /etc/shadow or other sensitive files.

**Management tools:**

- `sestatus` - Shows SELinux status
- `setenforce` - Toggles between enforcing/permissive modes
- `audit2allow` - Helps create new policy rules from denied actions

## AppArmor

**Core concept:** AppArmor is also a MAC system but takes a path-based approach that focuses on profiling individual applications rather than labeling all system objects.

**How it works:**

1.  Uses profiles to define what resources (files, networks, capabilities) specific applications can access
2.  Each profile can be set to enforcement or complain mode
3.  Easier to implement than SELinux but potentially less comprehensive

**Key components:**

- **Profiles:** Text files in /etc/apparmor.d/ describing application permissions
- **Paths:** Rather than contexts, AppArmor uses file paths to define access controls
- **Modes:**
    - Enforce: Blocks prohibited actions
    - Complain: Logs violations without blocking (similar to SELinux permissive)

**Real-world application:** An AppArmor profile for Firefox might allow it to write to the Downloads directory and ~/.mozilla/ but prevent it from accessing system configuration files or other user data. If a browser vulnerability is exploited, the damage is contained to permitted paths.

**Management tools:**

- `aa-status` - Shows AppArmor status and loaded profiles
- `aa-enforce` - Puts profiles in enforcement mode
- `aa-complain` - Sets profiles to complain mode
- `aa-genprof` - Helps generate new profiles by monitoring application behavior

## TCP Wrappers

**Core concept:** A host-based network access control system that filters incoming network connections based on rules.

**How it works:**

1.  Uses two configuration files: /etc/hosts.allow and /etc/hosts.deny
2.  When a connection attempt occurs, TCP wrappers checks these files to determine if access should be granted
3.  Processing follows "allow, then deny" order - hosts.allow is checked first

**Key components:**

- **hosts.allow:** Lists rules for services/hosts that are permitted
- **hosts.deny:** Lists rules for services/hosts that are rejected
- **Rule syntax:** `daemon_list : client_list : option : option`

**Real-world application:** A server might use TCP wrappers to allow SSH connections only from specific IP ranges. For example, a rule in hosts.allow might state `sshd: 192.168.1.` to allow SSH access only from the local network, while hosts.deny contains `sshd: ALL` to deny all other connections.

**Management considerations:**

- Many modern services no longer use TCP wrappers by default
- Works only with services compiled with libwrap support
- Being replaced by firewall solutions like iptables/nftables
- No dedicated management tools - directly edit configuration files

## Comparing the Three Mechanisms

**Implementation approach:**

- SELinux: System-wide, comprehensive labeling of all objects
- AppArmor: Application-specific profiles based on file paths
- TCP Wrappers: Network connection filtering at the service level

**Protection scope:**

- SELinux: Protects against system-wide threats, unauthorized access between domains
- AppArmor: Contains individual application misbehavior
- TCP Wrappers: Guards against network-based intrusion for specific services

**Administration complexity:**

- SELinux: Highest learning curve, more complex configuration
- AppArmor: Moderately complex, easier to understand paths vs. contexts
- TCP Wrappers: Simplest with straightforward allow/deny rules

These complementary security mechanisms can work together to provide layered security: TCP wrappers control which connections reach services, AppArmor or SELinux constrain what those services can do on the system, creating a robust defense strategy.