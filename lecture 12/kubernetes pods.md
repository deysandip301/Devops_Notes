# Kubernetes Pods

## 1. What is a Pod?
- Smallest deployable unit in Kubernetes: one or more co-scheduled containers sharing network, IPC, and volumes.
- Pod creation flows from kubectl → API server → etcd → scheduler → kubelet → container runtime.

## 2. Control Plane & Data Plane Roles
- **API Server:** Entry point, validates/authenticates/authorizes, runs admission controllers, stores objects in etcd.
- **etcd:** Persistent key-value store for cluster state.
- **Controller Manager:** Runs controllers (ReplicaSet, Deployment, Job, etc.).
- **Scheduler:** Assigns Pods to nodes.
- **Cloud Controller Manager:** Cloud-specific controllers.
- **kubelet:** Node agent, manages containers on node.
- **Container Runtime:** Pulls images, creates containers.
- **kube-proxy:** Programs networking for Services.
- **CNI/CSI:** Set up networking/storage for Pods.

## 3. Pod Object Basics
- Group of one or more containers, co-located and co-scheduled.
- Key fields: metadata (uid, resourceVersion), spec (containers, volumes, nodeName), status (phase, conditions).
- Stored in etcd under `/registry/pods/<namespace>/<podName>`.

## 4. Pod Creation Flow (Step-by-Step)
1. `kubectl apply -f pod.yaml` → API server (REST call)
2. API server: authentication/authorization
3. Admission controllers (mutate/validate)
4. API server writes Pod to etcd
5. API server responds to client
6. Scheduler assigns Pod to node (writes binding)
7. Kubelet on node sees assignment, enqueues Pod
8. Kubelet creates Pod: sets up volumes, networking (CNI), pulls images, starts containers
9. Kubelet reports PodStatus to API server
10. kube-proxy/CNI/CSI finalize networking/storage; Pod becomes Ready
11. Controllers (ReplicaSet/Deployment) reconcile state

## 5. etcd & Pod State
- etcd stores serialized Pod objects (spec, status).
- Updates: create, binding, status, watches (ADDED, MODIFIED, DELETED).
- Optimistic concurrency via resourceVersion.

## 6. Admission Webhooks
- Mutating webhooks can inject sidecars, defaults.
- Validating webhooks can reject Pods.
- Mutations affect scheduling and kubelet behavior.

## 7. Networking & Storage
- **CNI:** Allocates Pod IP, sets up veth/network namespace.
- **kube-proxy:** Sets up Service routing rules.
- **CSI:** Handles persistent volumes, attaches/mounts as needed.

## 8. Pod Lifecycle & States
- Phases: Pending, Running, Succeeded, Failed, Unknown.
- Conditions: Ready, PodScheduled, Initialized, Unschedulable.
- containerStatuses: waiting/running/terminated, restartCount, image IDs.
- Lifecycle hooks: init containers, preStop, graceful termination.

## 9. Debugging & Best Practices
- Use `kubectl get/describe/logs` for troubleshooting.
- Common issues: auth errors, webhook failures, scheduling, image pulls, CNI/CSI, CrashLoopBackOff.
- Use Deployments/ReplicaSets for management, define resource requests/limits, use probes, affinity, PDBs.

---
Focus: Pod definition, creation flow, control/data plane roles, networking/storage, lifecycle, and best practices.
