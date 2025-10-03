# Table of Contents

- [Docker Basics Cheat Sheet](#docker-basics-cheat-sheet)
- [What is Docker?](#what-is-docker)
- [Why is Docker Needed?](#why-is-docker-needed)
- [Who Owns Docker?](#who-owns-docker)
- [Docker Architecture](#docker-architecture)
- [Docker vs Virtual Machines (VMs)](#docker-vs-virtual-machines-vms)
- [Summary](#summary)
- [Docker Volumes & Networks – Commands Reference](#docker-volumes--networks--commands-reference)
- [DOCKER FILE](#docker-file)
- [1. Basic Rules for Writing Dockerfile](#1-basic-rules-for-writing-dockerfile)
- [2. Common Instructions](#2-common-instructions)
- [3. Simple Dockerfile Example](#3-simple-dockerfile-example)
- [Docker Compose Notes](#docker-compose-notes)

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

![Docker-Architecture](/dockervsvm.png)

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

Simple Analogy:  
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

```dockerfile
docker --version            # Check Docker version
docker info                 # Show system-wide info
docker help                 # Show help for Docker
```
## 2. Images

```dockerfile
docker images               # List all images
docker pull <image>         # Download image (e.g. docker pull ubuntu)
docker rmi <image_id>       # Remove image
docker build -t myimage .   # Build image from Dockerfile
docker tag <img> <repo:tag> # Tag an image
```

## 3. Containers

```dockerfile
docker ps                   # List running containers
docker ps -a                # List all containers
docker run <image>          # Run container
docker run -it <image> bash # Run with interactive shell
docker stop <id>            # Stop container
docker start <id>           # Start stopped container
docker restart <id>         # Restart container
docker rm <id>              # Remove container
docker logs <containerid>   # To see all the logs of containers
docker logs <containerid>   # To see the logs continuosly at runtime
```

# Docker Volumes & Networks – Commands Reference

# DOCKER VOLUMES

```dockerfile
docker volume ls               # List all volumes
docker volume create myvol     # Create a named volume
docker volume inspect myvol    # Inspect volume details
docker run -v myvol:/data ubuntu           # Run container with volume
docker run -v $(pwd):/app ubuntu           # Run container with host directory bind mount
docker volume rm myvol        # Remove a volume
docker volume prune            # Remove all unused volumes
```

# DOCKER NETWORKS

```dockerfile
docker network ls              # List all networks
docker network create mynet    # Create a network
docker network create --driver bridge mybridge   # Create a bridge network
docker network create --driver overlay myoverlay # Create an overlay network
docker network inspect mynet   # Inspect network details
docker run -d --name c1 --network mynet nginx   # Run container in a network
docker run -d --name c2 --network mynet alpine ping c1  # Run another container and ping first container
docker network connect mynet c3      # Connect container c3 to network
docker network disconnect mynet c3   # Disconnect container c3 from network
docker network rm mynet              # Remove a network
docker network prune                  # Remove all unused networks
```

# DOCKER FILE

Dockerfile is a **text file** that contains instructions to **build a Docker image**.  
It automates creating images with your app and its dependencies, ensuring **consistency and portability**.

---

# 1. Basic Rules for Writing Dockerfile

1. **Use valid instructions**: `FROM`, `RUN`, `CMD`, `LABEL`, `EXPOSE`, `ENV`, `ADD`, `COPY`, `ENTRYPOINT`, `WORKDIR`
2. **Case-insensitive**: Instructions are not case-sensitive but uppercase is convention.
3. **One instruction per line**: Each line creates a layer.
4. **Comments**: Use `#` for comments.
5. **FROM required**: Every Dockerfile must start with a base image (or `scratch`).
6. **Minimize layers**: Combine commands with `&&`.
7. **Keep it readable**: Meaningful names, organized structure.

---

# 2. Common Instructions

| Instruction | Purpose |
|-------------|---------|
| `FROM <image>` | Base image |
| `RUN <command>` | Run command during build |
| `CMD ["executable","param"]` | Default command for container |
| `ENTRYPOINT ["executable"]` | Entrypoint, less overrideable |
| `COPY <src> <dest>` | Copy files from host to image |
| `ADD <src> <dest>` | Like COPY but supports URLs and tar files |
| `WORKDIR <path>` | Set working directory |
| `ENV <key>=<value>` | Environment variable |
| `EXPOSE <port>` | Declare port to expose |
| `LABEL <key>=<value>` | Metadata for image |

---

# 3. Simple Dockerfile Example

**Scenario:** Python Flask app

```dockerfile
# Step 1: Base image
FROM python:3.11

# Step 2: Set working directory
WORKDIR /app

# Step 3: Copy dependency file
COPY requirements.txt .

# Step 4: Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Step 5: Copy app code
COPY . .

# Step 6: Expose port
EXPOSE 5000

# Step 7: Default command
CMD ["python", "app.py"]
```

# Docker Compose Notes

## What is Docker Compose?
Docker Compose is a tool to **define and run multi-container Docker applications**.  
Instead of starting each container manually with long `docker run` commands, you define everything in a **YAML file** (`docker-compose.yml`) and run with a single command.

---

## Why Use Docker Compose?
- Start/stop multiple containers with **one command**.
- Easy to configure **networking** between containers.
- Manage **volumes** and persistent data.
- Run containers in **different environments** (dev, test, prod).
- Makes your environment **reproducible**.

---

## Does Docker Compose Replace Dockerfile?
❌ **No.**  
- **Dockerfile** → Defines **how to build an image**.  
- **Docker Compose** → Defines **how to run one or more containers** from those images.  

You often use **both together**:  
- First create a `Dockerfile` to define your app.  
- Then use `docker-compose.yml` to run it (with databases, caching, etc.).

---

## 📌 Logic Behind Writing docker-compose.yml

A `docker-compose.yml` file has three main parts:

1. **Version**  
   - Specifies which Compose file format you are using.  
   - Example: `version: '3.8'`

2. **Services**  
   - Each service = one container.  
   - Define its image, command, ports, volumes, environment, dependencies, etc.  
   - Example: `web`, `db`, `redis`.

3. **Other Configurations**  
   - **Volumes** → Persist data.  
   - **Networks** → Allow services to communicate.  
   - **Depends_on** → Define startup order.  

👉 **Example (Single Service)**  
```yaml
version: '3.8'
services:
  web:
    image: python:3.9
    command: python -m http.server 8000
    ports:
      - "8000:8000"
```

## Structure of docker-compose.yml

```yaml
version: '3.8'       # Compose file version
services:            # Define your services (containers)

  web:               # Service name
    image: python:3.9         # Image to use
    command: python -m http.server 8000  # Command to run inside container
    ports:
      - "8000:8000"           # Host:Container port mapping
    volumes:
      - .:/app                # (Optional) Mount current folder to container
    environment:
      - DEBUG=true            # (Optional) Env variables

  db:                # Another service (example: MySQL database)
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_DATABASE=mydb
      - MYSQL_USER=user
      - MYSQL_PASSWORD=pass
    ports:
      - "3306:3306"
```
## Basic Example
```yaml
version: '3.8'
services:
  web:
    image: python:3.9
    command: python -m http.server 8000
    ports:
      - "8000:8000"
```

## Multi-service example
```yaml
version: '3.8'
services:

  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_DATABASE=mydb
      - MYSQL_USER=user
      - MYSQL_PASSWORD=pass
    ports:
      - "3306:3306"
```

## File Naming
| File Name | Description |
|-----------|-------------|
| `docker-compose.yml` | Default name for Docker Compose configuration. Compose automatically looks for this file if no file is specified. |
| `docker-compose.override.yml` | Optional file to override settings from the main `docker-compose.yml`. |
| Custom names | You can use any custom file name with `-f <filename>` option: `docker-compose -f custom.yml up`. |

---

## Important Commands

| Command | Description | Example |
|---------|------------|---------|
| `docker-compose up` | Starts all services defined in the compose file | `docker-compose up` |
| `docker-compose up -d` | Starts services in detached mode (background) | `docker-compose up -d` |
| `docker-compose down` | Stops and removes all containers, networks, and volumes created by `up` | `docker-compose down` |
| `docker-compose stop` | Stops running containers without removing them | `docker-compose stop` |
| `docker-compose start` | Starts previously stopped containers | `docker-compose start` |
| `docker-compose restart` | Restarts services | `docker-compose restart` |
| `docker-compose build` | Builds or rebuilds services | `docker-compose build` |
| `docker-compose logs` | View output logs from services | `docker-compose logs` |
| `docker-compose ps` | Lists running containers | `docker-compose ps` |
| `docker-compose exec` | Execute a command in a running container | `docker-compose exec web bash` |
| `docker-compose pull` | Pull images defined in compose file | `docker-compose pull` |

---

## Advantages of Docker Compose

- **Multi-container orchestration**: Easily run multiple services together.  
- **Simplified configuration**: Use one YAML file instead of multiple `docker run` commands.  
- **Environment management**: Supports variables and different environments using `.env` files.  
- **Portability**: Share the compose file, and anyone can run the same environment.  
- **Scalability**: Easily scale services using `docker-compose up --scale <service>=<num>`.  
- **Consistent workflow**: Same commands work in development, testing, and production.  
