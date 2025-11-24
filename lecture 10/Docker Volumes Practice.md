# Docker Networking – Practice

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
## Sample Logs
```
bash-5.1$ docker login --username=deysandip301
Password:
Login Succeeded
bash-5.1$ docker volume create --label env=lab app_data
bash-5.1$ docker volume create logs_data
bash-5.1$ docker run -d --name app_container -v app_data:/usr/share/nginx/html nginx
... (image pull and container creation logs) ...
bash-5.1$ docker run -dit --name log_container -v logs_data:/logs alpine sleep inf
... (image pull and container creation logs) ...
bash-5.1$ docker run -d --name app_container_2 -v app_data:/usr/share/nginx/html nginx
... (container creation logs) ...
```

---
Practice: Create, inspect, mount, share, and clean up Docker volumes. Review logs for each step.
