# Intro to Docker – Concise Notes

## 1. What is Docker?
- Open-source platform for building, packaging, and running applications in containers.
- Containers: Lightweight, portable, isolated environments with app + dependencies.

## 2. Why Docker?
- Solves dependency conflicts and "works on my machine" issues.
- Consistent environments across dev, test, and prod.
- Easy to start, stop, copy, and remove apps.

## 3. Key Concepts
- **Image:** Read-only blueprint for containers (built from Dockerfile).
- **Container:** Running instance of an image.
- **Dockerfile:** Instructions to build an image.
- **Docker Engine:** Runs and manages containers.
- **Docker Hub:** Public image repository.
- **Docker Compose:** Tool for multi-container apps.

## 4. Docker Architecture
- Docker CLI (Client) ↔ Docker Daemon (Server)
- Daemon builds, runs, and manages containers.
- Containers run on the host OS.

## 5. Installing Docker (Linux)
- Official script: `curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh`
- Or: `sudo apt update && sudo apt install docker.io -y`
- Start: `sudo systemctl enable docker && sudo systemctl start docker`
- Verify: `docker --version`, `docker run hello-world`

## 6. Basic Docker Commands
- `docker --version` – Check version
- `docker run hello-world` – Test setup
- `docker ps` – List running containers
- `docker ps -a` – List all containers
- `docker images` – List images
- `docker pull nginx` – Download image
- `docker run -it ubuntu bash` – Interactive shell
- `docker stop <container_id>` – Stop container
- `docker rm <container_id>` – Remove container
- `docker rmi <image_id>` – Remove image

## 7. Docker Images & Layers
- Image: Read-only, layered template for containers.
- Each Dockerfile command (FROM, RUN, COPY, etc.) creates a new layer.
- Layers are stacked (base OS → app install → configs → metadata).
- Container adds a writable layer on top of image layers.

## 8. Union File System
- Merges all layers into a single view (copy-on-write).
- Modified files are copied to the writable layer.

## 9. Image Storage (Linux)
- Default: `/var/lib/docker/overlay2/`
- Layers stored as directories; metadata in `imagedb` and `layerdb`.

## 10. Inspecting Images & Layers
- `docker image inspect <image>` – View image details
- `docker history <image>` – Show image layers

## 11. Layer Reuse & Caching
- Layers are identified by SHA256 hashes.
- Shared layers are reused across images for efficiency.

---
Focus: Images, containers, Dockerfile, basic commands, and how layers work.
