# Docker & Containerization Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) · Intermediate (Q21–Q40) · Advanced (Q41–Q50)

---

## Basic (Q1–Q20)

### Q1: What is Docker?

**Answer:** Docker is a platform for developing, shipping, and running applications inside **containers** — lightweight, standalone packages that include everything needed to run (code, runtime, libraries, configs). It ensures consistency across environments.

**💡 Tip:** Docker = shipping container for code. Same container runs identically on your laptop, staging, and production.

---

### Q2: What is the difference between a container and a virtual machine?

**Answer:** VMs virtualize hardware and run a full OS with their own kernel. Containers share the host OS kernel and only package the application and its dependencies — making them much lighter and faster to start.

**💡 Tip:** VM = house with its own foundation. Container = apartment sharing the building's foundation (kernel) but with its own furniture.

---

### Q3: What is a Docker image?

**Answer:** A read-only template with instructions for creating a container. It includes the application code, runtime, libraries, and configuration. Images are built from a Dockerfile and stored in registries.

**💡 Tip:** Image = blueprint/class. Container = running instance/object. One image can create many containers.

---

### Q4: What is a Dockerfile?

**Answer:** A text file containing step-by-step instructions to build a Docker image. Each instruction creates a layer in the image (e.g., `FROM`, `RUN`, `COPY`, `CMD`).

**💡 Tip:** Dockerfile = recipe. Image = cooked dish. Container = served meal on the table.

---

### Q5: What is a Docker container?

**Answer:** A runnable instance of a Docker image. It's an isolated process with its own filesystem, network, and resources, running on top of the host OS kernel.

**💡 Tip:** Container = a running image. `docker run <image>` creates a container from an image.

---

### Q6: What is a Docker registry?

**Answer:** A storage and distribution system for Docker images. Docker Hub is the default public registry. Organizations use private registries (AWS ECR, GCR, Azure ACR) for internal images.

**💡 Tip:** Registry = app store for Docker images. `docker push` uploads, `docker pull` downloads.

---

### Q7: What is Docker Hub?

**Answer:** Docker's default public registry hosting over 100,000 container images. It provides official images (e.g., `node`, `postgres`, `nginx`) and lets users publish their own images.

**💡 Tip:** Docker Hub = npm/GitHub for Docker images. Always use **official images** as your base.

---

### Q8: What does `docker build` do?

**Answer:** It reads a Dockerfile and builds a Docker image, executing each instruction as a layer. The `-t` flag tags the image with a name (e.g., `docker build -t myapp .`).

**💡 Tip:** `docker build -t name:tag .` — the `.` means "use the current directory as build context."

---

### Q9: What does `docker run` do?

**Answer:** It creates and starts a new container from an image. Common flags: `-d` (detached), `-p` (port mapping), `-v` (volume), `-e` (environment variables), `--name` (container name).

**💡 Tip:** `docker run = create + start`. Use `docker start` to restart a stopped container.

---

### Q10: What is the difference between `docker run` and `docker start`?

**Answer:** `docker run` creates a **new** container from an image and starts it. `docker start` starts an **existing stopped** container.

**💡 Tip:** Run = new birth. Start = wake up from sleep. Run always creates; start reuses.

---

### Q11: What does `docker ps` show?

**Answer:** It lists running containers. Use `docker ps -a` to show all containers (including stopped ones).

**💡 Tip:** `ps` = process status (from Linux). Default shows only running. `-a` = all.

---

### Q12: What is the `FROM` instruction in a Dockerfile?

**Answer:** It specifies the base image to build from. Every Dockerfile must start with `FROM` (except in multi-stage builds where later stages also use `FROM`).

**💡 Tip:** `FROM` = your starting point. `FROM node:20-alpine` = start with a minimal Node.js 20 environment.

---

### Q13: What is the difference between `CMD` and `ENTRYPOINT`?

**Answer:**
- **CMD** — provides default command/arguments that can be overridden from `docker run`
- **ENTRYPOINT** — defines the executable that always runs; arguments from `docker run` are appended to it

**💡 Tip:** ENTRYPOINT = the program. CMD = the default arguments. Combined: ENTRYPOINT defines the executable, CMD provides defaults.

---

### Q14: What does `docker-compose` do?

**Answer:** Docker Compose is a tool for defining and running multi-container applications. You define services in a `docker-compose.yml` file and start everything with `docker-compose up`.

**💡 Tip:** Compose = orchestrator for dev. One YAML file defines your entire stack (app + database + cache).

---

### Q15: What are Docker volumes?

**Answer:** Volumes are the preferred mechanism for persisting data generated by containers. They're stored on the host filesystem but managed by Docker, surviving container removal.

