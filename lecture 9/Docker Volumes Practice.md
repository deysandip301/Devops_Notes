# Docker Volumes – Practice

---
Lecture Title: Docker Volumes  
Lecture Date: 19th Nov 2025 (Wednesday)
---

## Q1. Docker Volumes

### Step 1: Create Docker Volumes
```bash
docker volume create --label env=lab app_data
docker volume create logs_data
docker volume ls
```

### Step 2: Inspect Docker Volumes
```bash
docker volume inspect app_data
docker volume inspect logs_data
```

### Step 3: Run app_container with Volume Mounted
```bash
docker run -d \
    --name app_container \
    -v app_data:/usr/share/nginx/html \
    nginx
docker ps
```

### Step 4: Run log_container with Logs Volume
```bash
docker run -dit \
    --name log_container \
    -v logs_data:/logs \
    alpine sh
docker ps
```

### Step 5: Write Data to app_data Volume
```bash
docker exec app_container sh -c "echo 'Hello from app_container 1' > /usr/share/nginx/html/index.html"
docker exec app_container cat /usr/share/nginx/html/index.html
```

### Step 6: Share Volume with Another Container
```bash
docker run -d \
    --name app_container_2 \
    -v app_data:/usr/share/nginx/html \
    nginx
docker exec app_container_2 cat /usr/share/nginx/html/index.html
```

### Step 7: List All Volumes
```bash
docker volume ls
```

### Post Lab Cleanup
```bash
docker volume rm app_data logs_data
docker volume ls
docker container rm app_container app_container_2 log_container
docker container ls -a
```

---
## Important Docker Volume Commands
- `docker volume create <name>`: Create a volume
- `docker volume create --label key=value <name>`: Create with metadata
- `docker volume ls`: List volumes
- `docker volume inspect <name>`: Inspect volume
- `docker volume rm <name>`: Remove volume
- `docker run -v <volume>:<container_path> <image>`: Mount volume

---
## Sample Logs
```
bash-5.1$ docker run -d --name=app_container_2 -v app_data:/usr/share/nginx/html nginx
... (container created)
bash-5.1$ docker run -dit --name log_container -v logs_data:/logs alpine sh
... (container created)
bash-5.1$ docker volume ls
DRIVER    VOLUME NAME
local     app_data
local     logs_data
... (other logs)
```

---
Practice: Create, inspect, mount, share, and clean up Docker volumes.
