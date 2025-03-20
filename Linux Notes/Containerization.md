---
title: Containerization
updated: 2025-03-20 01:42:01Z
created: 2025-03-15 21:00:12Z
---

# **Containerization**

- **Definition**: Packaging and running applications in isolated environments called containers.
- **Benefits**: Lightweight, consistent, portable, and scalable.
- **Technologies**: Docker, Docker Compose, Linux Containers (LXC).
- **Difference from VMs**: Containers share the host system’s kernel, making them more efficient.
- **Use case analogy**: Like portable "stage pods" for a concert, providing isolated yet lightweight setups.

### **Security**

- **Isolation**: Containers prevent applications from interfering with each other or the host system.
- **Risks**: Containers have lower isolation than VMs, making privilege escalation or container escapes possible.
- **Mitigation**: Proper configuration and security hardening are necessary.

### **Advantages**

- **Consistency**: Containers encapsulate dependencies, ensuring the same behavior across environments.
- **Efficiency**: Multiple containers can run simultaneously on the same host with minimal overhead.
- **Scalability**: Ideal for microservices and cloud-native applications.

### **Docker**

- **Definition**: Open-source platform for automating application deployment via containers.
- **Key Features**: Layered filesystem, resource isolation, flexibility, and portability.
- **Analogy**: Like a sealed lunchbox—each container is self-contained, and new ones are created from a recipe (Dockerfile).
- **Management Tools**: Kubernetes, Docker Compose for orchestrating multiple containers.

&nbsp;

**Why use Docker for file hosting?**

- Docker containers are lightweight and easy to deploy.
- Apache provides a simple way to serve files over HTTP.
- The SSH server allows secure file transfer (`scp`) to the container.

#### Docker Management

Some of the most commonly used Docker management commands are:

| **Command** | **Description** |
| --- | --- |
| `docker ps` | List all running containers |
| `docker stop` | Stop a running container. |
| `docker start` | Start a stopped container. |
| `docker restart` | Restart a running container. |
| `docker rm` | Remove a container. |
| `docker rmi` | Remove a Docker image. |
| `docker logs` | View the logs of a container. |

&nbsp;

Docker provides a powerful and flexible way to run applications in containers. By using various options with Docker commands, you can customize your containers to meet specific needs. Some key functionalities include:

- **Exposing Ports:** Allowing external access to services inside the container.
- **Mounting Volumes:** Ensuring data persists even if the container is stopped or removed.
- **Setting Environment Variables:** Configuring applications inside the container dynamically.

#### **Working with Docker Images and Containers**

When you start a container from a Docker image, any changes you make inside that container (like installing new software or modifying files) **are not automatically saved** to the original image. If you want to keep those changes, you need to create a **new Docker image** that includes them.

This is done using a **Dockerfile**, which defines how the new image is built. A Dockerfile typically includes:

1.  **A Base Image:** Defined using `FROM <base-image>`, which sets the foundation for your new image.
2.  **Commands to Modify the Image:** Such as `RUN` to install software, `COPY` to add files, or `ENV` to set environment variables.
3.  **Building the Image:** Running `docker build -t <new-image-name> .` creates the updated image with a unique tag.

This approach ensures that the **original image remains unchanged**, while the new image incorporates your modifications.

#### **Why Persistence Matters in Docker**

By default, Docker containers are **stateless**, meaning that any data changes made inside a running container **will be lost** when the container stops or is deleted. To prevent data loss, use **volumes** or **bind mounts**, which store data outside the container on the host machine. This is crucial for applications that require data persistence, such as databases.

#### **Managing Docker Containers at Scale**

In production environments, running multiple containers efficiently can be challenging. Tools like **Docker Compose** and **Kubernetes** help simplify this process by:

- **Docker Compose:** Defining multi-container applications in a `docker-compose.yml` file, making it easy to start multiple services together.
- **Kubernetes:** A more advanced container orchestration tool for managing, scaling, and automating container deployments across multiple servers.

