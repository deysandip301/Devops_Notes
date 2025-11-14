# Create Your Own Docker Images – Practice

## Q1. Dockerfile for a Java App
```Dockerfile
# FROM openjdk:latest (deprecated)
FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY HelloWorld.java /app
RUN javac HelloWorld.java
CMD ["java", "HelloWorld"]
```
Build the image:
```bash
docker build -t hello-world .
# or specify Dockerfile
# docker build -t hello-world -f Dockerfile .
```
- `-t`: Tag (name) of the image
- `-f`: Path to Dockerfile
- `.`: Build context (current directory)

## Q2. Building a Simple Docker Image
```Dockerfile
FROM alpine:latest
CMD ["echo", "Hello DevOps"]
```
Build and run:
```bash
docker build -t hello-devops .
docker run --rm hello-devops
```

## Sample Logs
```
bash-5.1$ docker build -t java-hello-world .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM eclipse-temurin:17-jdk
... (pulling and extracting layers) ...
Successfully built <image_id>
Successfully tagged java-hello-world:latest
bash-5.1$ docker run java-hello-world
Hello, World!

bash-5.1$ docker build -t hello-devops .
Sending build context to Docker daemon
Step 1/2 : FROM alpine:latest
... (pulling and extracting layers) ...
Successfully built <image_id>
Successfully tagged hello-devops:latest
bash-5.1$ docker images
REPOSITORY      TAG     IMAGE ID      CREATED         SIZE
hello-devops    latest  <image_id>    seconds ago     8.44MB
alpine          latest  <image_id>    weeks ago       8.44MB
```

---
Practice: Write Dockerfiles, build images, run containers, and observe logs.