**💡 Tip:** Volume = external hard drive for containers. Without volumes, data is deleted when the container is removed.

---

### Q16: What is the difference between `COPY` and `ADD` in a Dockerfile?

**Answer:**
- **COPY** — copies files from host to the container (straightforward)
- **ADD** — does the same but also supports auto-extracting tar archives and remote URLs

**💡 Tip:** Always prefer `COPY` — it's explicit and predictable. Only use `ADD` when you need tar extraction.

---

### Q17: What is `docker exec`?

**Answer:** It runs a command inside a running container. Commonly used to get a shell: `docker exec -it <container> sh` or `docker exec -it <container> bash`.

**💡 Tip:** `exec` = "execute this inside the running container." `-it` = interactive + TTY (for shells).

---

### Q18: What are Docker networks?

**Answer:** Networks enable communication between containers. By default, Docker creates a bridge network. Custom networks allow containers to communicate by name (automatic DNS resolution).

**💡 Tip:** Default bridge = IP-based communication. Custom networks = name-based communication. Always use custom networks for multi-container apps.

---

### Q19: What is layer caching in Docker?

**Answer:** Each instruction in a Dockerfile creates a layer. Docker caches layers and reuses them if the instruction hasn't changed. If a layer changes, all subsequent layers are rebuilt.

**💡 Tip:** Order matters! Put rarely changing instructions (like `FROM`, `RUN apt-get`) first, and frequently changing ones (like `COPY . .`) last to maximize cache hits.

---

### Q20: What does `docker stop` vs `docker kill` do?

**Answer:** `docker stop` sends SIGTERM (graceful shutdown), then SIGKILL after a timeout (10s). `docker kill` sends SIGKILL immediately (forceful termination).

**💡 Tip:** Stop = "please wrap up and exit." Kill = "exit NOW, no questions asked." Always prefer stop.

---

## Intermediate (Q21–Q40)

### Q21: What is a multi-stage build?

**Answer:** A Dockerfile with multiple `FROM` statements where each stage can copy artifacts from previous stages. This allows you to build in one stage (with build tools) and create a minimal final image (without build tools).

**💡 Tip:** Multi-stage = build phase + runtime phase. The final image only contains what's needed to run, not to build. Reduces image size dramatically.

---

### Q22: What is the difference between Docker Compose and Kubernetes?

**Answer:** Docker Compose orchestrates containers on a single host for development. Kubernetes orchestrates containers across multiple hosts (cluster) for production, with auto-scaling, self-healing, and rolling updates.

**💡 Tip:** Compose = dev/simple deployments on one machine. Kubernetes = production-grade orchestration across a cluster.

---

### Q23: What is the `.dockerignore` file?

**Answer:** Similar to `.gitignore`, it specifies files and directories to exclude from the build context (e.g., `node_modules`, `.git`, `README.md`). This reduces build context size and speeds up builds.

**💡 Tip:** Always create `.dockerignore`. Without it, `COPY . .` copies everything including `node_modules` — making builds slow and images bloated.

---

### Q24: How do you pass environment variables to a Docker container?

**Answer:**
- `-e VAR=value` with `docker run`
- `--env-file .env` to load from a file
- Define in `docker-compose.yml` under `environment:` or `env_file:`

**💡 Tip:** Never hardcode secrets in Dockerfiles. Use environment variables or Docker secrets for sensitive data.

---

### Q25: What is the difference between bind mounts and volumes?

**Answer:**
- **Bind mounts** — map a specific host directory to a container directory. Host-dependent.
- **Volumes** — managed by Docker, stored in Docker's area on the host. Portable and independent of host structure.

**💡 Tip:** Volumes = Docker-managed, portable, recommended. Bind mounts = host-dependent, useful for development (live reload).

---

### Q26: What is Docker networking? Name the network drivers.

**Answer:** Docker networking connects containers to each other and to the outside world. Network drivers:
- **bridge** — default, for standalone containers
- **host** — shares the host's network stack (no isolation)
- **overlay** — connects containers across multiple hosts (Swarm)
- **none** — disables networking
- **macvlan** — assigns MAC addresses to containers

**💡 Tip:** Bridge = most common (single host). Overlay = multi-host. Host = maximum performance, zero isolation.

---

### Q27: What is the Docker daemon (dockerd)?

**Answer:** The background service that manages Docker objects (images, containers, networks, volumes). The Docker CLI communicates with the daemon via REST API. The daemon listens on a Unix socket (`/var/run/docker.sock`).

**💡 Tip:** CLI = the remote control. Daemon = the TV. The CLI sends commands to the daemon which does the actual work.

---

### Q28: What is the Docker build cache and how do you invalidate it?

