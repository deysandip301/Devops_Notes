# Shell Scripting – Concise Notes

## 1. Shebangs & Script Basics
- `#!/bin/bash` – Use Bash shell
- `#!/bin/sh` – Basic shell (POSIX)
- `#!/usr/bin/env bash` – Portable Bash

## 2. Hello World Script
```bash
#!/bin/bash
echo "Hello, World!"
```

## 3. Useful Bash Commands
- `echo` – Print text
- `date` – Show current date/time
- `df -h` – Disk usage
- `ls -l` – List files
- `read -p` – Prompt for input
- `if [ -f "$file" ]; then ... fi` – File exists check
- `tar -czf` – Create compressed backup
- `ping -c 1 8.8.8.8` – Check internet
- `${#str}` – String length
- `${str^^}` – Uppercase string (Bash)
- `${str,,}` – Lowercase string (Bash)
- `rev` – Reverse string
- `[[ "$str1" == "$str2" ]]` – String comparison (Bash)
- `${str:$pos:$len}` – Substring (Bash)

## 4. Functions in Bash
```bash
myfunc() {
  echo "Hello"
}
myfunc
```
- Arguments: `$1`, `$2`, `$@`, `$#`

## 5. Directory & File Operations
- `mkdir -p /tmp/newfolder` – Create directory
- `cp`, `mv`, `rm` – Copy, move, remove files
- `chmod`, `chown`, `chgrp` – Permissions/ownership

## 6. Sourcing Scripts
- `source ./utils.sh` – Import functions from another script

## 7. Script Arguments
- `$#` – Number of arguments
- `$1`, `$2` – First, second argument
- `if [ $# -eq 0 ]; then ... fi` – Check if arguments provided

## 8. ACLs (Access Control Lists)
- `getfacl file` – View ACLs
- `setfacl -m u:user:rw file` – Set ACL
- `setfacl -x u:user file` – Remove ACL
- `setfacl -b file` – Remove all ACLs
- `setfacl -R -m u:user:rwX dir/` – Recursive ACL
- `setfacl -d -m u:user:rwX dir/` – Default ACL for new files

## 9. Example: Create User Script (Standard)
```bash
#!/bin/bash
set -e
if [[ $EUID -ne 0 ]]; then echo "Run as root"; exit 1; fi
if [[ $# -ne 2 ]]; then echo "Usage: $0 <username> <groupname>"; exit 1; fi
USERNAME="$1"
GROUPNAME="$2"
if ! getent group "$GROUPNAME" > /dev/null; then groupadd "$GROUPNAME"; fi
if ! id "$USERNAME" &>/dev/null; then useradd -m -s /bin/bash -g "$GROUPNAME" "$USERNAME"; fi
```

## 10. Timestamped Directory Function
```bash
create_timestamped_dir() {
  local project_name="$1"
  local timestamp=$(date +%Y%m%d-%H%M%S)
  local dir_path="/tmp/${project_name}-${timestamp}"
  mkdir -p "$dir_path"
  echo "$dir_path"
}
```

---
Keep scripts executable: `chmod +x script.sh`
Run Bash scripts with `./script.sh` (not `sh script.sh` for Bash-specific features).
