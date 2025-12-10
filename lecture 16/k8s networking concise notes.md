# Kubernetes Networking Model – Concise Notes

## 1. Kubernetes Basics
- **API Server**: Central control plane, REST API, backed by etcd.
- **Controllers**: Reconcile desired vs. actual state (e.g., scheduler, kubelet).
- **Pods**: Smallest unit, one/more containers, share network namespace & volumes.
- **Nodes**: Worker machines running kubelet, kube-proxy, container runtime.

## 2. Networking Model Principles
- All Pods can talk to all Pods (no NAT).
- All Nodes can talk to all Pods (no NAT).
- Pod IP is the same inside/outside the Pod.

## 3. Container-to-Container (in a Pod)
- Containers share network namespace (IP, ports, localhost).
- Communicate via localhost.

## 4. Pod-to-Pod Networking
- Each Pod: own network namespace, connected via veth pairs to Linux bridge.
- **Same node**: veth → bridge → veth.
- **Different nodes**: Routed via node's default interface, Pod CIDR per node.
- **CNI plugins**: AWS VPC CNI (real VPC IPs), Calico, Flannel, Weave (overlays/BGP).

## 5. Pod-to-Service (ClusterIP)
- Service gives stable virtual IP (ClusterIP), load balances to Pods.
- **kube-proxy**: Uses iptables (classic) or IPVS (modern) for load balancing.
- **DNS**: CoreDNS provides service discovery.

## 6. Internet-to-Service
- **Egress (Pod → Internet)**: SNAT Pod IP to Node IP, then NAT to public IP.
- **Ingress (Internet → Pods)**:
  - Service Type=LoadBalancer (L4): Cloud LB → NodePort → Pods.
  - Ingress Controller (L7): Host/path routing, SSL, points to Services.

## 7. Summary Table
| Layer                  | What Happens                                 |
|------------------------|----------------------------------------------|
| Container-to-Container | Share net ns, localhost                      |
| Pod-to-Pod (same node) | veth pair → bridge → veth                    |
| Pod-to-Pod (cross)     | Routed via Pod CIDR                          |
| Pod-to-Service         | iptables/IPVS DNAT                           |
| DNS                    | CoreDNS resolves service names               |
| Egress                 | SNAT Pod → Node → Internet                   |
| Ingress                | LoadBalancer/Ingress Controller              |