**Answer:** Docker caches each layer during build. If a layer's instruction and context haven't changed, Docker reuses the cached layer. Invalidation methods:
- Change the instruction
- Use `--no-cache` flag
- Change files being copied (if `COPY` is before the layer)

**💡 Tip:** `docker build --no-cache` = "start fresh, ignore all caches." Useful when builds behave unexpectedly.

---

### Q29: What is `docker system prune`?

**Answer:** It removes all unused data: stopped containers, dangling images, unused networks, and build cache. `docker system prune -a` also removes all images not used by any container.

**💡 Tip:** Prune = spring cleaning. Use it to reclaim disk space. `-a` is aggressive — it removes ALL unused images.

---

### Q30: How do you reduce Docker image size?

**Answer:**
1. Use minimal base images (`alpine`, `distroless`)
2. Use multi-stage builds
3. Minimize layers (combine `RUN` commands with `&&`)
4. Use `.dockerignore`
5. Don't install unnecessary packages
6. Clean up in the same layer (`RUN apt-get install ... && rm -rf /var/lib/apt/lists/*`)

**💡 Tip:** Alpine = ~5MB base vs Ubuntu = ~77MB. Multi-stage builds can cut 80%+ off image size.

---

### Q31: What is Docker Swarm?

**Answer:** Docker's native clustering and orchestration tool for managing a cluster of Docker nodes. It provides service discovery, load balancing, rolling updates, and scaling — but is less feature-rich than Kubernetes.

**💡 Tip:** Swarm = lightweight Kubernetes. Good for simple orchestration needs. Production-heavy workloads → Kubernetes.

---

### Q32: What is a Docker image tag?

**Answer:** A label applied to an image to identify a specific version or variant (e.g., `node:20-alpine`, `nginx:latest`). Tags are mutable — the same tag can point to different images over time.

**💡 Tip:** `latest` is just a default tag, not actually "the latest." Always pin specific versions in production (e.g., `node:20.10.0-alpine`).

---

### Q33: What is the difference between `EXPOSE` and `-p` (port mapping)?

**Answer:** `EXPOSE` in a Dockerfile is documentation — it declares which port the container listens on but doesn't publish it. `-p 8080:80` in `docker run` actually maps host port 8080 to container port 80.

**💡 Tip:** `EXPOSE` = documentation only. `-p` = actual port forwarding. You need `-p` (or `-P`) to access the container from outside.

---

### Q34: What is the `WORKDIR` instruction?

**Answer:** It sets the working directory for subsequent `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions in the Dockerfile. If the directory doesn't exist, Docker creates it.

**💡 Tip:** Always use `WORKDIR` instead of `RUN cd ...`. `WORKDIR /app` = all following commands run in `/app`.

---

### Q35: What are dangling images?

**Answer:** Images that have no tag and are not referenced by any container. They accumulate from failed or intermediate builds. Remove them with `docker image prune`.

**💡 Tip:** Dangling = orphaned images with `<none>` tag. Safe to delete — they're build artifacts nobody uses.

---

### Q36: How do you view logs from a Docker container?

**Answer:** `docker logs <container>` shows stdout/stderr from the container. `docker logs -f <container>` follows (tails) the logs in real-time. `docker logs --tail 100` shows the last 100 lines.

**💡 Tip:** `docker logs -f` = `tail -f` for containers. The container must write to stdout/stderr for logs to appear.

---

### Q37: What is the `HEALTHCHECK` instruction in a Dockerfile?

**Answer:** It defines a command that Docker periodically runs to check if the container is healthy. Docker reports the status as `starting`, `healthy`, or `unhealthy`.

**💡 Tip:** `HEALTHCHECK CMD curl -f http://localhost/ || exit 1` — without health checks, Docker only knows if the process is running, not if the app is actually working.

---

### Q38: What is the difference between `docker-compose up` and `docker-compose start`?

**Answer:** `up` builds (if needed), creates, and starts all services defined in the YAML. `start` only starts existing stopped containers — it doesn't build or create new ones.

**💡 Tip:** `up` = full startup (build + create + start). `start` = restart stopped containers. Use `up -d` for detached mode.

---

### Q39: What is the `USER` instruction in a Dockerfile?

**Answer:** It sets the user (and optionally group) for subsequent instructions and for the running container. Best practice: don't run as root — create and switch to a non-root user.

**💡 Tip:** `RUN adduser -D appuser && USER appuser` = security best practice. Running as root in containers is a security risk.

---

### Q40: What is the difference between Docker Compose `v2` and `v3`?

**Answer:** v3 is designed for swarm deployment and production. Key changes: removed `volumes_from`, `extends`, and `cpu_shares`. Added `deploy` section for replicas, resources, and placement constraints.

