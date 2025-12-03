# K8s ReplicaSet – Practice

---
## Q1. Replicaset in Kubernetes

### Task 1: Create a Namespace (replica-set-namespace.yaml)
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: replica-set-namespace
```
Apply & Verify:
```bash
kubectl get namespaces
kubectl apply -f replica-set-namespace.yaml
kubectl get namespaces
```

### Task 2: Create a Pod with a Label (labeled-pod.yaml)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: labeled-pod
  namespace: replica-set-namespace
  labels:
    app: my-app
spec:
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 80
```
Apply & Verify:
```bash
kubectl get pods -n replica-set-namespace
kubectl apply -f labeled-pod.yaml
kubectl get pods -n replica-set-namespace
```

### Task 3: Create a Replica Set (replica-set.yaml)
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app-replicaset
  namespace: replica-set-namespace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: nginx-container
          image: nginx
          ports:
            - containerPort: 80
```
Apply & Verify:
```bash
kubectl get rs -n replica-set-namespace
kubectl get pods -n replica-set-namespace
kubectl describe rs my-app-replicaset -n replica-set-namespace
```

---
## Sample Logs
```
user@b5d7d49fa5c4:~$ kubectl apply -f replica-set.yaml
replicaset.apps/my-app-replicaset created
user@b5d7d49fa5c4:~$ kubectl get pods -n replica-set-namespace
NAME              READY   STATUS    RESTARTS   AGE
labeled-pod       1/1     Running   0          27m
my-app-replicaset-7ksw4   1/1     Running   0          11s
my-app-replicaset-m2rms   1/1     Running   0          11s
my-app-replicaset-xxxxx   1/1     Running   0          11s
```

---
Practice: Create a namespace, labeled pod, and ReplicaSet. Verify pod creation and ReplicaSet status.
