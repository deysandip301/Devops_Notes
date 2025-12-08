# K8s Services – Practice

---
## Q1. Exposing a Deployment with Services

### Step 1: Verify Namespace & Set Context
```bash
kubectl get ns svc-lab1
kubectl config set-context --current --namespace=svc-lab1
```

### Step 2: Create Deployment (nginx-deploy, 3 replicas)
Create DockerHub secret:
```bash
kubectl create secret docker-registry dockerhub-cred \
    --docker-username=YOUR_USERNAME \
    --docker-password=YOUR_PASSWORD \
    -n svc-lab1
```
Deployment YAML:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: svc-lab1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      imagePullSecrets:
        - name: dockerhub-cred
      containers:
        - name: nginx-container
          image: nginx
          ports:
            - containerPort: 80
```
Apply & Verify:
```bash
kubectl apply -f deployment.yaml
kubectl get deployment nginx-deploy
kubectl get pods
```

### Step 3: Expose Deployment as ClusterIP Service
```bash
kubectl expose deployment nginx-deploy \
    --name nginx-clusterip \
    --port 80 \
    --target-port 80 \
    --type ClusterIP
```

### Step 4: Check Service Details
```bash
kubectl get svc nginx-clusterip
```

### Step 5: Patch Service to NodePort (30080)
```bash
kubectl patch svc nginx-clusterip -p '{
    "spec" : {
        "type" : "NodePort",
        "ports" : [{
            "port" : 80,
            "targetPort" : 80,
            "nodePort" : 30080
        }]
    }
}'
```

### Step 6: Verify NodePort
```bash
kubectl get svc nginx-clusterip
```

---
## Q2. Understanding Kubernetes Services

### Step 1: Verify Namespace & Set Context
```bash
kubectl get ns svc-lab
sudo kubectl config set-context --current --namespace=svc-lab
```

### Step 2: Deployment YAML (web-deploy.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deploy
  namespace: svc-lab
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
kubectl apply -f web-deploy.yaml
kubectl get deployment
```

### Step 3: Service YAML (web-service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
  namespace: svc-lab
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 80
```
Apply & Verify:
```bash
kubectl apply -f web-service.yaml
kubectl get svc
```

### Step 4: Verify the SVC Endpoints
```bash
kubectl get endpoints web-service
```

---
## Q3. Exploring Kubernetes Service Types

### Step 1: Verify Namespace & Set Context
```bash
kubectl get ns svc-types-lab
sudo kubectl config set-context --current --namespace=svc-types-lab
```

### Step 2: Deployment YAML (2 Replicas)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: svc-types-lab
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
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
kubectl apply -f nginx-deploy.yaml
kubectl get deployment
```

### Step 3: ClusterIP Service (Internal Access)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-clusterip
  namespace: svc-types-lab
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```
Apply & Verify:
```bash
kubectl apply -f nginx-clusterip.yaml
kubectl get svc
```

### Step 4: NodePort Service (External Access)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
  namespace: svc-types-lab
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
# nodePort is auto-assigned if not specified
```
Apply & Verify:
```bash
kubectl apply -f nginx-nodeport.yaml
kubectl get svc
```

### Step 5: LoadBalancer Service (Cloud / Pending Locally)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-lb
  namespace: svc-types-lab
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```
Apply & Verify:
```bash
kubectl apply -f nginx-lb.yaml
kubectl get svc
```

### Step 6: Verify Endpoints
```bash
kubectl get endpoints nginx-clusterip
kubectl get endpoints nginx-nodeport
kubectl get endpoints nginx-lb
```

---
## Sample Logs
```
kubectl get deployment nginx-deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   0/2     2            0           3m25s
kubectl expose deployment nginx-deploy --name=nginx-clusterip --port=80 --target-port=80 --type=ClusterIP
service/nginx-clusterip exposed
kubectl patch svc nginx-clusterip ...
service/nginx-clusterip patched
```

---
Practice: Create deployments, expose with different service types, patch services, and verify endpoints.