**💡 Tip:** v2 = for `docker-compose` (standalone). v3 = for swarm/stack deployments. Most projects now use the Compose Specification (no version field needed).

---

## Advanced (Q41–Q50)

### Q41: What is container namespace isolation?

**Answer:** Linux namespaces provide isolation for containers:
- **PID** — process isolation (container sees only its own processes)
- **NET** — network isolation (own network stack)
- **MNT** — filesystem mount isolation
- **UTS** — hostname isolation
- **IPC** — inter-process communication isolation
- **USER** — user ID isolation (map container users to host users)

**💡 Tip:** Namespaces = walls between containers. Each namespace makes the container think it has its own OS.

---

### Q42: What are cgroups in Docker?

**Answer:** Control Groups (cgroups) limit and monitor resource usage per container: CPU, memory, disk I/O, network. They prevent one container from consuming all host resources.

**💡 Tip:** Namespaces = isolation (what you can see). Cgroups = limits (what you can use). `docker run --memory=512m --cpus=1` sets cgroup limits.

---

### Q43: What is the Docker storage driver?

**Answer:** The storage driver manages how image layers are stacked and how the container's writable layer works. Common drivers: `overlay2` (default, recommended), `devicemapper`, `btrfs`, `zfs`.

**💡 Tip:** `overlay2` = the default and recommended driver. It uses OverlayFS to efficiently layer filesystems. Don't change it unless you have a specific reason.

---

### Q44: What is a rootless Docker container?

**Answer:** Running Docker without root privileges using user namespaces. The Docker daemon and containers run as a non-root user, reducing the attack surface if a container is compromised.

**💡 Tip:** Rootless = security hardening. If a container escapes, the attacker only has non-root privileges. Enabled with `dockerd-rootless.sh`.

---

### Q45: How do you debug a failing container?

**Answer:**
1. `docker logs <container>` — check application logs
2. `docker inspect <container>` — check configuration and state
3. `docker exec -it <container> sh` — get a shell inside
4. `docker events` — see daemon events
5. `docker diff <container>` — see filesystem changes
6. Check exit code: `docker inspect --format='{{.State.ExitCode}}' <container>`

**💡 Tip:** Exit codes matter: 0 = clean exit, 1 = application error, 137 = OOM killed (out of memory), 139 = segfault.

---

### Q46: What is the Docker BuildKit?

**Answer:** An improved build engine (default since Docker 23.0) that provides parallel build stages, better caching, secrets mounting, and SSH forwarding. Enable with `DOCKER_BUILDKIT=1` or set in daemon config.

**💡 Tip:** BuildKit = faster, smarter builds. `RUN --mount=type=cache` lets you cache package installs across builds.

---

### Q47: How do you implement secrets management in Docker?

**Answer:**
- **Docker Swarm secrets** — encrypted at rest, mounted as files in `/run/secrets/`
- **BuildKit secrets** — `RUN --mount=type=secret` for build-time secrets (not saved in image)
- **Environment variables** — for non-sensitive config
- **External tools** — HashiCorp Vault, AWS Secrets Manager with sidecar patterns

**💡 Tip:** Never put secrets in Dockerfile or image layers. Use `--mount=type=secret` for builds, Swarm secrets or external vaults for runtime.

---

### Q48: What is the difference between Docker and containerd?

**Answer:** containerd is the container runtime that Docker uses under the hood. Docker = CLI + daemon + containerd + runc. Kubernetes can use containerd directly (without Docker), which is the standard in modern K8s.

**💡 Tip:** Docker = the user-friendly package. containerd = the core runtime. Kubernetes deprecated Docker as a runtime in v1.24 but still uses containerd.

---

### Q49: What is a distroless image?

**Answer:** Images from Google that contain only your application and its runtime dependencies — no shell, no package manager, no OS utilities. They have a minimal attack surface and smaller size.

**💡 Tip:** Distroless = no shell means attackers can't `sh` into your container. `gcr.io/distroless/nodejs20` — smallest secure runtime image.

---

### Q50: How do you optimize Docker for production?

**Answer:**
1. Pin base image versions (no `:latest`)
2. Use multi-stage builds
3. Run as non-root user (`USER`)
4. Add `HEALTHCHECK`
5. Set resource limits (`--memory`, `--cpus`)
6. Use `.dockerignore`
7. Scan images for vulnerabilities (`docker scout`, Trivy)
8. Use minimal base images (Alpine, distroless)
9. Don't store secrets in images
10. Log to stdout/stderr (not files)

**💡 Tip:** Production Docker = security + performance + reliability. Every image should be: minimal, non-root, version-pinned, health-checked, and vulnerability-scanned.
