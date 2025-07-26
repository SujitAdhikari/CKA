# 🚀 Docker Learning Notes (CKA/CKAD Portfolio)

---

## 📆 Image Registry

Docker images are stored in:

- **Public Registry** – Accessible by anyone (e.g., Docker Hub)
- **Private Registry** – Self-hosted or cloud-based with restricted access (e.g., Harbor, AWS ECR)

### 🔧 Example: Docker Hub
1. Create an account on [Docker Hub](https://hub.docker.com)
2. Create a repository
3. Login:
   ```bash
   docker login
   ```
4. Push image:
   ```bash
   docker tag myimage username/myimage:v1
   docker push username/myimage:v1
   ```

---

## ⚙️ Docker Architecture

### 🛑 Docker Components

```
+-------------------------+
|     Docker CLI         | ←—— User interacts via commands
+-------------------------+
            ↓
+-------------------------+
|     Docker Daemon       | ←—— Manages containers, images, volumes
| (dockerd, containerd)   |
+-------------------------+
            ↓
+-------------------------+
|     Container Runtime   | ←—— e.g., runc
+-------------------------+
            ↓
+-------------------------+
|       Linux Kernel      | ←—— Provides namespaces and cgroups
+-------------------------+
```

### 🔹 Key Components:
- **Docker CLI**: User interface (commands like `docker run`)
- **Docker Daemon (`dockerd`)**: Manages Docker objects
- **containerd**: Manages container lifecycle
- **runc**: Low-level container runtime
- **Image Registry**: Stores and distributes images
- **Linux Kernel Features**:
  - Namespaces: Isolation (PID, net, mount)
  - CGroups: Resource control (CPU, memory)

---

## ✅ Advantages of Docker

- Lightweight and fast startup
- Easy to replicate environments (dev/prod)
- Platform-agnostic deployment
- Great for CI/CD pipelines
- Better resource utilization than VMs

## ❌ Disadvantages of Docker

- Less isolation than full VMs
- Storage & networking requires more setup for production
- Security considerations with privileged containers
- Logging and monitoring need external tools

---

## 🔐 Kernel Concepts

- **Namespace** – Isolate process view (network, PID, user)
- **CGroup** – Resource restriction (CPU, memory, block I/O)
- **UnionFS** – Combines multiple layers of file system for images

---

## 🛑 Container Engine Examples

- Docker Engine
- containerd (Docker uses it internally)
- CRI-O (used in Kubernetes)

---

## ☘️ Orchestration Tools

- **Kubernetes (K8s)** – Enterprise-grade orchestrator
- **Docker Swarm** – Lightweight orchestrator by Docker
- **Nomad**, **Rancher**, etc.

---

## ☘️ Kubernetes Overview (For CKA)

- Kubernetes ensures **High Availability (HA)**
- Supports **multi-master setup** using **HAProxy** or **Keepalived**
- Lab environments should use **Kubeadm** or **Minikube**
- Kubernetes components:
  - **Control Plane**: API server, controller-manager, scheduler, etcd
  - **Worker Nodes**: kubelet, kube-proxy, container runtime

---

# 🛠️ Docker Installation (Ubuntu 22.04)

## Step 1: Remove Old Versions
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove -y $pkg; done
```

## Step 2: Add Docker Repository
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

## Step 3: Install Docker Engine
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Step 4: Test Docker
```bash
sudo docker run hello-world
```

---

## ❌ Uninstall Docker (Cleanup)
```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
sudo rm -rf /var/lib/docker /var/lib/containerd
sudo rm /etc/apt/sources.list.d/docker.list
sudo rm /etc/apt/keyrings/docker.asc
```

---

# 📆 Docker Basics

## 🔍 Image & Container Commands
```bash
docker search nginx         # Search images
docker pull nginx           # Pull image
docker images               # List local images
docker ps                   # Running containers
docker ps -a                # All containers
docker rm <name|id>         # Remove container
docker rmi <image>          # Remove image
```

## 🪪 Running Containers
```bash
docker run nginx                      # Run default container
docker run -d -p 8080:80 nginx        # Run in background with port
docker create --name web1 nginx       # Create without starting
docker start web1                     # Start container
docker inspect web1                   # Get metadata (e.g., IP)
curl <ip>                             # Test webserver
docker exec -it web1 sh               # Enter container shell
```

---

# 🏰 Dockerfile & Image Build

## Simple Dockerfile
```dockerfile
# File: Dockerfile
FROM alpine
CMD ["echo", "Welcome to LP_CKA"]
```

### Build and Run
```bash
docker build . -t cka5
docker images
docker run cka5
```

---

## Dockerfile with File Copy
```dockerfile
# File: abc
FROM alpine
COPY his.txt /opt
CMD ["cat", "/opt/his.txt"]
```

### Build and Run
```bash
echo "Container history log" > his.txt
docker build -f abc . -t class3:v1
docker run class3:v1
```

---

# 🧲 Useful Docker Commands (for CKA)
| Command | Purpose |
|--------|---------|
| `docker stats` | Live container resource usage |
| `docker logs <name>` | Get logs |
| `docker exec -it <name> sh` | Interactive shell |
| `docker inspect <name>` | Metadata |
| `docker commit` | Create image from container |
| `docker cp` | Copy files to/from container |
| `docker network ls` | List networks |
| `docker volume ls` | List volumes |

---

# 🧪 Best Practices

- Use minimal base images (e.g., `alpine`)
- Avoid `latest` tag in production
- Use multi-stage builds to reduce image size
- Include `.dockerignore`
- Tag and version your images (e.g., `v1.0.0`)
- Use named volumes for persistence

---

# 🛰️ Future Improvements (Todo)

- [ ] Add **Docker Compose** examples
- [ ] Add **GitHub Actions CI/CD pipeline** for Docker build & push
- [ ] Add Kubernetes manifest basics (deployment, service, etc.)
- [ ] Compare **Minikube vs Kubeadm** for labs
- [ ] Add **Helm** basics
