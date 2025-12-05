# K8s Deployment – Practice

---
## Q1. Deployments in Kubernetes

### Task 1: Create a Namespace (deployment-namespace.yaml)
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: deployment-namespace
```
Apply & Verify:
```bash
kubectl get namespaces
kubectl apply -f deployment-namespace.yaml
kubectl get namespaces
```

### DockerHub Secrets (Avoid Rate-Limit Errors)
```bash
kubectl create secret docker-registry dockerhub-secret \
    --docker-username=YOUR_USERNAME \
    --docker-password=YOUR_PASSWORD \
    -n deployment-namespace
kubectl get secrets -n deployment-namespace
```

### Task 2: Create a Deployment (deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
  namespace: deployment-namespace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      imagePullSecrets:
        - name: dockerhub-secret
      containers:
        - name: nginx-container
          image: nginx
          ports:
            - containerPort: 80
```
Apply & Verify:
```bash
kubectl apply -f deployment.yaml
kubectl get deployments -n deployment-namespace
kubectl get rs -n deployment-namespace
kubectl get pods -n deployment-namespace
```

---
## Sample Logs
```
kubectl get namespaces
NAME                  STATUS   AGE
kube-system           Active   ...
kube-public           Active   ...
kube-node-lease       Active   ...
default               Active   ...
deployment-namespace  Active   41s

kubectl apply -f deployment.yaml
deployment.apps/web-app-deployment created

kubectl get deployments -n deployment-namespace
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
web-app-deployment  3/3     3            3           13s
```

---
Practice: Create a namespace, DockerHub secret, and deployment. Verify deployment, ReplicaSet, and pods.
