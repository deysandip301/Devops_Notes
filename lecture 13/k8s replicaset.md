# K8s ReplicaSet

## What is a ReplicaSet?
- Ensures a specified number of Pod replicas are always running.
- Continuously reconciles desired state (spec.replicas) vs current state (matching pods).
- Creates, deletes, or replaces Pods as needed.

## Control Plane Flow
1. `kubectl apply -f nginx-rs.yaml` → API server (REST call)
2. API server: AuthN, AuthZ, admission, stores ReplicaSet in etcd
3. ReplicaSet controller (in kube-controller-manager) sees new RS, compares pod count, creates Pods if needed
4. etcd stores new Pod objects (ownerReferences point to RS)
5. Scheduler assigns Pods to nodes (writes binding)
6. Kubelet on each node pulls image, creates containers, updates status

## Continuous Reconciliation
- ReplicaSet controller always checks: desired vs current pod count
- If equal: do nothing
- If less: create Pods
- If more: delete excess Pods

## Failure Scenarios
- Pod crash: RS creates replacement Pod
- Node dies: Node controller marks pods Unknown, RS deletes and recreates
- Manual pod delete: RS creates replacement (unless orphaned)
- RS delete: All owned pods are deleted

## etcd Object Layout
- `/registry/replicasets/default/nginx-rs` (spec, selector, podTemplate)
- `/registry/pods/default/web-abc12` (ownerRef = nginx-rs, spec.nodeName, status)

## Component Roles
| Component            | Role                                      |
|----------------------|-------------------------------------------|
| kubectl              | Sends RS YAML to API server                |
| API Server           | Validates, stores in etcd                  |
| etcd                 | Stores RS, Pods, and their status          |
| ReplicaSet Controller| Detects mismatch, creates/deletes Pods     |
| Scheduler            | Assigns Pods to nodes                      |
| Kubelet              | Manages containers, reports status         |
| Container Runtime    | Actually runs containers                   |

---
Focus: How ReplicaSet works, control flow, reconciliation, failure handling, and component roles.
