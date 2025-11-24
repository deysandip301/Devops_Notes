# Intro to Kubernetes

## What is Kubernetes?
- Open-source container orchestration platform.
- Automates deployment, scaling, healing, networking, storage, and updates for containers.
- Provides self-healing and declarative state management.

## Architecture Overview
- Master–worker (control plane–data plane) architecture.
- Control Plane (Master): The "brain" of the cluster.
- Worker Nodes: The "muscle" running your apps (pods).

## Control Plane Components
- **kube-apiserver:** Entry point for all commands (kubectl, REST API). Validates and processes requests, stores state in etcd.
- **etcd:** Distributed key-value store for all cluster data (config, state, secrets, pod definitions).
- **kube-scheduler:** Assigns pods to nodes based on resources, constraints, and policies.
- **kube-controller-manager:** Runs control loops to ensure actual state matches desired state (node, deployment, job, volume controllers, etc.).
- **cloud-controller-manager:** Integrates with cloud providers for load balancers, storage, and networking (cloud-specific controllers).

## Data Plane Components (Worker Nodes)
- **kubelet:** Node agent that ensures containers are running as specified, monitors pod health, mounts volumes, pulls images.
- **kube-proxy:** Manages network rules, load balances traffic, maintains service-to-pod mapping.
- **Container Runtime:** Runs containers (containerd, CRI-O, Docker).

## Additional Cluster Services
- **CNI Plugin:** Manages pod networking (assigns IPs, creates interfaces, handles routes). Examples: Calico, Flannel, Cilium.
- **CSI Plugin:** Manages storage (persistent volumes, dynamic provisioning). Examples: EBS, NFS, Ceph.

## How It Works (Typical Flow)
1. User runs `kubectl apply -f app.yaml`.
2. API server writes desired state to etcd.
3. Scheduler assigns pod to a node.
4. Kubelet pulls image, creates container, sets up networking.
5. Kube-proxy updates service mappings.
6. Controller manager ensures desired state (replicas, jobs, etc.).

## Visual Summary
- Control Plane: kube-apiserver, etcd, scheduler, controller-manager, cloud-controller-manager
- Worker Nodes: kubelet, kube-proxy, container runtime, pods

---
Focus: Core components, architecture, and flow of Kubernetes.
