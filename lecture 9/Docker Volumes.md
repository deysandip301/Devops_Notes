# Docker Volumes

## Why Use Volumes?
- Containers are ephemeral; data in writable layer is lost when container stops.
- Volumes persist data beyond container lifecycle.
- Use for databases, uploads, logs, configs, and persistent app storage.

## Types of Docker Volumes
1. **Anonymous Volume**
   - Created automatically, random name.
   - Example: `docker run -d -v /data nginx`
   - Good for temporary storage.
2. **Named Volume**
   - User-defined, managed by Docker.
   - Example: `docker volume create mydata`
     `docker run -d -v mydata:/var/lib/mysql mysql:8`
   - Data persists after container removal.
3. **Bind Mount**
   - Mounts host directory into container.
   - Example: `docker run -d -v $(pwd)/website:/usr/share/nginx/html nginx`
   - Host changes reflect in container; best for development.
4. **tmpfs Mount**
   - Mounts directory in RAM (not persistent).
   - Example: `docker run -d --tmpfs /cache nginx`
   - Fast, secure, temporary data.

## Where Is Data Stored?
- Volumes: `/var/lib/docker/volumes/`
- Bind mounts: Host filesystem (not managed by Docker)

## Real-World Use Cases
- Named volume: Databases, persistent app data
- Bind mount: Live code, configs, logs
- tmpfs: Caches, sensitive data
- Anonymous: Temporary containers

## Common Commands
- List volumes: `docker volume ls`
- Inspect volume: `docker volume inspect <name>`
- Create volume: `docker volume create mydata`
- Use volume: `docker run -v mydata:/path image`
- Bind mount: `docker run -v /host/path:/container/path image`
- tmpfs: `docker run --tmpfs /cache image`

## Summary Table
| Type         | Persistent | Managed by Docker | Best For           |
|--------------|------------|-------------------|--------------------|
| Anonymous    | Yes        | Yes               | Temporary storage  |
| Named        | Yes        | Yes               | Databases, uploads |
| Bind Mount   | Host       | No                | Development, logs  |
| tmpfs        | No (RAM)   | Yes               | Secure, fast cache |

---
Focus: Persisting data, volume types, commands, and use cases.
