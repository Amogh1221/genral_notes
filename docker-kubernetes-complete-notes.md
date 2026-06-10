# Docker & Kubernetes — Complete Notes for AI/ML Engineers
### Interview-Ready | Placement-Focused | MLOps Perspective

> **How to use this:** Read end-to-end once to build the mental model. Every section is designed so the concept explanation *is* the answer to the interview question listed at the top of each block.

---

## Table of Contents

1. [Docker — Core Concepts](#1-docker--core-concepts)
2. [Docker Images & Containers](#2-docker-images--containers)
3. [Dockerfile Deep Dive](#3-dockerfile-deep-dive)
4. [Docker Volumes & Storage](#4-docker-volumes--storage)
5. [Docker Networking](#5-docker-networking)
6. [Docker Compose](#6-docker-compose)
7. [Docker Best Practices & Security](#7-docker-best-practices--security)
8. [Kubernetes — Core Concepts](#8-kubernetes--core-concepts)
9. [Kubernetes Architecture](#9-kubernetes-architecture)
10. [Kubernetes Workloads](#10-kubernetes-workloads)
11. [Kubernetes Networking & Services](#11-kubernetes-networking--services)
12. [Kubernetes Storage](#12-kubernetes-storage)
13. [Kubernetes Configuration & Secrets](#13-kubernetes-configuration--secrets)
14. [Kubernetes Security (RBAC)](#14-kubernetes-security-rbac)
15. [Kubernetes Probes & Health Checks](#15-kubernetes-probes--health-checks)
16. [Kubernetes Scaling & Autoscaling](#16-kubernetes-scaling--autoscaling)
17. [Docker + Kubernetes for ML/AI (MLOps)](#17-docker--kubernetes-for-mlai-mlops)
18. [Interview Q&A — Docker](#18-interview-qa--docker)
19. [Interview Q&A — Kubernetes](#19-interview-qa--kubernetes)
20. [Interview Q&A — MLOps / AI/ML Scenarios](#20-interview-qa--mlops--aiml-scenarios)
21. [Quick Reference Cheat Sheet](#21-quick-reference-cheat-sheet)

---

## 1. Docker — Core Concepts

> **Interview Q:** *What is Docker and why do we use it?*

Docker is a platform for packaging applications and their dependencies into **containers** — isolated, portable, lightweight runtime environments. It solves the classic "it works on my machine" problem by guaranteeing identical behaviour across every environment.

**Key terminology:**
- **Docker Daemon (`dockerd`)** — background service managing containers, images, volumes, and networks
- **Docker Client (CLI)** — the `docker` command you type in the terminal; sends API calls to the daemon
- **Docker Registry** — storage for images (Docker Hub is the default public registry; you can self-host with Docker Registry)
- **Docker Hub** — the public cloud-hosted registry; official + community images live here

**Docker's internal architecture (3 components since v1.11):**

| Component | Role |
|---|---|
| `dockerd` (Docker Engine) | Builds images, exposes API, top-level orchestration |
| `containerd` | Downloads images, manages container lifecycle |
| `runc` | Creates Linux namespaces + cgroups, actually runs the container process |

> `containerd` *manages* the container. `runc` *runs* it. This separation follows the OCI specification.

**Docker vs VM:**

| | Docker Container | Virtual Machine |
|---|---|---|
| OS | Shares host kernel | Full OS per VM |
| Startup | Seconds | Minutes |
| Size | MB | GB |
| Isolation | Process-level | Hardware-level |
| Performance | Near-native | Overhead from hypervisor |

**Docker architecture diagram:**
![Docker Architecture](https://docs.docker.com/get-started/images/docker-architecture.webp)

---

## 2. Docker Images & Containers

> **Interview Q:** *What is the difference between a Docker image and a container?*

| | Image | Container |
|---|---|---|
| Definition | Read-only blueprint/snapshot | Running instance of an image |
| State | Static, immutable | Has a writable layer on top |
| Analogy | Class definition | Object instance |
| Storage | Stored in registry / local cache | Runs in memory with ephemeral layer |

One image → many containers. A container = image layers + a thin writable layer on top.

### Image Layers

Images are built from stacked **read-only layers**. Each `RUN`, `COPY`, `ADD` instruction in a Dockerfile creates a new layer. Docker uses **Union File System (UnionFS)** to merge them.

```
Container writable layer  ← lost when container dies
────────────────────────
Layer 3: COPY app/ .
Layer 2: RUN pip install -r requirements.txt
Layer 1: FROM python:3.11-slim
```

**Layer caching:** If nothing changed in a layer, Docker reuses the cached version. This is why instruction order in Dockerfiles matters — put rarely-changing instructions (installing packages) before frequently-changing ones (copying source code).

> **Interview Q:** *What are dangling images?*
> Dangling images are untagged, unused image layers — typically intermediate layers left after a rebuild. Remove them with `docker image prune`.

### Essential Image Commands

```bash
docker pull nginx:alpine           # Download from registry
docker images                      # List local images
docker images -a                   # Include intermediate layers
docker rmi <image_id>              # Remove image
docker image prune                 # Remove dangling images
docker image prune -a              # Remove all unused images
docker history <image>             # Show layer history
docker image inspect <image>       # Full metadata (ports, env vars, etc.)
docker tag my-model:v1 my-model:latest  # Tag an image
docker push myrepo/my-model:v1    # Push to registry
```

### Essential Container Commands

```bash
docker run --name myapp -d -p 8080:80 nginx   # Run detached
docker run -it ubuntu bash                     # Interactive shell
docker ps                                      # Running containers
docker ps -a                                   # All containers (incl. stopped)
docker stop <name>                             # Graceful stop (SIGTERM)
docker kill <name>                             # Forceful stop (SIGKILL)
docker rm <name>                               # Remove stopped container
docker rm $(docker ps -aq)                    # Remove all containers
docker exec -it <name> bash                   # Shell into running container
docker logs -f <name>                          # Stream logs
docker logs --since=10m <name>                # Last 10 min of logs
docker cp <name>:/app/file.txt ./             # Copy from container
docker stats <name>                            # Live resource usage
docker top <name>                              # Processes inside container
docker inspect <name>                          # Full config/network info
```

> **Interview Q:** *What is the difference between `docker stop` and `docker kill`?*
> `stop` sends `SIGTERM` giving the process a chance to shut down cleanly. `kill` sends `SIGKILL` which terminates immediately. Always prefer `stop` in production.

---

## 3. Dockerfile Deep Dive

> **Interview Q:** *What is a Dockerfile and walk me through each instruction.*

A Dockerfile is a text file with sequential instructions Docker uses to build an image. Every instruction either creates a new layer or adds metadata.

```dockerfile
# ── Base image ──────────────────────────────────────────────────
FROM python:3.11-slim

# ── Metadata ────────────────────────────────────────────────────
LABEL maintainer="yourname@email.com"
LABEL version="1.0"

# ── Build-time variables (not available at runtime) ─────────────
ARG MODEL_VERSION=v1

# ── Environment variables (available at runtime) ────────────────
ENV APP_PORT=8000
ENV MODEL_PATH=/app/models

# ── Working directory ───────────────────────────────────────────
WORKDIR /app

# ── Install dependencies first (cached separately from code) ────
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ── Copy application source ─────────────────────────────────────
COPY src/ ./src/

# ── Expose port (documentation only, doesn't actually open it) ──
EXPOSE 8000

# ── Create non-root user for security ───────────────────────────
RUN useradd -m appuser
USER appuser

# ── Health check ─────────────────────────────────────────────────
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1

# ── Default command ─────────────────────────────────────────────
ENTRYPOINT ["python", "-m", "uvicorn"]
CMD ["src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### All Dockerfile Instructions

| Instruction | Purpose | Notes |
|---|---|---|
| `FROM` | Sets base image | Must be first (except `ARG`) |
| `RUN` | Execute command during build | Creates a new layer |
| `CMD` | Default command at container start | Can be overridden by `docker run <cmd>` |
| `ENTRYPOINT` | Fixed executable at start | CMD becomes arguments to ENTRYPOINT |
| `COPY` | Copy files from host to image | Prefer over `ADD` for simple copies |
| `ADD` | Like COPY + supports URLs + tar extraction | Use only when you need those extras |
| `WORKDIR` | Set working directory | Creates directory if it doesn't exist |
| `ENV` | Set runtime environment variable | Visible inside running container |
| `ARG` | Build-time variable | Not visible at runtime |
| `EXPOSE` | Documents which port the app uses | Does NOT publish the port |
| `VOLUME` | Marks a mount point | Declares intent for persistent data |
| `USER` | Set user/UID for subsequent instructions | Critical for security |
| `LABEL` | Key-value metadata | Good for version tagging |
| `SHELL` | Override default shell (`/bin/sh`) | e.g., use bash |
| `HEALTHCHECK` | Container self-health test | Used by Docker and Kubernetes |

> **Interview Q:** *What is the difference between CMD and ENTRYPOINT?*
> `ENTRYPOINT` defines the fixed executable. `CMD` provides default arguments to it. If you use both:
> `ENTRYPOINT ["python"] CMD ["app.py"]` → runs `python app.py`. Running `docker run myimage train.py` overrides CMD → `python train.py`. To override ENTRYPOINT, use `--entrypoint` flag.

> **Interview Q:** *Difference between `COPY` and `ADD`?*
> Use `COPY` for simple file/directory copies. Use `ADD` only when you need remote URL fetching or automatic tar extraction. `COPY` is more explicit and predictable.

> **Interview Q:** *What is `ARG` vs `ENV`?*
> `ARG` is only available during the build process (`docker build --build-arg KEY=val`). `ENV` persists into the running container. Never put secrets in `ARG` — they appear in `docker history`.

### Multi-Stage Builds

> **Interview Q:** *What is a multi-stage build and why is it useful?*

Multi-stage builds let you use multiple `FROM` statements to keep only what's needed in the final image — critical for ML where training dependencies (CUDA, build tools) shouldn't be in the inference image.

```dockerfile
# Stage 1: Build / Train
FROM python:3.11 AS builder
WORKDIR /build
COPY requirements-train.txt .
RUN pip install -r requirements-train.txt
COPY . .
RUN python train.py --output /build/model.pkl

# Stage 2: Serve (lean image — only inference deps)
FROM python:3.11-slim AS server
WORKDIR /app
COPY requirements-serve.txt .
RUN pip install -r requirements-serve.txt
COPY --from=builder /build/model.pkl ./model.pkl   # ← copy from build stage
COPY src/serve.py .
EXPOSE 8000
CMD ["python", "serve.py"]
```

Final image only contains the slim server — not the training dependencies. Image size can drop from 4GB → 300MB.

### `.dockerignore`

Prevents files from being sent to the Docker daemon (reduces build context size, improves caching, avoids leaking secrets):

```
__pycache__/
*.pyc
.git/
.env
*.ipynb_checkpoints
data/raw/
models/checkpoints/
.DS_Store
```

---

## 4. Docker Volumes & Storage

> **Interview Q:** *How does data persist in Docker? What are the different storage options?*

Container filesystems are ephemeral — data is lost when a container dies. Three options for persistence:

### Storage Options Comparison

| Type | Storage Location | Persists after container dies? | Best for |
|---|---|---|---|
| **Named Volume** | `/var/lib/docker/volumes/` | ✅ Yes | Production data, databases |
| **Bind Mount** | Anywhere on host filesystem | ✅ Yes | Dev mode, sharing source code |
| **tmpfs Mount** | Host RAM only | ❌ No | Secrets, temp sensitive data |

![Docker Storage Types](https://docs.docker.com/storage/images/types-of-mounts-bind.png)

### Image Layer Internals

```
Container  →  Read/Write layer  (lost on stop/delete)
             ─────────────────
Image      →  Layer N (RUN ...)   read-only
             Layer 2 (COPY ...)   read-only
             Layer 1 (FROM ...)   read-only
```

### Volume Commands

```bash
docker volume create ml-data                    # Create named volume
docker volume ls                                # List volumes
docker volume inspect ml-data                  # Show mount point + metadata
docker volume rm ml-data                       # Remove volume
docker volume prune                            # Remove all unused volumes
docker volume ls -f dangling=true             # Show unused volumes
```

### Using Volumes

```bash
# Named volume (recommended for production)
docker run -v ml-models:/app/models my-model-server

# Bind mount (dev: live-reload source code)
docker run -v ./src:/app/src my-app

# Read-only bind mount
docker run -v ./config:/app/config:ro my-app

# Using --mount (more explicit, preferred)
docker run --mount type=volume,source=ml-data,target=/app/data my-app
docker run --mount type=bind,source=$(pwd)/src,target=/app/src my-app
docker run --mount type=tmpfs,target=/app/tmp my-app
```

> **Interview Q:** *Named Volume vs Bind Mount — when to use which?*
> Named volumes are managed by Docker, portable, good for production databases and model artefacts. Bind mounts are direct host paths — perfect for development (live code changes) and config file injection. In production, prefer named volumes.

> **Interview Q:** *What is a dangling volume?*
> A volume not attached to any container. Wasted disk space. Clean up with `docker volume prune`.

### Sharing Volumes Between Containers

```bash
# Start container with volume
docker run -v /app/data --name data-source ubuntu

# Second container inherits the same volume
docker run --volumes-from data-source my-processor
```

---

## 5. Docker Networking

> **Interview Q:** *How does Docker networking work? What are the network drivers?*

Docker implements networking by manipulating **iptables rules** on the host. It creates virtual network interfaces and bridges. Containers on the same user-defined network can communicate by container name (DNS resolution). Default bridge network only allows IP communication.

### Network Drivers

| Driver | Description | Use Case |
|---|---|---|
| `bridge` (default) | Virtual switch connecting containers on same host | Most single-host setups |
| `host` | Container shares host's network stack (no isolation) | Performance-critical apps |
| `none` | No network — fully isolated | Maximum security / sandboxing |
| `overlay` | Multi-host networking (Docker Swarm) | Distributed services |
| `macvlan` | Assigns MAC address to container | Legacy apps needing direct network access |
| `ipvlan` | Full IPv4/IPv6 control | Advanced networking scenarios |

> Docker creates `bridge`, `none`, and `host` networks automatically on install.

### Key Networking Behaviour

```
Same default bridge network:        communicate by IP only
Same user-defined bridge network:   communicate by name OR IP ✅
Different networks:                 cannot communicate (iptables blocks)
Container ↔ host:                   always reachable
```

### Network Commands

```bash
docker network create my-net                         # Default: bridge
docker network create --subnet 10.7.0.0/16 my-net   # Custom subnet
docker network ls                                    # List networks
docker network inspect my-net                        # Show connected containers + IPs
docker network rm my-net                             # Remove network
docker network connect my-net my-container          # Connect running container
docker network disconnect my-net my-container       # Disconnect
```

### Port Mapping

```bash
docker run -p 8080:80 nginx           # host:container (accessible externally)
docker run -p 127.0.0.1:8080:80 nginx # Localhost only
docker run --expose 80 nginx          # Expose without publishing to host
```

> **Interview Q:** *How do two containers communicate?*
> Put them on the same **user-defined bridge network**. Then they can reach each other using container names. e.g., a FastAPI app can call `http://redis:6379` if both are on the same network. Using the default bridge network, only IP addresses work.

---

## 6. Docker Compose

> **Interview Q:** *What is Docker Compose and how does it differ from Dockerfile?*

**Dockerfile** defines how to build a single image. **Docker Compose** orchestrates multiple containers as a multi-service application using a single YAML file.

| | Dockerfile | Docker Compose |
|---|---|---|
| Scope | Single image | Multi-container application |
| Purpose | Build | Run + orchestrate |
| File | `Dockerfile` | `docker-compose.yml` |

### Compose File Structure

```yaml
version: '3.8'

services:
  # ML Model API
  model-api:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - MODEL_PATH=/models/my_model.pkl
      - LOG_LEVEL=INFO
    env_file:
      - .env
    volumes:
      - ./models:/models            # bind mount for model files
      - model-cache:/app/.cache     # named volume for cache
    networks:
      - ml-network
    depends_on:
      redis:
        condition: service_healthy  # wait until redis is healthy
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]   # GPU access for inference
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    restart: on-failure

  # Redis for prediction caching
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    networks:
      - ml-network
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      retries: 3

  # Postgres for experiment tracking
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: mlflow
      POSTGRES_USER: mlflow
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - pg-data:/var/lib/postgresql/data
    networks:
      - ml-network
    restart: unless-stopped

volumes:
  model-cache:
  redis-data:
  pg-data:

networks:
  ml-network:
    driver: bridge
```

### Compose Commands

```bash
docker compose up -d               # Start all services (detached)
docker compose up --build          # Rebuild images then start
docker compose down                # Stop + remove containers
docker compose down -v             # Also remove volumes
docker compose ps                  # Status of all services
docker compose logs -f model-api   # Stream logs for a service
docker compose exec model-api bash # Shell into a running service
docker compose restart model-api   # Restart one service
docker compose scale model-api=3   # Run 3 replicas (Swarm mode)
docker compose config              # Validate compose file
```

### Restart Policies

| Policy | Behaviour |
|---|---|
| `no` | Never restart (default) |
| `always` | Always restart (even on clean exit) |
| `on-failure` | Restart only on non-zero exit code |
| `unless-stopped` | Restart unless manually stopped |

> **Interview Q:** *How does `depends_on` work?*
> `depends_on` controls **start order** — it doesn't wait for the service to be *ready*. Use `condition: service_healthy` with healthchecks to wait until a service is actually healthy before starting the dependent service.

---

## 7. Docker Best Practices & Security

> **Interview Q:** *What are Dockerfile best practices for production ML images?*

### Layer Optimisation

```dockerfile
# ❌ BAD — separate RUN commands = separate layers, bigger image
RUN apt-get update
RUN apt-get install -y wget
RUN wget https://example.com/file.tar.gz
RUN tar xvzf file.tar.gz
RUN rm file.tar.gz

# ✅ GOOD — chain with && = single layer
RUN apt-get update && apt-get install -y wget \
    && wget https://example.com/file.tar.gz \
    && tar xvzf file.tar.gz \
    && rm file.tar.gz \
    && apt-get remove -y wget \
    && apt-get autoremove -y

# ✅ Order: stable deps first, changing code last
FROM python:3.11-slim
RUN pip install torch>=2.0                # Changes rarely → cached
COPY requirements.txt .
RUN pip install -r requirements.txt      # Changes sometimes
COPY src/ .                              # Changes often → always rebuilds from here
```

### Security Best Practices

```dockerfile
# 1. Use minimal base images
FROM python:3.11-slim        # not python:3.11 (saves ~700MB)
FROM python:3.11-alpine      # even smaller (but may break C extensions)

# 2. Run as non-root user
RUN useradd -m -u 1000 mluser
USER mluser

# 3. Use specific image tags, never :latest in production
FROM python:3.11.9-slim      # pinned, reproducible

# 4. Don't copy secrets — use runtime env vars or secrets managers
# ❌ COPY .env .           # .env gets baked into image
# ✅ Use: docker run -e KEY=val  or  --env-file at runtime
```

**Runtime security flags:**
```bash
docker run --read-only my-app                              # Read-only filesystem
docker run --user=1000:1000 my-app                        # Run as non-root
docker run --cap-drop ALL --cap-add NET_BIND_SERVICE app  # Drop Linux capabilities
docker run --security-opt=no-new-privileges:true my-app   # Block privilege escalation
docker run -m=4g --cpus=2 my-model-server                 # Limit resources
```

**Daemon security:**
- Never expose `/var/run/docker.sock` to containers (root-equivalent access)
- Disable inter-container communication by default: `dockerd --icc=false`
- Keep Docker updated — patches fix CVEs regularly
- Use `docker scout` / Trivy / Anchore to scan images for vulnerabilities
- Enable Docker Content Trust: `DOCKER_CONTENT_TRUST=1`

### Docker Logging

```bash
# Log location (json-file driver, default)
/var/lib/docker/containers/<container_id>/<container_id>-json.log

# Tail and follow
docker logs -f --since=1h my-container

# Configure rotation (prevents disk fill)
docker run --log-opt max-size=100m --log-opt max-file=5 my-app

# Global rotation config (/etc/docker/daemon.json)
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m",
    "max-file": "5"
  }
}
```

### Resource Management

```bash
docker run -m=4g my-model                    # Max 4GB RAM
docker run --memory-reservation=2g my-model  # Soft limit
docker run --cpus=2 my-model                 # 2 CPU cores
docker run --gpus all my-model               # All GPUs (nvidia-docker)
docker run --gpus '"device=0,1"' my-model   # Specific GPUs
```

---

## 8. Kubernetes — Core Concepts

> **Interview Q:** *What is Kubernetes and why do we need it beyond Docker?*

**Kubernetes (K8s)** is an open-source container orchestration platform. While Docker runs containers on a single machine, Kubernetes manages containers across a **cluster** of machines — handling scheduling, scaling, self-healing, load balancing, and rolling updates automatically.

| | Docker (alone) | Kubernetes |
|---|---|---|
| Scope | Single host | Cluster of machines |
| Scaling | Manual | Automatic (HPA) |
| Self-healing | No | Yes (restarts failed pods) |
| Load balancing | Manual / Compose | Built-in Services |
| Rolling updates | No | Yes (zero downtime) |
| Config management | env files | ConfigMaps + Secrets |
| Storage | Volumes | PersistentVolumes |

**Kubernetes Architecture Diagram:**
![Kubernetes Architecture](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

---

## 9. Kubernetes Architecture

> **Interview Q:** *Explain the Kubernetes architecture. What are the components of the control plane?*

A Kubernetes cluster has two tiers:

### Control Plane (Master Node) — the brain

| Component | Role | Analogy |
|---|---|---|
| **kube-apiserver** | HTTP API — all commands go through here | Front desk / receptionist |
| **etcd** | Distributed key-value store of all cluster state | Source of truth database |
| **kube-scheduler** | Assigns pods to worker nodes based on resources | Task dispatcher |
| **kube-controller-manager** | Runs control loops to maintain desired state | Supervisor that fixes deviations |
| **cloud-controller-manager** | Integrates with cloud APIs (AWS, GCP, Azure) | Cloud bridge |

### Worker Nodes — the muscle

| Component | Role |
|---|---|
| **kubelet** | Node agent — ensures containers in pods are running |
| **kube-proxy** | Maintains network rules (iptables) for Service routing |
| **Container Runtime** | Runs containers (containerd, CRI-O — Docker is no longer default) |

> **Interview Q:** *What is etcd?*
> etcd is a distributed, strongly consistent key-value store. It's the single source of truth for all cluster state — desired state, actual state, config, secrets. If etcd goes down, the cluster loses its brain. Always back it up.

> **Interview Q:** *What does the kube-scheduler do?*
> It watches for unscheduled pods and assigns each one to a suitable node. It considers resource requests (CPU, memory, GPU), node affinity/taints, and available capacity.

---

## 10. Kubernetes Workloads

> **Interview Q:** *What is a Pod? Why is it the smallest deployable unit?*

### Pod

A **Pod** is a group of one or more containers sharing the same network namespace and storage. Containers in a pod share the same IP address and can communicate over `localhost`. Pods are ephemeral — they can be killed and rescheduled at any time.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: model-server
  labels:
    app: model-server
spec:
  containers:
  - name: inference
    image: myrepo/model:v2
    ports:
    - containerPort: 8000
    resources:
      requests:
        memory: "512Mi"
        cpu: "250m"
        nvidia.com/gpu: "1"       # GPU request for ML inference
      limits:
        memory: "2Gi"
        cpu: "1000m"
        nvidia.com/gpu: "1"
```

> You almost never create Pods directly — you use higher-level controllers.

### Deployment

> **Interview Q:** *What is a Deployment? How does a rolling update work?*

A Deployment manages a **ReplicaSet**, which ensures N identical pods are always running. It supports rolling updates (zero-downtime upgrades) and rollbacks.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: model-serving
spec:
  replicas: 3
  selector:
    matchLabels:
      app: model-serving
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1            # Allow 1 extra pod during update
      maxUnavailable: 0      # Never drop below desired count
  template:
    metadata:
      labels:
        app: model-serving
    spec:
      containers:
      - name: model
        image: myrepo/model:v2
        ports:
        - containerPort: 8000
```

```bash
kubectl apply -f deployment.yaml          # Deploy
kubectl rollout status deployment/model   # Watch rollout
kubectl rollout history deployment/model  # View versions
kubectl rollout undo deployment/model     # Rollback to previous
kubectl set image deployment/model model=myrepo/model:v3  # Update image
```

### ReplicaSet

Ensures a specified number of pod replicas are always running. Deployments manage ReplicaSets — rarely create ReplicaSets directly.

### StatefulSet

> **Interview Q:** *When do you use StatefulSet vs Deployment?*

| | Deployment | StatefulSet |
|---|---|---|
| Pod identity | Random names (`pod-xyz`) | Stable ordered names (`pod-0`, `pod-1`) |
| Storage | Shared or none | Each pod gets its own PVC |
| Scaling | Any order | Ordered (0 → 1 → 2) |
| Use case | Stateless apps, ML inference | Databases, Kafka, ML training with checkpoints |

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: ml-trainer
spec:
  serviceName: "ml-trainer"
  replicas: 3
  selector:
    matchLabels:
      app: ml-trainer
  template:
    spec:
      containers:
      - name: trainer
        image: myrepo/trainer:v1
  volumeClaimTemplates:          # Each pod gets its OWN PVC
  - metadata:
      name: training-data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 50Gi
```

### DaemonSet

Ensures one pod runs on **every node** (or a subset). Used for logging agents, monitoring, node-level utilities.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-collector
spec:
  selector:
    matchLabels:
      app: log-collector
  template:
    spec:
      containers:
      - name: fluentd
        image: fluentd:latest
```

*ML use case: Run a GPU metrics exporter on every GPU node in the cluster.*

### Jobs and CronJobs

> **Interview Q:** *What is a Kubernetes Job? How is it different from a Deployment?*

- **Job** — runs a task until successful completion. Perfect for batch ML training, data preprocessing, evaluation runs.
- **CronJob** — runs a Job on a schedule. Perfect for periodic retraining, scheduled batch inference.

```yaml
# One-time training job
apiVersion: batch/v1
kind: Job
metadata:
  name: model-training-run
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 3           # Retry up to 3 times on failure
  template:
    spec:
      restartPolicy: OnFailure
      containers:
      - name: trainer
        image: myrepo/trainer:v1
        command: ["python", "train.py", "--epochs=100"]
        resources:
          limits:
            nvidia.com/gpu: "2"

---
# Scheduled retraining
apiVersion: batch/v1
kind: CronJob
metadata:
  name: nightly-retrain
spec:
  schedule: "0 2 * * *"    # Every night at 2am
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: retrainer
            image: myrepo/retrainer:v1
```

---

## 11. Kubernetes Networking & Services

> **Interview Q:** *What is a Kubernetes Service? What are the types?*

Pods are ephemeral — their IPs change constantly. A **Service** is a stable network endpoint that routes traffic to a set of pods selected by labels. The Service IP never changes even as pods are replaced.

### Service Types

| Type | Accessibility | Use Case |
|---|---|---|
| `ClusterIP` (default) | Inside cluster only | Service-to-service communication |
| `NodePort` | Via `<NodeIP>:<NodePort>` (30000-32767) | Dev/testing, simple external access |
| `LoadBalancer` | External cloud load balancer | Production external traffic |
| `ExternalName` | Maps to a DNS name | Routing to external services |

```yaml
# ClusterIP — internal service
apiVersion: v1
kind: Service
metadata:
  name: model-api-service
spec:
  type: ClusterIP
  selector:
    app: model-serving           # Routes to pods with this label
  ports:
  - port: 80
    targetPort: 8000

---
# LoadBalancer — external production traffic
apiVersion: v1
kind: Service
metadata:
  name: model-api-lb
spec:
  type: LoadBalancer
  selector:
    app: model-serving
  ports:
  - port: 80
    targetPort: 8000
```

### Ingress

> **Interview Q:** *What is an Ingress? How is it different from a Service?*

**Ingress** is an L7 (HTTP/HTTPS) routing layer. A single Ingress can route traffic to multiple services based on URL path or hostname — like a reverse proxy. Requires an **Ingress Controller** (nginx, Traefik, AWS ALB) to be deployed first.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ml-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: api.mymlapp.com
    http:
      paths:
      - path: /predict
        pathType: Prefix
        backend:
          service:
            name: model-api-service
            port:
              number: 80
      - path: /train
        pathType: Prefix
        backend:
          service:
            name: training-service
            port:
              number: 80
  tls:
  - hosts:
    - api.mymlapp.com
    secretName: tls-secret
```

### Namespaces

Logical isolation within a cluster. Resources in different namespaces are independent.

```bash
kubectl get namespaces
kubectl create namespace ml-production
kubectl create namespace ml-staging
kubectl get pods -n ml-production       # Target a specific namespace
kubectl get pods --all-namespaces       # All namespaces
```

Built-in namespaces: `default`, `kube-system`, `kube-public`, `kube-node-lease`

---

## 12. Kubernetes Storage

> **Interview Q:** *How does Kubernetes handle persistent storage?*

### PersistentVolume (PV) and PersistentVolumeClaim (PVC)

- **PersistentVolume (PV)** — a piece of storage provisioned by an admin (or dynamically by a StorageClass)
- **PersistentVolumeClaim (PVC)** — a request for storage by a pod

```yaml
# PVC — request 100GB for training data
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: training-data-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Gi
  storageClassName: standard    # Triggers dynamic provisioning

---
# Use PVC in a pod
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: trainer
    volumeMounts:
    - mountPath: /data
      name: training-data
  volumes:
  - name: training-data
    persistentVolumeClaim:
      claimName: training-data-pvc
```

### Volume Types in Pods

```yaml
spec:
  volumes:
  - name: config-vol
    configMap:
      name: my-config
  - name: secret-vol
    secret:
      secretName: my-secret
  - name: scratch
    emptyDir: {}           # Temp storage, lost when pod dies
  - name: host-vol
    hostPath:
      path: /data/models   # Direct host path (use carefully)
```

> **Interview Q:** *What is `emptyDir`?*
> A temporary volume created when the pod starts and deleted when the pod ends. Shared between containers in the same pod. Good for scratch space, intermediate data — e.g., sharing preprocessed data between an init container and main container.

---

## 13. Kubernetes Configuration & Secrets

> **Interview Q:** *What is a ConfigMap? What is the difference between ConfigMap and Secret?*

### ConfigMap

Stores non-sensitive configuration as key-value pairs. Decouples config from container images.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: model-config
data:
  MODEL_VERSION: "v2"
  BATCH_SIZE: "32"
  LOG_LEVEL: "INFO"
  config.yaml: |
    model:
      architecture: resnet50
      num_classes: 1000
```

**Use as environment variable:**
```yaml
spec:
  containers:
  - name: model
    envFrom:
    - configMapRef:
        name: model-config
```

**Use as mounted file:**
```yaml
  volumeMounts:
  - name: config
    mountPath: /app/config
  volumes:
  - name: config
    configMap:
      name: model-config
```

### Secret

For sensitive data (passwords, API keys, tokens). Stored as **base64-encoded** in etcd (not encrypted by default — enable etcd encryption at rest in production).

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: ml-secrets
type: Opaque
data:
  WANDB_API_KEY: <base64-encoded-value>
  DB_PASSWORD: <base64-encoded-value>
```

```bash
# Create from CLI (base64 encoding handled automatically)
kubectl create secret generic ml-secrets \
  --from-literal=WANDB_API_KEY=your-key \
  --from-literal=DB_PASSWORD=your-pass
```

| | ConfigMap | Secret |
|---|---|---|
| Data type | Non-sensitive | Sensitive |
| Encoding | Plain text | base64 |
| Encrypted at rest | No | Optional (requires config) |
| Use for | App config, env vars | API keys, passwords, TLS certs |

> **Interview Q:** *Are Kubernetes Secrets actually secure?*
> By default, Secrets are only base64-encoded (not encrypted) in etcd. For real security: enable etcd encryption at rest, use RBAC to restrict Secret access, or integrate with external secrets managers (HashiCorp Vault, AWS Secrets Manager).

---

## 14. Kubernetes Security (RBAC)

> **Interview Q:** *What is RBAC in Kubernetes?*

Role-Based Access Control (RBAC) controls who can do what on which resources.

| Resource | Scope | Purpose |
|---|---|---|
| `Role` | Namespace | Permissions within a namespace |
| `ClusterRole` | Cluster-wide | Permissions across all namespaces |
| `RoleBinding` | Namespace | Assigns Role to user/service account |
| `ClusterRoleBinding` | Cluster-wide | Assigns ClusterRole cluster-wide |

```yaml
# Role: allow reading pods in ml-production namespace
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: ml-production
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods", "pods/logs"]
  verbs: ["get", "list", "watch"]

---
# Bind the role to a service account
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: ml-production
  name: read-pods-binding
subjects:
- kind: ServiceAccount
  name: ml-pipeline-sa
  namespace: ml-production
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

### Service Accounts

A ServiceAccount is a non-human identity for processes running in pods. Every pod automatically gets the `default` ServiceAccount. Create dedicated ServiceAccounts for ML pipelines with only the permissions they need.

```bash
kubectl create serviceaccount ml-pipeline-sa -n ml-production
```

---

## 15. Kubernetes Probes & Health Checks

> **Interview Q:** *What are liveness, readiness, and startup probes?*

| Probe | Checked by | Failure Action | Purpose |
|---|---|---|---|
| **Liveness** | kubelet continuously | Restart container | Is the app still alive? (dead-lock detection) |
| **Readiness** | kubelet continuously | Remove from Service endpoints | Is the app ready to receive traffic? |
| **Startup** | kubelet at start only | Restart container | Has the app finished starting? (slow-start protection) |

```yaml
spec:
  containers:
  - name: model-server
    image: myrepo/model:v2
    
    startupProbe:
      httpGet:
        path: /health
        port: 8000
      failureThreshold: 30      # Allow 30 * 10s = 5min to start (ML models load slowly)
      periodSeconds: 10
    
    livenessProbe:
      httpGet:
        path: /health
        port: 8000
      initialDelaySeconds: 10
      periodSeconds: 30
      failureThreshold: 3
    
    readinessProbe:
      httpGet:
        path: /ready             # Returns 200 only when model is loaded
        port: 8000
      initialDelaySeconds: 5
      periodSeconds: 10
      failureThreshold: 3
```

**Probe types:**
```yaml
# HTTP check
httpGet:
  path: /health
  port: 8000

# TCP port check
tcpSocket:
  port: 8000

# Command execution
exec:
  command: ["python", "-c", "import model; model.ping()"]
```

> **ML-specific note:** Large models (LLMs, vision models) can take minutes to load into GPU memory. Use `startupProbe` with a high `failureThreshold` to avoid liveness killing the container before it finishes loading.

### Pod Termination Flow

1. kubelet sends `SIGTERM` to container
2. `preStop` hook runs (if defined)
3. `terminationGracePeriodSeconds` countdown starts (default: 30s)
4. If still running after grace period → `SIGKILL`

```yaml
lifecycle:
  preStop:
    exec:
      command: ["/bin/sh", "-c", "sleep 5"]  # Drain in-flight requests
```

---

## 16. Kubernetes Scaling & Autoscaling

> **Interview Q:** *How does Kubernetes autoscaling work? What are HPA and VPA?*

### Manual Scaling

```bash
kubectl scale deployment model-serving --replicas=10
```

### Horizontal Pod Autoscaler (HPA)

Automatically scales the number of pod replicas based on CPU, memory, or custom metrics (GPU utilisation, request queue length).

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: model-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: model-serving
  minReplicas: 2
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 70
```

```bash
kubectl autoscale deployment model-serving --cpu-percent=60 --min=2 --max=20
kubectl get hpa
```

### Vertical Pod Autoscaler (VPA)

Automatically adjusts CPU and memory **requests/limits** of existing pods. Good for right-sizing ML training jobs.

### Cluster Autoscaler

Adds or removes **nodes** from the cluster based on pod scheduling demand. Integrates with cloud providers (AWS, GCP, Azure).

| Autoscaler | What it scales | When to use |
|---|---|---|
| HPA | Number of pods | Variable traffic, online inference |
| VPA | Pod CPU/memory | Right-sizing training jobs |
| Cluster Autoscaler | Number of nodes | When pods can't be scheduled |

---

## 17. Docker + Kubernetes for ML/AI (MLOps)

> **Interview Q:** *How do you containerise and deploy an ML model using Docker and Kubernetes?*

### End-to-End ML Deployment Pattern

```
Data Scientists → train model → save artefacts
        ↓
DevOps/MLOps  → Dockerfile → Docker image → Registry
        ↓
Kubernetes    → Deployment + Service + HPA → Production
        ↓
Monitoring    → Prometheus + Grafana + model drift detection
```

### Containerising a PyTorch Model

```dockerfile
FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime

WORKDIR /app

# Install inference dependencies only
COPY requirements-serve.txt .
RUN pip install --no-cache-dir -r requirements-serve.txt \
    && pip install fastapi uvicorn

COPY models/ ./models/
COPY src/inference.py .

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=15s --start-period=120s \
  CMD curl -f http://localhost:8000/health || exit 1

USER nobody
CMD ["uvicorn", "inference:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Kubernetes Deployment for GPU Inference

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pytorch-inference
  namespace: ml-production
spec:
  replicas: 2
  selector:
    matchLabels:
      app: pytorch-inference
  template:
    metadata:
      labels:
        app: pytorch-inference
    spec:
      containers:
      - name: inference
        image: myrepo/pytorch-inference:v2.1
        ports:
        - containerPort: 8000
        resources:
          requests:
            memory: "4Gi"
            cpu: "1"
            nvidia.com/gpu: "1"
          limits:
            memory: "8Gi"
            cpu: "2"
            nvidia.com/gpu: "1"
        env:
        - name: MODEL_PATH
          value: "/models/resnet50.pt"
        - name: WANDB_API_KEY
          valueFrom:
            secretKeyRef:
              name: ml-secrets
              key: WANDB_API_KEY
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 120    # Model load time
          periodSeconds: 30
        readinessProbe:
          httpGet:
            path: /ready
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        volumeMounts:
        - name: model-storage
          mountPath: /models
      volumes:
      - name: model-storage
        persistentVolumeClaim:
          claimName: model-pvc
      nodeSelector:
        accelerator: nvidia-tesla-v100   # Schedule only on GPU nodes
```

### Key MLOps Patterns

**Model Versioning:**
```bash
docker tag myrepo/model:latest myrepo/model:v1.2.3
docker push myrepo/model:v1.2.3
kubectl set image deployment/model-serving inference=myrepo/model:v1.2.3
```

**Blue-Green Deployment (zero-downtime model swap):**
```bash
# Deploy v2 alongside v1
kubectl apply -f deployment-v2.yaml
# Test v2
kubectl apply -f service-v2-test.yaml
# Switch traffic
kubectl patch service model-api -p '{"spec":{"selector":{"version":"v2"}}}'
# Clean up v1
kubectl delete deployment model-serving-v1
```

**Canary Deployment (gradual rollout):**
```yaml
# 90% traffic to v1 (9 replicas), 10% to v2 (1 replica)
# Both share the same Service selector label
---
kind: Deployment
metadata:
  name: model-v1
spec:
  replicas: 9
  template:
    metadata:
      labels:
        app: model
        version: v1
---
kind: Deployment
metadata:
  name: model-v2
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: model
        version: v2
```

**Kubeflow:** Purpose-built Kubernetes-native platform for ML workflows. Provides pipelines (DAGs), Jupyter Notebooks, hyperparameter tuning (Katib), and model serving (KFServing/KServe) on top of Kubernetes.

**KServe (KFServing):** Kubernetes-native model serving framework. Supports TF, PyTorch, SKLearn, ONNX, XGBoost with canary rollouts, autoscaling, and explainability out-of-the-box.

---

## 18. Interview Q&A — Docker

**Q: What problem does Docker solve?**
> Environment inconsistency — "it works on my machine." Docker packages the app with all its dependencies so it runs identically everywhere: dev laptop, CI server, production cloud.

**Q: What is the difference between a container and a virtual machine?**
> Containers share the host OS kernel and are process-level isolation (lightweight, seconds to start). VMs emulate full hardware and run their own OS kernel (heavyweight, minutes to start). Containers trade some isolation for speed and efficiency.

**Q: Explain the Docker build process.**
> Docker reads a Dockerfile top to bottom. Each instruction that modifies the filesystem creates an immutable layer. Layers are cached — if the instruction and its inputs haven't changed, Docker reuses the cached layer. `docker build -t myapp:v1 .` sends the build context to the daemon, which executes each step.

**Q: How do you reduce Docker image size?**
> (1) Use slim/alpine base images. (2) Multi-stage builds — only ship runtime artifacts. (3) Chain RUN commands with `&&` to reduce layer count. (4) Use `.dockerignore` to exclude unnecessary files. (5) `--no-cache-dir` for pip. (6) Remove build tools after use.

**Q: What happens when a Docker container exits?**
> The container enters stopped state. Its writable layer still exists (accessible via `docker start`). All data in the writable layer is lost if you `docker rm` it. Data in volumes persists.

**Q: How do you pass secrets to a container securely?**
> (1) `docker run -e SECRET=val` for local dev. (2) `--env-file .env` for groups of vars. (3) Use Kubernetes Secrets or a secrets manager (Vault, AWS Secrets Manager) in production. Never bake secrets into the image.

**Q: What is Docker layer caching and how do you optimise for it?**
> Docker caches each layer. A change in any instruction invalidates the cache for that layer and all subsequent layers. Optimisation: put instructions that change rarely (OS packages, pip install) before instructions that change often (COPY source code). This way, rebuilds only re-execute the changed layers and everything after.

**Q: What is a multi-stage build and when would you use it in ML?**
> Multiple `FROM` statements in one Dockerfile. Each stage can copy artifacts from previous stages. For ML: Stage 1 installs training dependencies + trains model → Stage 2 copies the saved model + installs inference-only dependencies. Result: a small production image without CUDA build tools.

**Q: Difference between `EXPOSE` and `-p` flag?**
> `EXPOSE` in a Dockerfile is documentation — it tells other developers what port the app uses but doesn't actually open it. `-p 8080:80` in `docker run` actually binds the container port 80 to host port 8080.

**Q: What is Docker Compose and when would you use it over raw `docker run`?**
> Docker Compose manages multi-container applications declaratively via YAML. Use it when your app needs multiple services (API + database + cache + queue). In ML: model server + Redis cache + Postgres + MLflow all defined in one `docker-compose.yml` and started with one command.

**Q: How do you handle GPU access in Docker?**
> Install `nvidia-container-toolkit` on the host. Then: `docker run --gpus all my-ml-image` or `--gpus '"device=0"'` for specific GPUs. Use CUDA-enabled base images like `pytorch/pytorch:2.1.0-cuda11.8-...`.

**Q: What are Docker health checks and why are they important?**
> A `HEALTHCHECK` command Docker runs periodically inside the container. Returns 0 (healthy) or 1 (unhealthy). Allows Docker/Kubernetes to restart unhealthy containers automatically. Critical for ML services: a container can be running but the model might have failed to load.

---

## 19. Interview Q&A — Kubernetes

**Q: What is Kubernetes and why use it with Docker?**
> Docker runs containers on a single machine. Kubernetes orchestrates containers across a cluster of machines — handling scheduling, scaling, self-healing, load balancing, and upgrades automatically. Docker builds and runs; Kubernetes manages at scale.

**Q: What is the difference between a Pod and a container?**
> A container is a running process in an isolated environment. A Pod is the Kubernetes abstraction that wraps one or more containers sharing the same network (IP) and storage. Pods are the smallest deployable unit in Kubernetes, not individual containers.

**Q: Why do pods have multiple containers?**
> The **sidecar pattern**: a main container (app) + helper containers sharing resources. Examples: main API + log shipping sidecar + metrics exporter sidecar. In ML: inference container + a model-reload sidecar that watches for new model files.

**Q: Explain the difference between Deployment and StatefulSet.**
> Deployment is for stateless apps — pods are interchangeable. StatefulSet is for stateful apps — each pod has a stable identity (name, network, storage). Use StatefulSet for databases, Kafka, or ML training where each pod needs its own persistent checkpoint directory.

**Q: What happens when a pod dies?**
> The ReplicaSet controller detects the running count dropped below desired. It creates a new pod. The new pod gets a new IP but the Service's stable virtual IP remains unchanged, so traffic routing is unaffected.

**Q: What is the role of kube-proxy?**
> kube-proxy runs on every node and maintains iptables/IPVS rules that implement Services. When you send traffic to a Service's ClusterIP, kube-proxy routes it to one of the healthy backend pods.

**Q: How does a rolling update work in Kubernetes?**
> A rolling update replaces pods one by one (or in small batches). `maxSurge: 1` allows one extra pod to run during the update. `maxUnavailable: 0` ensures existing pods aren't killed until the new one is ready. Zero-downtime because traffic only routes to ready pods.

**Q: What is the difference between ClusterIP, NodePort, and LoadBalancer?**
> ClusterIP: accessible only within the cluster (service-to-service). NodePort: exposes on a static port on every node's IP (dev/testing). LoadBalancer: provisions a cloud load balancer with a public IP (production). For production traffic use LoadBalancer or Ingress.

**Q: What is an Ingress and why use it instead of LoadBalancer services?**
> Ingress provides L7 (HTTP) routing — one external IP, multiple backend services routed by path or hostname. LoadBalancer provisions one external IP per service (expensive). Ingress is cost-efficient and supports SSL termination, path routing, redirects.

**Q: What is the difference between liveness and readiness probes?**
> Liveness probe: is the container alive? Failure → restart it. Readiness probe: is the container ready to serve traffic? Failure → remove from Service endpoints (stop sending requests) but don't restart. Both matter for ML: readiness prevents traffic to a pod while the model is loading; liveness restarts a pod stuck in deadlock.

**Q: What is etcd and what happens if it goes down?**
> etcd is the cluster's distributed key-value database storing all state — what pods should be running, config, secrets, node info. If etcd goes down, the API server becomes read-only (can't create/update resources), existing pods keep running but can't be rescheduled. Always run etcd with 3 or 5 replicas and back it up regularly.

**Q: How does Kubernetes handle secrets? Is base64 encoding encryption?**
> No — base64 is encoding, not encryption. Secrets in etcd are stored as base64 by default. For real security: enable etcd encryption at rest, use RBAC to restrict access, or integrate an external secrets manager like HashiCorp Vault via the Secrets Store CSI driver.

**Q: What is a DaemonSet and when would you use it in ML?**
> DaemonSet ensures one pod runs on every node (or matching nodes). ML use cases: GPU metrics exporter on every GPU node, node-level log shipper, NVIDIA device plugin (required for GPU scheduling).

**Q: What is PodDisruptionBudget?**
> PDB defines minimum availability during voluntary disruptions (node drain, rolling updates). `minAvailable: 2` means at least 2 pods must be running at all times. Prevents accidentally taking down all replicas during cluster maintenance.

**Q: What is the difference between Kubernetes and Docker Swarm?**
> Both orchestrate containers, but Kubernetes is far more powerful and the industry standard. Kubernetes has richer scheduling, better autoscaling, more ecosystem support, and handles complex workloads. Docker Swarm is simpler to set up but limited for production ML workloads.

---

## 20. Interview Q&A — MLOps / AI/ML Scenarios

**Q: How would you deploy a PyTorch model to production using Kubernetes?**
> (1) Export model as TorchScript or ONNX. (2) Write a FastAPI inference server. (3) Build a Docker image (multi-stage: slim runtime image). (4) Push to registry. (5) Write Kubernetes Deployment with GPU resource requests, liveness/readiness probes, and resource limits. (6) Expose via a Service + Ingress. (7) Add HPA scaled on request latency or CPU. (8) Monitor with Prometheus/Grafana.

**Q: How do you handle model versioning in Kubernetes?**
> Tag Docker images with semantic versions (`model:v1.2.3`). Use Kubernetes Deployment with `image: myrepo/model:v1.2.3`. For rollback: `kubectl rollout undo deployment/model`. For controlled rollout: canary deployments (10% traffic to new version) before full switch.

**Q: How do you scale inference to handle traffic spikes?**
> Horizontal Pod Autoscaler (HPA) on CPU/memory or custom metrics (request latency, queue depth). Set `minReplicas` to handle baseline, `maxReplicas` for peak. For GPU inference, scale based on GPU utilisation using DCGM Exporter + Prometheus custom metrics.

**Q: How do you run distributed training on Kubernetes?**
> Use Kubernetes Jobs with `parallelism > 1` for simple data-parallel training. For PyTorch DDP or TensorFlow MirroredStrategy, use frameworks like **Kubeflow PyTorchJob** or **TF Job** that coordinate multi-pod distributed training with proper ring-allreduce communication.

**Q: How would you handle a model that takes 5 minutes to load?**
> Use `startupProbe` with `failureThreshold: 30` and `periodSeconds: 10` (= 5 minutes). This disables liveness and readiness until startup completes. Without this, the kubelet would kill the container before the model loads.

**Q: How do you store and share model artifacts between pods?**
> Use a PersistentVolumeClaim (PVC) with ReadWriteMany access mode (NFS, AWS EFS, GCP Filestore). One pod writes the trained model to the PVC; inference pods mount the same PVC read-only. Or use an object store (S3, GCS) and have each pod download on startup.

**Q: How does Docker help with reproducibility in ML?**
> A Docker image pins the Python version, CUDA version, library versions, and OS. The image is immutable — the exact same bits run everywhere. Combined with pinned package versions in `requirements.txt`, you get fully reproducible training and inference environments.

**Q: What is Kubeflow and why is it useful for ML?**
> Kubeflow is an ML toolkit built on Kubernetes. It provides: Pipelines (DAG-based workflow orchestration), Notebooks (JupyterHub), Katib (hyperparameter tuning), KServe (model serving). It lets data scientists and MLOps engineers use Kubernetes without needing deep k8s expertise.

**Q: How do you implement CI/CD for ML models with Docker and Kubernetes?**
> Trigger pipeline on git push → (1) Run tests (unit tests, data validation). (2) Build Docker image with new model code. (3) Push to registry with commit SHA tag. (4) Run integration tests in staging namespace. (5) Auto-deploy to production if tests pass using `kubectl set image` or Helm upgrade.

**Q: How would you troubleshoot a crashing ML pod in Kubernetes?**
> (1) `kubectl describe pod <name>` — check Events section for OOMKilled, image pull errors, probe failures. (2) `kubectl logs <name> --previous` — logs from the crashed container. (3) `kubectl exec -it <name> -- bash` — shell in if it's running. (4) Check resource limits — OOMKilled means not enough memory. (5) Check liveness probe — misconfigured probe may be killing a healthy pod.

**Q: What is the difference between batch inference and online inference in Kubernetes?**
> Online inference: always-running Deployment + Service, responds in real time, scaled with HPA. Batch inference: Kubernetes Job or CronJob, processes a dataset, terminates when done — cost-efficient (no idle pods). Use batch for nightly predictions over large datasets, online for real-time recommendations.

---

## 21. Quick Reference Cheat Sheet

### Docker Commands

```bash
# Images
docker pull image:tag          docker push repo/image:tag
docker build -t name:tag .     docker images
docker rmi image_id            docker image prune -a

# Containers
docker run -d -p 8080:80 --name app image
docker run -it image bash
docker ps -a                   docker stop/start/restart name
docker rm name                 docker rm $(docker ps -aq)
docker exec -it name bash      docker logs -f name
docker stats                   docker inspect name
docker cp name:/path ./path

# Volumes
docker volume create vol       docker volume ls
docker volume inspect vol      docker volume rm vol

# Networks
docker network create net      docker network ls
docker network connect net container

# Cleanup
docker system prune -a         # Remove everything unused
```

### kubectl Commands

```bash
# Context
kubectl config get-contexts    kubectl config use-context name

# Resources
kubectl get pods/deployments/services/nodes -n namespace
kubectl get all -n ml-production
kubectl describe pod/deployment/service name
kubectl apply -f manifest.yaml
kubectl delete -f manifest.yaml
kubectl delete pod name

# Debugging
kubectl logs pod-name --previous        # Crashed container logs
kubectl exec -it pod-name -- bash
kubectl describe pod pod-name           # Events + conditions
kubectl top pods / kubectl top nodes    # Resource usage

# Scaling
kubectl scale deployment name --replicas=5
kubectl autoscale deployment name --cpu-percent=70 --min=2 --max=10
kubectl rollout undo deployment/name    # Rollback

# Config
kubectl create configmap name --from-literal=KEY=val
kubectl create secret generic name --from-literal=KEY=val
kubectl create namespace ml-production
```

### Kubernetes Object Reference

```
Pod             → Wraps containers, smallest deployable unit
Deployment      → Manages stateless pod replicas + rolling updates
StatefulSet     → Pods with stable identity + persistent storage
DaemonSet       → One pod per node
Job             → Run-to-completion task
CronJob         → Scheduled Job
Service         → Stable network endpoint for pods
Ingress         → HTTP routing to multiple services
ConfigMap       → Non-sensitive config
Secret          → Sensitive data (base64)
PVC             → Storage request from a pod
PV              → Actual storage resource
HPA             → Autoscale pod count on metrics
Namespace       → Logical cluster isolation
ServiceAccount  → Pod identity for RBAC
Role/ClusterRole → Permissions
RoleBinding     → Assigns role to subject
```

### Resource Units

```
CPU:    1 = 1 core, 500m = 0.5 core, 100m = 0.1 core
Memory: 128Mi = 128 mebibytes, 4Gi = 4 gibibytes
GPU:    nvidia.com/gpu: "1" = 1 GPU (requires nvidia device plugin)
```

---

*Sources: Docker Official Docs, Kubernetes Official Docs, your uploaded notes, MLOps interview question banks from DataCamp, DataQuest, Edureka, InterviewBit, LockedIn AI, and Turing (2024–2026).*
