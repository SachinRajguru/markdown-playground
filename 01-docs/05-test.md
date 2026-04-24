
# 🐳 Docker : Complete Practical Lab & Technical Guide

## Table of Contents
1. [What is Docker? Why Do We Need It?](#1-what-is-docker-why-do-we-need-it)
2. [Docker Images & Containers](#2-docker-images--containers-core-concepts)
3. [Installation of Docker CLI & Desktop](#3-installation-of-docker-cli--desktop)
4. [Important Docker Commands](#4-important-docker-commands)
5. [Docker vs Virtual Machine](#5-docker-vs-virtual-machine)
6. [Port Mapping & Setting Environment Variables](#6-port-mapping--setting-environment-variables)
7. [Troubleshooting Containers](#7-troubleshooting-containers)
8. [Using Containers to Build Node Application](#8-using-containers-to-build-node-application)
9. [Dockerization of Node.js Application (Dockerfile)](#9-dockerization-of-nodejs-application-dockerfile)
10. [Docker Compose (Multi-Container Apps)](#10-docker-compose-multi-container-apps)
11. [Publishing to DockerHub](#11-publishing-to-dockerhub)
12. [Layering in Docker Images](#12-layering-in-docker-images)
13. [Volume Mounting](#13-volume-mounting)
14. [Docker Networking](#14-docker-networking)

---

## 1. What is Docker? Why Do We Need It?

### Real-World Problem: "It Works on My Machine!"

**Scenario**: You're working in a team on a Node.js application. Your team has 5 developers, and a new developer joins with a Mac system.

```bash
Team Member 1 (Linux): Node v16, Mongo v4.2
Team Member 2 (Windows): Node v18, Mongo v6 
New Developer (Mac): Installs Node v20, Mongo v7 (latest versions)
```

**Problems Encountered**:

1. **Manual Errors**: Installing 20+ dependencies one-by-one is error-prone
2. **Version Conflicts**: App requires Node v16, but new dev installed v20
3. **OS Differences**: Commands work on Linux but fail on Mac/Windows
4. **Scale Issues**: With 50+ developers, manual setup becomes impossible
5. **Production Deployment**: Same issues when deploying to servers

**Classic Problem**: *"It works on my machine, but not on yours!"*

### Docker Solution
Docker creates **containers** - single, portable packages containing:
```bash
Your App Code + Dependencies + Exact Environment
```

**Analogy**: 
Think of containers as pre-packed lunch boxes. Everything your app needs is inside - no more "I forgot the Redis version!"

**Key Benefits**:
```bash
✓ Portable: Runs anywhere (Mac/Windows/Linux)
✓ Lightweight: MBs vs GBs (compared to VMs)  
✓ Isolated: Run Node v16 + v20 simultaneously
✓ Fast: Instant start/stop
✓ Shareable: Share image, everyone builds identical container
```

### Interview Q&A

**Q: Why Docker over manual environment setup?**

**A**: Eliminates "works on my machine" by packaging app + exact dependencies into portable containers. Scales to 100s of developers without manual errors.

---

## 2. Docker Images & Containers (Core Concepts)

### Definition: Docker Container
```bash
Container = App Code + Dependencies + Runtime Environment
Packaged as a single, executable unit
```

### Definition: `Docker Image` ⚠️ NOT a photo!
```bash
Image = Blueprint/Recipe/Executable Instructions
Tells Docker "HOW to build a container"
```

**Class vs Object Analogy**:
```bash
Docker Image    = Class (Blueprint)  
Docker Container = Object (Instance)

1 Image → Multiple Containers
```

**Real Example**:
```bash
Image: "Node v16 + Express + Mongo v4.2"
Container 1: Dev1 runs this on Mac
Container 2: Dev2 runs this on Windows  
Container 3: Production server
```

**Workflow**:
```bash
1. Build Image ✓
2. Share Image with team (Docker Hub) ✓
3. Everyone creates Container from Image ✓
```

### Practical Demo
```bash
# Run Ubuntu container interactively
docker run -it ubuntu

# Inside container (isolated from host Mac):
root@abc123:/# mkdir test-folder     # Only exists in container
root@abc123:/# ls                    # See Ubuntu files only
root@abc123:/# exit                  # Container stops
```

**Key Insight**: Container changes are **isolated** from host OS.

### Interview Q&A

**Q: Image vs Container?**

**A**: `Image` = static blueprint (no resources). `Container` = running instance (uses resources). 1:Many relationship.

---

## 3. Installation of Docker CLI & Desktop

### Step-by-Step Installation

#### Mac Installation
1. Visit [docker.com](https://docker.com)
2. Download **Docker Desktop** (Intel/Apple Silicon)
3. Install → Restart system
4. **Verify**:
```bash
docker --version    # Docker version 27.5.1
docker              # Lists all command
```

#### Windows Installation
1. Download Docker Desktop (Windows AMD64)
2. Run installer → Accept admin permissions
3. Restart system
4. **Post-install Setup**:
   - Accept agreement
   - Choose **Recommended settings**
   - Select **Full Stack Developer**
   - Skip login (optional)

#### Verification Commands
```bash
# Check Docker is working
docker --version
docker              # Lists all commands

# Test installation
docker run hello-world
```

**Docker Desktop UI**: Shows Containers/Images/Volumes/Networks tabs.
```bash
📁 Images Tab: All downloaded images
🐳 Containers Tab: Running/stopped containers
```

### **Docker Hub** (Like GitHub for Images)
- https://hub.docker.com/
Public registry with 100,000+ pre-built images

### Interview Q&A

**Q: Docker CLI vs Docker Desktop?**

**A**: `CLI` = command-line interface for automation. `Desktop` = GUI for visualization/debugging.

---

## 4. Important Docker Commands

### Lab 1: Hello World Container

**Step 1: Pull First Image**
```bash
# Terminal command
docker pull hello-world
```
**What happens**:
```bash
1. Docker client contacts Docker daemon
2. Downloads hello-world image from Docker Hub  
3. Image appears in Docker Desktop → Images tab
4. Size: ~13KB (super lightweight!)
```

**Step 2: Run Container**
```bash
docker run hello-world
```
**Output**:
```bash
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

**Docker Desktop shows**:
```bash
Containers Tab → New container created
Status: Exited (container ran, then stopped)
Random name assigned (festive_fermi)
```

**Behind the Scenes**:
```bash
Docker Client → Docker Daemon → Pull Image → Create Container → Run → Print Output → Exit
```

### Lab 2: Interactive Ubuntu Container

**Step 3: Run Interactive Mode**
```bash
docker run -it ubuntu
```
```bash
-it flags:
-i = Interactive (keep STDIN open)
-t = Allocate TTY (terminal)
```

**Inside Container** (you're now in Ubuntu!):
```bash
# List files
ls

# Create directory  
mkdir my-project
ls

# Create file
touch hello.txt
ls
```

**Exit Container**:
```bash
exit
```

**Stop Container**:
```bash
# Copy Container ID from Docker Desktop
docker stop <container-id>
# OR use name
docker stop festive_fermi
```

**Status Check**:
```bash
Docker Desktop → Container shows "Exited"
```

### Interview Q&A

**Q: docker run vs docker run -it?**

**A**: `docker run` = run & auto-exit. `docker run -it` = interactive shell inside container.

### Lab 3: Container Lifecycle
```bash
# List ALL containers (running + stopped)
docker ps -a

# List RUNNING containers only
docker ps

# Start existing container
docker start <container_id/name>

# Stop running container  
docker stop <container_id/name>

# Remove container
docker rm <container_id>

# Remove image (delete containers first)
docker rmi <image_name>
```

### Lab 4: Image Tags/Versions
```bash
# Pull specific version
docker pull mysql:8.0
docker pull mysql:latest

# Run with custom name
docker run --name my-mysql-old -d mysql:8.0

# Run detached (-d = background)
docker run -d --name my-mysql mysql:latest
```

### Cheat Sheet
```bash
docker pull <image>       # Download image
docker images             # List images
docker run <image>        # Create + run container
docker ps -a              # List containers
docker start/stop/rm      # Manage containers
docker rmi <image>        # Delete image
```

### Interview Q&A

**Q: docker run vs docker start?**

**A**: `run` = create NEW container. `start` = restart EXISTING container.

---

## 5. Docker vs Virtual Machine

### **Architecture Comparison**

```Bash
HOST MACHINE (Your Laptop)
┌─────────────────────────────────────┐
│  Kernel (Linux/Windows/Mac)         │
├─────────────────────────────────────┤
│  Docker: Virtualizes APP LAYER ONLY │ ← Lightweight!
│  VM: Virtualizes FULL OS + Kernel   │ ← Heavy!
└─────────────────────────────────────┘
```

**Docker Desktop Magic**:
```bash
Adds lightweight hypervisor layer
Works on Mac/Windows (non-Linux)
```

### Key Differences

| Feature | Docker | VM |
|---------|--------|----|
| **Size** | KB/MB | GB |
| **Speed** | Seconds | Minutes |
| **Isolation** | App layer | Full OS |
| **OS Support** | Same host kernel | Any OS |
| **Overhead** | Minimal | High |

**Docker Advantage**: 
- Shares host kernel → **lightweight**

**Docker Disadvantage**: 
- Linux-focused (Windows/Mac need Docker Desktop)

### Interview Q&A

**Q: Why is Docker faster than VM?**

**A**: Docker shares host kernel, virtualizes only app layer. VMs virtualize entire OS + kernel.

**Q: When would you choose VM over Docker?**

**A**: Need different OS kernel or complete OS isolation (security/banking).

---

## 6. Port Mapping & Setting Environment Variables

**Scenario**: Run a Node.js app inside container, access from browser.

### Port Binding Concept
```bash
Host Machine          Container
  8080 ──────────────→ 3306 (MySQL)
  5000 ──────────────→ 3306 (MySQL v8.0)
```

**Problem**: Container ports (3306) ≠ Host ports (8080, 5000)

**Solution**: `-p host_port:container_port`

### Practical Lab
```bash
# MySQL with port binding + env vars
docker run -d \
  --name mysql-latest \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 8080:3306 \
  mysql:latest

docker run -d \
  --name mysql-old \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 5000:3306 \
  mysql:8.0
```

**Error Case**:
```bash
docker run -p 8080:3306 mysql:8.0  # ❌ Port already allocated
```

### Environment Variables
```bash
# -e = environment variable
docker run -e MYSQL_ROOT_PASSWORD=secret mysql
```

### Interview Q&A

**Q: Why can't two containers bind same host port?**

**A**: Host OS ports are unique. Container ports can be same (isolated).

---

## 7. Troubleshooting Containers

### Essential Debug Commands

```bash
# List all containers (running + stopped)
docker ps -a

# List running containers only
docker ps

# View container logs
docker logs <container_name>

# Access running container shell
docker exec -it <container_name> bash    # or sh

# Example:
docker exec -it mysql-latest bash
root@abc123:/# mysql -u root -p
root@abc123:/# SHOW DATABASES;
```

**Docker Desktop**: Click container → **Logs** tab

### Interview Q&A

**Q: Container crashes on startup. How to debug?**

**A**: 
1. `docker logs <container>` → Check error messages
2. `docker exec -it <container> bash` → Inspect filesystem/env

---

## 8. Using Containers to Build Node Application

### Lab: Node.js + MongoDB Setup

**test-app/server.js** (provided code):
```javascript
const express = require("express");
const app = express();
const path = require("path");
const MongoClient = require("mongodb").MongoClient;

const PORT = 5050;
app.use(express.urlencoded({ extended: true }));
app.use(express.static("public"));

const MONGO_URL = "mongodb://admin:qwerty@localhost:27017";
const client = new MongoClient(MONGO_URL);

//GET all users
app.get("/getUsers", async (req, res) => {
    await client.connect(URL);
    console.log('Connected successfully to server');

    const db = client.db("college-db");
    const data = await db.collection('users').find({}).toArray();
    
    client.close();
    res.send(data);
});

//POST new user
app.post("/addUser", async (req, res) => {
    const userObj = req.body;
    console.log(req.body);
    await client.connect(URL);
    console.log('Connected successfully to server');

    const db = client.db("college-db");
    const data = await db.collection('users').insertOne(userObj);
    console.log(data);
    console.log("data inserted in DB");
    client.close();
});


app.listen(PORT, () => {
    console.log(`server running on port ${PORT}`);
});
```

### Multi-Container Setup
1. **MongoDB Container**: Database
2. **Mongo Express**: GUI (localhost:8081)
3. **Node App**: Runs on localhost:5050

**Network Setup**:
```bash
docker network create mongo-network
docker network ls
```

**Mongo Container**:
```bash
docker run -d \
  --name mongo \
  --network mongo-network \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=qwerty \
  mongo
```

**Mongo Express**:
```bash
docker run -d \
  --name mongo-express \
  --network mongo-network \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017/" \
  mongo-express
```

**Test**: 
- localhost:8081 → Mongo Express UI (admin/pass: admin/qwerty)
- localhost:5050/users → GET users
- POST to /addUser → Creates data

### Interview Q&A

**Q: How does Node app connect to Mongo without localhost?**

**A**: Uses **container name** (`mongo:27017`) as hostname via Docker network.

---

## 9. Dockerization of Node.js Application (Dockerfile)

### Dockerfile Instructions

```bash
FROM node:16            # Base image (Node runtime)

ENV MONGODB_USERNAME=admin
ENV MONGODB_PASSWORD=qwerty

RUN mkdir /test-app     # Create working directory

WORKDIR /test-app       # Set working directory

COPY . /test-app        # Copy app files

RUN npm install         # Installs express + mongodb v6.13.0

CMD ["node", "/test-app/server.js"] # Default command when container starts - Runs your exact file
```

### Build & Run
```bash
# Build image
docker build -t test-app:1.0 .

# Verify
docker images           # See test-app:1.0

# Run container
docker run test-app:1.0

# Interactive
docker run -it test-app:1.0 bash
```

**Result**: Node app runs inside container on port 5050.

### Interview Q&A

**Q: FROM vs COPY vs CMD?**

**A**:
- `FROM`: Base image
- `COPY`: Copy files to container
- `CMD`: Default command when container starts

---

## 10. Docker Compose (Multi-Container Apps)

### docker-compose.yml (Multi-container orchestration)

```yaml
version: '3.8'          # Compose version
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"   # host:container
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty

  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_URL: "mongodb://admin:qwerty@mongo:27017/"
```

### Commands
```bash
# Start all services
docker-compose -f mongodbs.yml up -d

# Stop + remove
docker-compose -f mongodbs.yml down

# View logs
docker-compose -f mongodbs.yml logs
```

**Auto-features**:
- ✓ Default network created
- ✓ Services communicate by name
- ✓ One command for all containers

**Compose Concepts**:
- **Services**: Individual containers
- **Port Mapping**: `HOST:CONTAINER`
- **Env Variables**: `-e KEY=VALUE`
- **Volumes**: Persistent data storage

### Interview Q&A

**Q: docker run vs docker-compose?**

**A**: `run` = single container. `compose` = multi-container apps with dependencies.

---

## 11. Publishing to DockerHub

### Step-by-Step Publishing

1. **Create Repository**:
   - [hub.docker.com](https://hub.docker.com/) → New Repository
   - Name: `devcollege/test-application`
   - Public ✓

2. **Tag & Build**:
```bash
docker build -t devcollege/test-application:1.0 .
```

3. **Login**:
```bash
docker login
# Username: devcollege
# Password: or use browser auth
```

4. **Push**:
```bash
docker push devcollege/test-application:1.0
```

5. **Pull from anywhere**:
```bash
docker pull devcollege/test-application:1.0
docker run devcollege/test-application:1.0
```

### Interview Q&A

**Q: Local image vs DockerHub image naming?**

**A**: Local: `test-app`. Hub: `username/repo:tag`.

---

## 12. Layering in Docker Images

### Layer Caching Demo
```bash
# First pull (downloads ALL layers)
docker pull mysql:latest

# Second pull (reuses common layers)
docker pull mysql:8.0
# Output: "Layer already exists" ✓
```

**Layer Structure**:
```bash
mysql:8.0 image
├── Layer 1: debian (base OS)
├── Layer 2: Node runtime
├── Layer 3: MySQL binaries
└── Layer 4: Config files
```

**Benefit**: Common layers cached → Faster builds.

### **How Layers Work (Dockerfile Example)**
```dockerfile
# EACH instruction = NEW LAYER
FROM ubuntu:20.04      # Layer 1: Base OS
RUN apt update         # Layer 2: Package cache
RUN apt install -y curl # Layer 3: curl installed
COPY app.js /app/      # Layer 4: Your code
RUN npm install        # Layer 5: node_modules
CMD ["node", "app.js"] # Layer 6: Startup command
```

**Caching Magic**:
```bash
# Build 1: Downloads ALL layers (slow)
# Build 2: If COPY unchanged → Reuses layers 1-3 (FAST!)
# Only rebuilds from changed instruction
```

### **Best Practices for Layer Optimization**
```dockerfile
# ✓ GOOD: Group RUN commands (fewer layers)
RUN apt update && \
    apt install -y curl nodejs && \
    rm -rf /var/lib/apt/lists/*

# ✗ BAD: Separate RUN = More layers = Bigger image
RUN apt update
RUN apt install curl
RUN apt install nodejs
```

### Interview Q&A

**Q: Why Docker images have multiple layers?**

**A**: Layer caching. Reused layers across versions/images → faster pulls/builds.

**Q: How to optimize Dockerfile layers?**

**A**: Group `RUN` commands, copy `package.json` first, use `.dockerignore`.

---

## 13. Volume Mounting

### Data Persistence Problem
```bash
Without Volume: Container stops → Data LOST
With Volume: Container stops → Data PERSISTS
```

### Volume Types

| Type | Syntax | Managed By | Use Case |
|------|--------|------------|----------|
| **Bind Mount** | `-v /host/path:/container/path` | Host OS | Development |
| **Named Volume** | `docker volume create myvol`<br>`-v myvol:/container/path` | Docker | Production |
| **Anonymous** | `-v /container/path` | Docker | Temp storage |

### Practical Lab
```bash
# 1. Create Named Volume
docker volume create mongo-data

# 2. Bind Mount (host:container)
docker run -it \
  -v /Users/abc/Desktop/data:/test-data \
  ubuntu

# Inside container:
cd /test-data
touch index.html server.js
ls                           # Files visible

# Host Desktop/data → Same files appear!
```

**MongoDB with Volume**:
```bash
docker run -d \
  --name mongo-persistent \
  -v mongo-data:/data/db \
  -p 27017:27017 \
  mongo
```

**Docker Compose Volume**:
```yaml
version: '3.8'
services:
  mongo:
    image: mongo
    volumes:
      - mongo-data:/data/db  # Persist Mongo data
      - ./host-backup:/backup  # Bind mount
volumes:
  mongo-data:  # Named volume definition
```

### **Volume Commands**
```bash
docker volume ls           # List volumes
docker volume inspect mongo-data
docker volume rm mongo-data
```

### Interview Q&A

**Q: Container deleted but data persists?**

**A**: Volumes are **external** to containers. Data lives in Docker-managed storage.

**Q: Bind mount vs Named volume?**

**A**: `Bind` = host filesystem (dev). `Named` = Docker managed (prod).

---

## 14. Docker Networking

### Default Networks
```bash
docker network ls
# bridge   (default)
# host     
# none
```

### Network Drivers

| Driver | Use Case | Isolation |
|--------|----------|-----------|
| **Bridge** | Multi-container apps on same host | App-level |
| **Host** | Container uses host network | None |
| **None** | Complete isolation | Full |

### Custom Bridge Network
```bash
# Create
docker network create mongo-network

# Run containers in network
docker run --network mongo-network --name mongo mongo
docker run --network mongo-network --name mongo-express mongo-express

# Containers communicate by NAME
# mongo-express → mongo:27017 (no localhost!)
```

**Test Network Communication**:
```bash
# Inside mongo-express container
docker exec -it mongo-express sh
nslookup mongo    # Resolves to container IP
ping mongo        # Network connectivity ✓
```

**Docker Compose**: Auto-creates bridge network for all services.

### **Network Commands**
```bash
docker network ls              # List networks
docker network inspect mongo-network
docker network rm mongo-network
docker network connect mongo-network <container>
```

### Interview Q&A

**Q: Default bridge vs custom bridge?**

**A**: `Default`: Basic isolation. `Custom`: Direct container-to-container communication by name.

**Q: How do containers find each other?**

**A**: Docker DNS resolves container names to IPs within same network.

---

## Complete Docker Command Reference sheet
📥 [Open ➤ Docker Commands](../03-cheat-sheets/01-docker-commands.md)

---