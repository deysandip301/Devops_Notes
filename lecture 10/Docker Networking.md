# Docker Networking

## Key Concepts
- **IP Address:** Unique address for devices in a network.
- **NIC (Network Interface Card):** Hardware connecting a device to a network.
- **Ethernet Cable:** Physical medium for network connections.
- **Switch:** Layer-2 device forwarding frames by MAC address.
- **ARP:** Maps IP addresses to MAC addresses in a LAN.
- **Gateway:** Router for traffic outside the local subnet.
- **NAT (SNAT/DNAT):** Translates IPs/ports for routing and access.
- **Subnetting:** Divides networks for efficiency and security.

## Docker Networking Internals
- Uses Linux network namespaces, veth pairs, bridges, iptables, overlay networks.
- Each container gets its own network namespace (isolated stack).
- veth pairs connect containers to Docker networks.

## Docker Network Types
| Type     | Use Case                        |
|----------|---------------------------------|
| bridge   | Default, local container comm.  |
| host     | Shares host network stack       |
| none     | No networking (full isolation)  |
| overlay  | Multi-host, Swarm/K8s           |
| macvlan  | Unique MAC, LAN presence        |

## Bridge Network (Default)
- docker0 bridge connects containers on the same host.
- Internal IPs, NAT for internet access.
- Custom bridge networks support DNS-based discovery.

## Host Network
- Container uses host's network stack directly.
- No isolation, best performance.

## None Network
- No network access for the container.
- Used for secure, isolated jobs.

## Overlay Network
- Enables container comms across hosts (Swarm/K8s).
- Uses VXLAN encapsulation.

## Macvlan Network
- Containers get their own MAC and appear as real devices on LAN.

## Port Mapping
- `docker run -p 8080:80 nginx` maps host:8080 to container:80.
- Enables external access to container services.

## Inspecting Networks
- List: `docker network ls`
- Inspect: `docker network inspect <name>`
- Container details: `docker inspect <container>`

## Practical Example
1. Create network: `docker network create backend`
2. Run DB: `docker run -d --name db --network backend mysql`
3. Run app: `docker run -d --name app --network backend myapp`
- App connects to db:3306 by service name.

## Summary
- Docker networking uses Linux primitives for isolation and connectivity.
- Multiple network types for different use cases.
- Custom networks enable service discovery and better isolation.
- Port mapping and NAT allow external access.

---
Focus: Network types, internals, commands, and practical usage.
