# Shell Scripting Practice Log

## utils.sh
```bash
#!/bin/bash

# Function to create a directory with a timestamped name
create_timestamped_dir() {
    local base_name="$1"
    local timestamp
    timestamp=$(date +"%Y%m%d_%H%M%S")
    local dir_name="${base_name}_${timestamp}"
    local absolute_path="/tmp/${dir_name}"
    mkdir -p "$absolute_path"
    echo "$absolute_path"
}
```

## setup_project.sh
```bash
#!/bin/bash

# Source the utils.sh file to use its functions
source ./utils.sh

# Create a timestamped directory for the new project
project_dir=$(create_timestamped_dir "my-new-app")
echo "$project_dir"
```

## Terminal Session
```
user@2603264a2cd3:~$ vim setup_project.sh
user@2603264a2cd3:~$ ls
setup_project.sh
user@2603264a2cd3:~$ ls -al
total 28
drwxr-x--- 1 user user 4096 Feb  3 05:56 .
drwxr-xr-x 1 root root 4096 Jun  6  2025 ..
-rw-r--r-- 1 user user  220 Mar  5  2025 .bash_logout
-rw-r--r-- 1 user user 3771 Mar  5  2025 .bashrc
-rw-r--r-- 1 user user  807 Mar  5  2025 .profile
-rw------- 1 user user  752 Feb  3 05:56 .viminfo
-rw-rw-r-- 1 user user    0 Feb  3 05:56 setup_project.sh
user@2603264a2cd3:~$ 
```
