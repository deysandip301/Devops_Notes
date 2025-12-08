# K8s Services

## Why Services?
- Pods are ephemeral and get new IPs on restart.
- Services provide a stable virtual IP, DNS name, and load balancing for accessing pods.

## What is a Kubernetes Service?
- Abstraction for stable access to pods.
- Provides ClusterIP, DNS, load balancing, and traffic routing via kube-proxy.
- Allows access from inside or outside the cluster.

## How a Service Works
1. Service created with a pod selector (e.g., `app: myapp`).
2. kube-proxy sets up load-balancing rules (iptables/IPVS).
3. Service gets a stable ClusterIP.
4. Traffic to ClusterIP is forwarded to matching pods.

## Types of Kubernetes Services
| Type         | Purpose                        | Accessible From         |
|--------------|-------------------------------|------------------------|
| ClusterIP    | Internal communication        | Inside the cluster      |
| NodePort     | Expose on node IP/port        | External (nodeIP:port)  |
| LoadBalancer | Cloud LB, public access       | Internet or VPC         |

## ClusterIP Service (Default)
- Internal-only, for service-to-service comms.
- Example YAML:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-svc
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
    - port: 80
      targetPort: 8080
```
- Access: `curl http://backend-svc` (inside cluster)

## NodePort Service
- Exposes service on every node's IP at a static port (30000–32767).
- Example YAML:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-nodeport
spec:
  type: NodePort
  selector:
    app: backend
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 32080
```
- Access: `http://<NodeIP>:32080`

## LoadBalancer Service
- Exposes service via cloud load balancer (AWS, GCP, Azure).
- Example YAML:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-lb
spec:
  type: LoadBalancer
  selector:
    app: backend
  ports:
    - port: 80
      targetPort: 8080
```
- Access: `http://<EXTERNAL-IP>` (from cloud provider)

## Additional Concepts
- **Headless Service:** `clusterIP: None` for DNS-based discovery (StatefulSets, DBs).
- **Multi-Port Service:** Expose multiple ports in one service.
- **Service DNS:** `<servicename>.<namespace>.svc.cluster.local`
- **ExternalName Service:** Maps to external DNS name.

## Which Service Type to Use?
| Need                                 | Service Type   |
|--------------------------------------|---------------|
| Internal-only microservices          | ClusterIP     |
| Expose to local network (no cloud LB)| NodePort      |
| Production external access           | LoadBalancer  |
| StatefulSets / DBs                   | Headless      |
| Access external DB                   | ExternalName  |

---
Focus: Service types, YAML, access patterns, and when to use each.
