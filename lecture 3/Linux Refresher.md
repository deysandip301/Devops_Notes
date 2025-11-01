# Linux Refresher

## AWS Fundamentals

### AWS Regions – The Global Layer
- **Region:** Geographically isolated area with multiple Availability Zones (AZs).
- **Purpose:** Data sovereignty, fault tolerance, low latency.
- **Example:** US East (N. Virginia) – `us-east-1`
- Each region has its own resources (EC2, RDS, S3, etc.).

### Availability Zones (AZs)
- Each region has 2–6+ AZs (separate datacenters).
- Example: `ap-south-1a`, `ap-south-1b`, `ap-south-1c` in `ap-south-1` region.

### VPC (Virtual Private Cloud)
- Isolated virtual network for AWS resources within a region.
- Define IP ranges, subnets, route tables, gateways, security groups, NACLs.

### Subnets in AWS
- Subnet: Subdivision of a VPC, belongs to one AZ.
- Used to group resources (public/private), control routing and access.

#### Subnet Best Practices
1. Use multiple AZs for redundancy.
2. Divide subnets into tiers (public/private/isolated).
3. Use NAT Gateways for private subnet internet access.
4. Reserve enough CIDR space for scaling.
5. Tag subnets clearly.
6. Use route tables and security groups for isolation.

### EC2 Instance Creation Fields (Key Points)
- **Name & Tags:** Identify and organize instances.
- **AMI:** OS and pre-installed software image.
- **Instance Type:** Hardware config (CPU, RAM, etc.).
- **Key Pair:** SSH/RDP access.
- **Network Settings:** VPC, subnet, public IP, security groups.
- **Storage:** Root/additional volumes, type, size, encryption.
- **Advanced:** IAM role, shutdown behavior, monitoring, placement, user data, etc.

---

## Linux Fundamentals

### What is Linux?
- Open-source OS for servers, cloud, DevOps, automation.

### Linux Architecture
- **Kernel:** Manages hardware, processes, security.
- **Shell:** Command-line interface to interact with kernel (bash, zsh, etc.).
- **Why Learn Shell?** Automation, scripting, debugging, navigation.

---

### 1. Basic Linux Commands
- `pwd`: Show current directory.
- `cd`: Change directory.
- `mkdir`: Create directory.
- `rmdir`: Remove empty directory.
- `touch`: Create file/update timestamp.
- `cat`: View file contents.
- `echo`: Print/write text.
- `clear`: Clear terminal.
- `history`: Show command history.

#### Lab 1: Basic Commands
- Create `starting_point` directory.
- Navigate inside.
- Create `secret_lair.txt`.
- Display absolute path.
- View command history.

---

### 2. File Management Commands
- `ls`: List directory contents.
- `cp`: Copy files/directories.
- `mv`: Move/rename files.
- `rm`: Remove files/directories.
- `find`: Search for files.
- `du`: Disk usage.
- `df`: Disk free space.

#### Lab 2: File Management
- Remove `delete_directory` if exists.
- Delete `delete.txt`.
- Find all `.txt` files under `/home/user`.
- Show disk usage of `/var/log/`.

---

### 3. Process Management Commands
- `ps`: List running processes.
- `top`: Real-time process monitor.
- `htop`: Enhanced top (if installed).
- `kill`: Terminate process by PID.
- `jobs`, `bg`, `fg`: Manage background/foreground jobs.

#### Lab 3: Process Management
- List all processes.
- Find PID of `sshd`.
- Start background job (`sleep 60`).
- Bring to foreground (`fg`).
- Kill process by PID.

---

### Extended: Permissions & Ownership
- `chmod`: Change permissions.
- `chown`: Change owner.
- `chgrp`: Change group.

#### Lab 4: Advanced Challenge
- Work with hidden directory `secret_lair`.
- Count subdirectories.
- Identify permission-restricted one.
- Change ownership to user.
- Write answers to `/home/user/answer.txt`.