Using these tools helps ensure your containers run reliably, scale effectively, and integrate seamlessly in a production setting.

# **Understanding Linux Containers (LXC)**

Video: https://www.youtube.com/watch?v=aIwgPKkVj8s&t=1147s

Linux Containers (LXC) is a lightweight virtualization technology that enables multiple **isolated Linux systems** (containers) to run on a single host. It achieves this by using **kernel features** like:

- **Control groups (cgroups)** – To manage resource allocation (CPU, memory, etc.).
- **Namespaces** – To isolate processes, networks, and filesystems between containers.

Unlike traditional **virtual machines (VMs)**, which require a full OS for each instance, LXC **shares the host’s kernel**, making it much more efficient in terms of resource usage.

LXC is commonly used to create **system-level** containers, which behave like lightweight virtual machines. It provides powerful tools and APIs for managing these containers, making it a preferred choice for Linux system administrators.

* * *

### **LXC vs. Docker: Key Differences**

While both LXC and Docker are containerization technologies, they serve different purposes. Below is a breakdown of their key differences:

| **Category** | **LXC** | **Docker** |
| --- | --- | --- |
| **Approach** | Focuses on **system-level** containerization, creating isolated Linux environments that behave like lightweight VMs. | Focuses on **application-level** containerization, packaging single applications or microservices. |
| **Image Building** | Requires **manual setup** to create a root filesystem and install dependencies. | Uses **Docker images**, built using a **Dockerfile**, which automates the process. |
| **Portability** | Less portable, as LXC containers are closely tied to the host system's configuration. | Highly portable—Docker images can be shared via **Docker Hub** or private registries. |
| **Ease of Use** | Requires **advanced Linux knowledge** for configuration and management. | Designed for simplicity, with an easy-to-use **CLI** and strong community support. |
| **Security** | Provides isolation but may require additional configuration for strong security. | Offers built-in security features (e.g., **AppArmor, SELinux, read-only filesystems**) for better out-of-the-box protection. |

* * *

### **How Docker Improves on LXC**

Docker **builds on LXC** but enhances it with better usability, portability, and automation. Here’s how:

1.  **Automated Image Creation** – Docker uses a **Dockerfile** to define and build container images, making them easy to recreate and distribute.
2.  **Application-Centric Design** – Instead of running full Linux systems, Docker containers package **only the necessary dependencies** for an application.
3.  **Cross-Platform Portability** – Docker images can run on any system with Docker installed, making it ideal for **CI/CD pipelines** and cloud deployments.
4.  **Built-In Orchestration** – Docker integrates with tools like **Docker Compose** and **Kubernetes** to manage multiple containers efficiently.

* * *

- **Use LXC** when you need full system containers that behave like lightweight VMs, giving more control over the OS and system settings.
- **Use Docker** when you need easy-to-deploy, **portable** application containers with minimal overhead.

**LXC is ideal for system-level virtualization**, while **Docker is optimized for deploying applications in isolated, lightweight environments**.

&nbsp;

## Essential LXC Commands

| Command | Description |
| --- | --- |
| `lxc-ls` | List all existing containers. |
| `lxc-stop -n <container>` | Stop a running container. |
| `lxc-start -n <container>` | Start a stopped container. |
| `lxc-restart -n <container>` | Restart a running container. |
| `lxc-config -n <container> -s storage` | Manage container storage settings. |
| `lxc-config -n <container> -s network` | Manage container network settings. |
| `lxc-config -n <container> -s security` | Manage container security settings. |
| `lxc-attach -n <container>` | Connect to a container. |
| `lxc-attach -n <container> -f /path/to/share` | Connect to a container and share a specific directory or file. |

### Why Use LXC for Penetration Testing?

Penetration testers often need to test software and systems with complex dependencies that are difficult to replicate on a local machine. LXC containers provide an isolated, lightweight environment that includes everything required to run specific software, such as code, libraries, and configurations. This ensures consistent execution across different Linux machines, regardless of the host system's setup.

