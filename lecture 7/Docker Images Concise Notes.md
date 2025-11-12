# Docker Images – Concise Notes

## 1. What is a Docker Image?
- Read-only, layered filesystem built from a Dockerfile.
- Blueprint for containers; running an image creates a container with a writable layer.

## 2. Dockerfile Fundamentals
- `FROM <image>`: Base image (always first, except ARG).
- `RUN <command>`: Execute commands during build (creates new layer).
- `CMD ["executable", ...]`: Default command for container (one per Dockerfile).
- `ENTRYPOINT ["executable", ...]`: Main command, CMD provides default args.
- `COPY src dest`: Copy files from host to image (use for local files).
- `ADD src dest`: Like COPY, but supports remote URLs and auto-extracts archives.
- `ENV KEY=VALUE`: Set environment variables.

## 3. Best Practices
- Use minimal base images (`-alpine`, `-slim`).
- Pin versions for reproducibility.
- Combine RUN commands to reduce layers.
- Clean up caches/temp files in RUN.
- Use multi-stage builds for smaller images.
- Use `COPY` over `ADD` unless extra features needed.
- Use `USER` to avoid running as root.
- Use `HEALTHCHECK` for container health.
- Never hardcode secrets; use env vars or Docker secrets.

## 4. Example: Minimal Dockerfile
```Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
ENV PORT=8080
EXPOSE 8080
ENTRYPOINT ["python"]
CMD ["app.py"]
```

## 5. Build & Run
- Build: `docker build -t flask-app:latest .`
- Run: `docker run -p 8080:8080 flask-app`
- Access: http://localhost:8080

## 6. Inspecting Images
- List layers: `docker history <image>`
- Inspect metadata: `docker inspect <image>`
- Scan for vulnerabilities: `docker scan <image>` or `trivy image <image>`

## 7. References
- Example repo: https://github.com/vilasvarghese/docker-k8s/dockerfiles
- Build with custom Dockerfile: `docker build -t <image> -f <file> .`

---
Focus: Dockerfile instructions, best practices, building, running, and inspecting images.
