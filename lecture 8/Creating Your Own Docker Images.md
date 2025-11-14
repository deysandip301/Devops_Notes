# Creating Your Own Docker Images

## 1. What is a Container?
- A container = Linux process + temporary root filesystem (image layers + writable layer).
- Ephemeral: When the main process exits, the container and its writable layer are gone.
- Stateless by design.

## 2. Containers Are Just Processes
- Docker does NOT start a VM; it launches a Linux process with isolation (namespaces, cgroups).
- All containers share the host kernel.
- Root in container ≠ root on host (user namespace).

## 3. Container Internals
- Docker → containerd → shim → runc → your process
- containerd: Manages containers
- shim: Parent for container process
- runc: Sets up namespaces/cgroups, starts main process
- Main process is PID 1 in container namespace

## 4. Isolation Mechanisms
- Namespaces: PID, NET, UTS, MOUNT, USER, IPC
  - PID: Own process tree (PID 1 inside container)
  - NET: Own network stack (IP, routes)
  - MOUNT: Own root filesystem (OverlayFS)
  - UTS: Own hostname
  - IPC: Own shared memory
  - USER: Root in container mapped to non-root on host
- Cgroups: Limit CPU, memory, disk I/O, PIDs

## 5. Lifecycle & Ephemerality
- Container = one main process (PID 1)
- If PID 1 exits, container stops, writable layer is deleted
- Data is lost unless stored in volumes, bind mounts, or external storage

## 6. Why Containers Start Fast & Are Lightweight
- No kernel boot, no virtual hardware, no init system
- Just a process with isolated resources
- Share host kernel and OS

## 7. VM vs Container
- VM: Hardware → Hypervisor → Guest OS → App
- Container: Hardware → Linux Kernel → App (isolated by namespaces/cgroups)
- Containers do NOT run a separate kernel

## 8. Summary
- Containers = processes with isolation (not mini-VMs)
- Isolation: Namespaces (illusion of separate system)
- Resource limits: Cgroups
- Ephemeral: Data lost unless persisted externally
- Fast, lightweight, and efficient

---
Focus: What containers really are, how isolation works, lifecycle, and differences from VMs.
