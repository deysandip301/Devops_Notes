# Kubernetes Pods – Practice

---
## Q1. Kubernetes Namespace
```bash
kubectl get namespaces \
    --no-headers \
    -o custom-columns=NAME:.metadata.name \
    > answer.txt
```

---
## Q2. Pods in Kubernetes

### Phase 1: Namespace YAML (namespace.yaml)
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: assignment-namespace
```
Apply & Verify:
```bash
kubectl apply -f namespace.yaml
kubectl get namespaces
```

### Phase 2: Multi-Container Pod YAML (web-server-pod.yaml)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-server-pod
  namespace: assignment-namespace
spec:
  imagePullSecrets:
    - name: dockerhub-secret
  volumes:
    - name: shared-log
      emptyDir: {}
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: shared-log
          mountPath: /var/log/nginx
    - name: log-sidecar
      image: busybox
      command: ["sh", "-c", "tail -f /var/log/nginx/access.log"]
      volumeMounts:
        - name: shared-log
          mountPath: /var/log/nginx
```
Create DockerHub secret (if needed):
```bash
kubectl create secret docker-registry dockerhub-secret \
    --docker-username=YOUR_USER_NAME \
    --docker-password=YOUR_PASSWORD \
    --docker-email=YOUR_EMAIL \
    -n assignment-namespace
```
Apply & Verify:
```bash
kubectl apply -f web-server-pod.yaml
kubectl get pods -n assignment-namespace
kubectl describe pod web-server-pod -n assignment-namespace
```

### Phase 3: Access Application & Verify Logs
```bash
kubectl port-forward pod/web-server-pod 8080:80 -n assignment-namespace &
curl http://localhost:8080
kubectl logs web-server-pod -c log-sidecar -n assignment-namespace
```

---
## Sample Logs
```
... (nginx and alpine image pulls) ...
Handling connection for 8080
< !DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
... (nginx welcome page) ...
user@pod:~$ kubectl logs web-server-pod -c log-sidecar -n assignment-namespace
127.0.0.1 - - [timestamp] "GET / HTTP/1.1" 200 -
```

---
Practice: Create namespaces, multi-container pods with shared volumes, access apps, and verify logs.