#### Benefits of Using LXC in Testing:

1.  **Controlled Testing Environments:**
    
    - Quickly set up environments with specific dependencies (e.g., a web server and database).
    - Avoid manually configuring test systems, reducing errors and setup time.
2.  **Safe Exploit and Malware Testing:**
    
    - Create isolated containers to simulate vulnerable systems.
    - Test exploits in a secure environment without risking the host machine.

&nbsp;

### Enhancing LXC Security

Since LXC containers share the same kernel as the host system, proper security measures are essential to prevent unauthorized access or malicious activities.

#### Best Practices for Securing LXC Containers:

- **Restrict Access:**
    
    - Disable unnecessary services.
    - Use secure protocols and strong authentication.
    - Limit SSH access by removing `openssh-server` or allowing only trusted IPs.
- **Limit Resource Usage:**
    
    - Use cgroups to restrict CPU, memory, and disk usage.
    - Prevent excessive resource consumption that could impact the host system.
- **Isolate Containers from the Host:**
    
    - Apply security policies to prevent privilege escalation.
    - Use network isolation to restrict communication between containers.
- **Keep Containers Updated:**
    
    - Regularly update container software and security patches

&nbsp;

# **Securing LXC Containers**

## **Limiting Container Resources**

To prevent a container from consuming excessive system resources, we can use **cgroups (Control Groups)** to limit CPU and memory usage. This is done by configuring a container’s resource allocation settings in its configuration file.

### **Creating a Configuration File for Resource Limits**

To set CPU and memory limits for a container named `linuxcontainer`, create a new configuration file in the `/usr/share/lxc/config/` directory:

`sudo vim /usr/share/lxc/config/linuxcontainer.conf`

Inside this file, add the following lines to define CPU and memory constraints:

`lxc.cgroup.cpu.shares = 512lxc.cgroup.memory.limit_in_bytes = 512M`

### **Understanding Resource Parameters**

- **`lxc.cgroup.cpu.shares`**
    
    - Controls the CPU time a container can use relative to others.
    - The default value is `1024` (full access). Setting it to `512` reduces CPU access to **half**.
- **`lxc.cgroup.memory.limit_in_bytes`**
    
    - Defines the maximum memory a container can use.
    - Accepts multiple units: `bytes`, `K` (kilobytes), `M` (megabytes), `G` (gigabytes), `T` (terabytes).

### **Applying the Changes**

After editing the configuration file, save and exit:

1.  Press `Esc`
2.  Type `:wq` and hit `Enter`

Then, restart the LXC service to apply the changes:

`sudo systemctl restart lxc.service`

* * *

## **LXC Namespaces and Isolation**

LXC uses **Linux namespaces** to create isolated environments for processes, networks, and file systems, preventing interference between containers and the host system. Namespaces, a key feature of the Linux kernel, abstract system resources to ensure separation between containers.

### **Key Aspects of Namespace Isolation**

1.  **Process Isolation (`pid` namespace)**
    
    - Each container has its own process ID (PID) space, separate from the host.
    - This prevents container processes from interfering with or accessing host processes, improving system stability.
2.  **Network Isolation (`net` namespace)**
    
    - Containers have their own network interfaces, routing tables, and firewall rules.
    - Network activity inside a container is fully separated from the host, enhancing security.
3.  **File System Isolation (`mnt` namespace)**
    
    - Each container operates with an independent root filesystem.
    - Changes made inside a container do **not** affect the host system’s filesystem.

### **Limitations and Additional Security Measures**

While namespaces provide a high level of isolation, they do **not** guarantee complete security. To further protect both the host and containers, additional security measures should be implemented, such as:

- **Restricting container privileges** to prevent unauthorized access.
- **Applying resource limits** to avoid excessive CPU, memory, or disk usage.
- **Using SELinux or AppArmor** to enforce mandatory access controls.
- **Regularly updating container configurations** to patch vulnerabilities.

By combining **namespace isolation** with strong security policies, we can create a **more secure and reliable containerized environment**.