---
title: Linux Firewall
updated: 2025-03-27 02:51:29Z
created: 2025-03-26 02:05:30Z
---

### **Iptables and Alternative Firewall Solutions**

**iptables** is a powerful tool for filtering network traffic using customizable rules based on IP addresses, ports, and protocols. While widely used, there are newer alternatives:

- **nftables**: A modern replacement for iptables with improved performance and a cleaner syntax. However, nftables rules are not directly compatible with iptables, requiring manual migration.
    
- **UFW (Uncomplicated Firewall)**: A user-friendly front-end for iptables, simplifying firewall rule management.
    
- **FirewallD**: A dynamic firewall solution that supports complex configurations, including custom zones and services.
    

### **Core Components of iptables**

| **Component** | **Description** |
| --- | --- |
| **Tables** | Organize firewall rules into categories. |
| **Chains** | Group rules for specific types of traffic. |
| **Rules** | Define filtering criteria and actions for packets. |
| **Matches** | Specify conditions like IPs, ports, and protocols. |
| **Targets** | Determine actions (accept, drop, reject, or modify packets). |

- **Purpose of Firewalls**:
    
    - Control and monitor network traffic between different network segments.
        
    - Protect networks from unauthorized access and security threats.
        
    - Filter incoming and outgoing traffic based on rules, protocols, ports, etc.
        
- **Linux Firewall Capabilities**:
    
    - Linux includes built-in firewall tools for network security.
        
    - Ensures confidentiality, integrity, and availability of network resources.
        
- **History of Linux Firewalls**:
    
    - **ipfwadm** → **ipchains** → **iptables**.
        
    - **iptables** introduced in Linux 2.4 (2000), became the standard firewall tool.
        
- **iptables Features**:
    
    - Command-line interface for configuring firewall rules.
        
    - Filters traffic based on IP addresses, ports, protocols, etc.
        
    - Protects against DoS attacks, port scans, and intrusions.
        
    - Highly customizable for complex rulesets.
        
- **Netfilter Framework**:
    
    - Built into the Linux kernel.
        
    - Provides hooks for intercepting and modifying network traffic.
        
    - **iptables** is a front-end tool for configuring Netfilter rules.
        

&nbsp;

# **Understanding Tables and Chains in iptables**

When configuring firewalls on Linux systems, it's essential to understand how **tables** and **chains** work in `iptables`. These components organize and manage firewall rules based on the type of network traffic they handle.

https://www.youtube.com/watch?v=6Ra17Qpj68c&t=52s

* * *

## **Tables in iptables**

Tables in `iptables` group firewall rules by their function. Each table is designed for a specific task:

| **Table Name** | **Purpose** | **Built-in Chains** |
| --- | --- | --- |
| **filter** | Filters network traffic based on IP addresses, ports, and protocols. | INPUT, OUTPUT, FORWARD |
| **nat** | Modifies source or destination IP addresses of packets. | PREROUTING, POSTROUTING |
| **mangle** | Alters packet headers for advanced network control. | PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING |
| **raw** | Bypasses connection tracking for special packet processing. | PREROUTING, OUTPUT |

* * *

## **Chains in iptables**

Chains are sets of rules that determine how network traffic is handled. There are two types:

1.  **Built-in Chains** – Predefined chains automatically created in each table. Examples include:
    
    - **INPUT** (handles incoming traffic)
        
    - **OUTPUT** (handles outgoing traffic)
        
    - **FORWARD** (handles traffic passing through the system)
        
2.  **User-defined Chains** – Custom chains created by users to simplify rule management.
    
    - Example: A user-defined chain could group all firewall rules related to **web server traffic (port 80)**, making them easier to manage.

Each built-in chain is associated with specific tables:

- The **filter table** has INPUT, OUTPUT, and FORWARD chains for filtering traffic.
    
- The **nat table** uses PREROUTING and POSTROUTING to modify packet addresses before or after routing.
    
- The **mangle table** has multiple chains to manipulate packet headers at different processing stages.
    

User-defined chains improve rule organization by allowing administrators to group related rules for better efficiency and readability.

&nbsp;

# **Understanding Rules, Targets, and Matches in iptables**

In `iptables`, rules define how network traffic is handled based on specific criteria. These rules are added to chains using the `-A` (append) option and can be modified or removed using other options.

* * *

## **Rules and Targets**

Each rule consists of:

1.  **Criteria (Matches)** – Conditions that must be met for a rule to apply (e.g., source IP, protocol, port).
    
2.  **Target (Action)** – The action taken when traffic matches the rule (e.g., accept, drop, modify).
    

### **Common Targets in iptables**

| **Target** | **Description** |
| --- | --- |
| **ACCEPT** | Allows the packet through the firewall. |
| **DROP** | Blocks the packet without notification. |
| **REJECT** | Blocks the packet and sends an error message to the sender. |
| **LOG** | Logs packet details in the system log. |
| **SNAT** | Modifies the source IP (used in NAT). |
| **DNAT** | Modifies the destination IP (used in NAT). |
| **MASQUERADE** | Similar to SNAT but used for dynamic IPs. |
| **REDIRECT** | Redirects traffic to another port/IP. |
| **MARK** | Tags packets for advanced processing (e.g., routing). |

### **Example Rule: Allow SSH Traffic on Port 22**

To allow incoming TCP traffic on **port 22 (SSH)**, use:

`sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT`

* * *

## **Matches in iptables**

Matches define the conditions a packet must meet for a rule to apply. They check details such as **protocol, IP address, port number, and connection state**.

### **Common Match Options**

| **Match** | **Description** |
| --- | --- |
| `-p` or `--protocol` | Specifies the protocol (e.g., `tcp`, `udp`, `icmp`). |
| `--dport` | Matches a specific **destination port** (e.g., `--dport 80` for HTTP). |
| `--sport` | Matches a specific **source port**. |
| `-s` or `--source` | Matches packets from a specific **source IP address**. |
| `-d` or `--destination` | Matches packets destined for a specific **IP address**. |
| `-m state` | Matches packets based on **connection state** (e.g., `NEW`, `ESTABLISHED`). |
| `-m multiport` | Matches multiple ports or port ranges. |
| `-m tcp` | Matches TCP packets with additional TCP options. |
| `-m udp` | Matches UDP packets with additional UDP options. |
| `-m string` | Matches packets containing a **specific string**. |
| `-m limit` | Limits packet matches to control traffic rates. |
| `-m conntrack` | Matches packets based on **connection tracking**. |
| `-m mark` | Matches packets with a **Netfilter mark** (for advanced routing). |
| `-m mac` | Matches packets based on **MAC address**. |
| `-m iprange` | Matches packets within a specific **IP range**. |

### **Example Rule: Allow HTTP Traffic on Port 80**

To allow incoming TCP traffic on **port 80 (HTTP)**, use:

`sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT`

- `-p tcp` → Specifies TCP protocol
    
- `-m tcp` → Uses TCP match module
    
- `--dport 80` → Matches destination port **80**
    
- `-j ACCEPT` → Allows the packet
    

&nbsp;