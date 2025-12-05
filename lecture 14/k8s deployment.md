# K8s Deployment

## Pod (Basic Workload Unit)
- Smallest deployable unit in Kubernetes.
- Runs one or more containers (usually 1), shares network/storage, single IP.
- Ephemeral: not self-healing; if it dies, it's gone.

## ReplicaSet
- Ensures a specified number of Pod replicas are always running.
- Replaces failed Pods, deletes extras, but does not handle rolling updates or rollbacks.
- Usually managed by Deployments.

## Deployment (Recommended Controller)
- Manages ReplicaSets and Pods declaratively.
- Handles rolling updates, rollbacks, pause/resume, and self-healing.
- Declarative updates and revision history.

### Basic Deployment YAML
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: app
        image: nginx:1.27
        ports:
        - containerPort: 80
```

## Deployment Workflow
1. `kubectl apply -f deployment.yaml` → API server validates and stores in etcd.
2. Deployment controller checks for existing RS, creates/updates as needed.
3. ReplicaSet ensures correct number of Pods.
4. Scheduler assigns Pods to nodes.
5. Kubelet pulls image, creates Pod sandbox, starts containers.

## Rollout Strategies
- **RollingUpdate (default):** Gradually replaces old Pods with new ones (maxUnavailable, maxSurge).
- **Recreate:** Deletes all old Pods before creating new ones (for stateful apps).

## etcd Storage
- Deployments, ReplicaSets, and Pods are stored as key-value pairs in etcd.
- Deployments: `/registry/deployments/<namespace>/<deployment-name>`
- ReplicaSets: `/registry/replicasets/<namespace>/<rs-name>`
- Pods: `/registry/pods/<namespace>/<pod-name>`
- Rollouts/rollbacks tracked via revision annotations and separate ReplicaSets.

## Self-Healing & Reconciliation
- Controllers compare desired state (etcd) with actual state (kubelets).
- If mismatch, controllers fix it (e.g., recreate missing Pods).

## Common Deployment Operations
- Update image: `kubectl set image deployment/webapp app=nginx:1.28`
- Check rollout: `kubectl rollout status deployment/webapp`
- View history: `kubectl rollout history deployment/webapp`
- Rollback: `kubectl rollout undo deployment/webapp`
- Pause/resume: `kubectl rollout pause|resume deployment/webapp`
- Scale: `kubectl scale deployment webapp --replicas=10`
- Delete: `kubectl delete deployment webapp`

---
Focus: Pod, ReplicaSet, Deployment, workflow, rollout strategies, etcd storage, and key operations.
