---
title: Network Services
updated: 2025-03-11 02:46:55Z
created: 2025-03-09 18:37:00Z
---

**SSH**

The **OpenSSH** configuration can be customized by modifying the **/etc/ssh/sshd_config** file using a text editor. This file allows adjustments to settings such as the **maximum concurrent connections, authentication methods (passwords or keys), and host key verification**. However, any changes should be made with caution.

For instance, SSH enables secure remote login and command execution. It also supports **tunneling and port forwarding**, allowing encrypted data transmission to inspect network and system settings without the risk of interception by third parties.

**NFS**

The **Network File System (NFS)** is a protocol that allows us to access and manage files on remote systems as if they were stored locally. It simplifies the management of files across networks. For example, administrators use NFS to centrally store and manage files on both **Linux and Windows systems**, facilitating collaboration and data management. Popular NFS servers for Linux include **NFS-UTILS (Ubuntu)**, **NFS-Ganesha (Solaris)**, and **OpenNFS (Redhat Linux)**.

NFS is also effective for sharing and managing resources, such as replicating file systems between servers. It provides features like **access controls**, **real-time file transfers**, and support for **multiple users accessing data simultaneously**. If an **FTP client** is unavailable on a target system, NFS can be used as an alternative.

We can install NFS on Linux with the following command:

`sudo apt install nfs-kernel-server`

&nbsp;

NFS can be configured using the **/etc/exports** file, which defines the directories to be shared along with access permissions for users and systems. This file also allows customization of settings such as transfer speed and encryption.

NFS access rights determine which users and systems can interact with shared directories and what actions they are permitted to perform. Below are key access permissions that can be configured in NFS:

| **Permission** | **Description** |
| --- | --- |
| **rw** | Grants read and write access to the shared directory. |
| **ro** | Provides read-only access to the shared directory. |
| **no_root_squash** | Allows the root user on the client to retain full root privileges. |
| **root_squash** | Restricts the root user on the client to standard user privileges. |
| **sync** | Ensures data is written to disk before being transferred, preventing data loss. |
| **async** | Transfers data asynchronously, improving speed but potentially causing inconsistencies if changes are not fully committed. |

&nbsp;

https://www.youtube.com/watch?v=zmDIfJtCKCk&t=40s

&nbsp;

**EXAMPLE:**

```bash
mkdir ~/nfs_sharing  # Create a directory to share via NFS

echo '~/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports  # Add the NFS share configuration to /etc/exports

cat /etc/exports | grep -v "#"  # Display active NFS shares (excluding comments)

exportfs -a  # Export the shared directory

systemctl restart nfs-kernel-server  # Restart the NFS service to apply changes

mkdir ~/nfs_sharing_mount  # Create a local directory to mount the NFS share

mount 127.0.0.1:~/nfs_sharing ~/nfs_sharing_mount  # Mount the NFS share locally (replace 127.0.0.1 with actual server IP if needed)

tree ~/nfs_sharing_mount  # List the contents of the mounted NFS share
```

&nbsp;

We've successfully mounted the NFS share, `dev_scripts`, from the target system (10.129.12.17) to our local system at the `target_nfs` mount point. This allows us to browse the share's contents as if we were directly on the target machine. Furthermore, certain techniques exist that could potentially leverage NFS to escalate privileges on the remote system.

&nbsp;

**Apache Configuration**

```txt
<Directory /var/www/html>  
        Options Indexes FollowSymLinks  
        AllowOverride All  
        Require all granted  
</Directory>
```

This configuration defines access rules for the `/var/www/html` directory. It allows directory listing (`Indexes`), enables symbolic link following (`FollowSymLinks`), permits file modifications using `.htaccess` (`AllowOverride All`), and grants unrestricted access to all users (`Require all granted`).

For instance, if we need to transfer files to a target system using a web server, we can place the necessary files in the `/var/www/html` directory and use tools like `wget`, `curl`, or similar applications to download them onto the target system.

Additionally, Apache allows directory-specific settings through the `.htaccess` file, which can be created in the relevant directory. This file enables access control and other configurations without modifying the main Apache configuration file.

To enhance security and functionality, Apache also supports additional modules such as:

- **mod_rewrite** – Enables URL rewriting.
- **mod_security** – Provides a web application firewall.
- **mod_ssl** – Adds support for Secure Sockets Layer (SSL) encryption.

These modules help improve both security and performance when hosting web applications.

&nbsp;

## \*\*Python Web Server

\*\*

```bash
This setup allows you to quickly host files using Python’s built-in web server, making them accessible via a browser for easy file transfers.

# Install Python 3  
sudo apt install python3 -y

# Start a Python web server on the default port (TCP/8000)  
python3 -m http.server

# Start a Python web server and serve files from a specific directory  
python3 -m http.server --directory /home/cry0l1t3/target_files

# Start a Python web server on a custom port (e.g., 443)  
python3 -m http.server 443
```