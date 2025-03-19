---
title: Network Configuration
updated: 2025-03-19 02:03:06Z
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

`auto eth0iface eth0 inet static address 192.168.1.2 netmask 255.255.255.0 gateway 192.168.1.1 dns-nameservers 8.8.8.8 8.8.4.4`

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

&nbsp;