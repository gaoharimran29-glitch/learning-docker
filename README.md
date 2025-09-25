# Docker Basics Cheat Sheet

A quick reference guide for essential Docker commands.  

---

# What is Docker?

Docker is an **open-source platform** that allows developers to:
- Build
- Ship
- Run applications  
…inside **lightweight, portable containers**.

Containers package an application with its dependencies (libraries, binaries, config files) so that it runs **consistently across different environments** (developer laptop → testing → production).

Think of it as:  
> "A box that has everything your app needs, so it runs anywhere, without saying *it works on my machine*."

---

# Why is Docker Needed?

Problems without Docker:
- Works on developer’s laptop but fails in production.
- Different OS dependencies (Linux, Windows, Mac).
- Conflicts between project versions (e.g., Python 3.8 vs Python 3.11).
- Heavy Virtual Machines slow down testing.

Solutions with Docker:
- **Portability**: Runs anywhere.
- **Consistency**: Same environment everywhere.
- **Speed**: Containers start in seconds.
- **Efficiency**: Uses fewer resources than full VMs.
- **Scalability**: Perfect for cloud and microservices.

---

# Who Owns Docker?

- **Originally created**: By *Solomon Hykes* in **2013** at a company called **dotCloud**.  
- Later renamed to **Docker Inc.**  
- **Now**: Docker Inc. maintains the **Docker Desktop, CLI, Docker Hub (registry)**, and ecosystem.  
- **Open Source**: The core runtime technology (*containerd*, *runc*) is part of the **CNCF (Cloud Native Computing Foundation)**.  
- Community + Company both contribute.

---

# Docker Architecture

Docker uses a **client-server model**:

1. **Docker Client (CLI)**  
   - The command you type: `docker run ubuntu`  
   - Sends requests to the Docker daemon.

2. **Docker Daemon (dockerd)**  
   - Runs on the host machine.  
   - Responsible for building, running, and managing containers.

3. **Docker Images**  
   - Read-only templates used to create containers.  
   - Example: `ubuntu:20.04`

4. **Docker Containers**  
   - Running instances of images.  
   - Lightweight and isolated.

5. **Docker Registry (Docker Hub)**  
   - Stores and distributes images.  
   - You can pull/push images here.


---

# Docker vs Virtual Machines (VMs)

| Feature              | Docker (Containers)                              | Virtual Machines (VMs)                  |
|----------------------|--------------------------------------------------|------------------------------------------|
| **Isolation**        | Process-level isolation (shared OS kernel)       | Full OS isolation (separate kernel)      |
| **Startup Time**     | Seconds                                          | Minutes                                  |
| **Resource Usage**   | Lightweight (MBs)                                | Heavy (GBs)                              |
| **Portability**      | Very portable (runs anywhere)                    | Less portable (hypervisor-dependent)     |
| **Performance**      | Near-native performance                          | Slower (extra OS overhead)               |
| **Best For**         | Microservices, DevOps, CI/CD, cloud apps         | Monolithic apps, legacy systems          |
| **Example**          | Run 100s of containers on one machine            | Run a few VMs due to heavy usage         |

📌 Simple Analogy:  
- **VM = Full House** (with kitchen, bedroom, bathroom each time)  
- **Docker = Flat in an Apartment** (shares infrastructure, but still isolated)

---

# Summary

- **Docker = container platform** → lightweight, portable, fast.  
- **Needed** to solve "works on my machine" problem and enable DevOps/MLOps.  
- **Owned by Docker Inc.**, founded by Solomon Hykes (2013).  
- **Architecture**: Client → Daemon → Images → Containers → Registry.  
- **Profit**: Subscriptions + enterprise features.  
- **Docker vs VM**: Containers are faster, lighter, more scalable.  

---

##  1. Docker Setup & Info
```bash
docker --version            # Check Docker version
docker info                 # Show system-wide info
docker help                 # Show help for Docker
```
## 2. Images

```bash
docker images               # List all images
docker pull <image>         # Download image (e.g. docker pull ubuntu)
docker rmi <image_id>       # Remove image
docker build -t myimage .   # Build image from Dockerfile
docker tag <img> <repo:tag> # Tag an image
```

## 3. Containers

```bash
docker ps                   # List running containers
docker ps -a                # List all containers
docker run <image>          # Run container
docker run -it <image> bash # Run with interactive shell
docker stop <id>            # Stop container
docker start <id>           # Start stopped container
docker restart <id>         # Restart container
docker rm <id>              # Remove container
```
