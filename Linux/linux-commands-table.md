# Linux Commands — Complete Reference Table (Day 03–13)

A quick-reference table of every Linux command practiced, organized by category.

---

## 1. System Information

| Command | Explanation |
|---|---|
| `uname -a` | Shows kernel version, machine architecture, and system identity details |
| `cat /etc/os-release` | Shows which Linux distribution and version is installed |
| `whoami` | Prints the username currently logged in as |
| `hostname` | Prints the machine's network name |
| `pwd` | Prints the full path of the current working directory |
| `uptime` | Shows how long the system has been running, plus load average |

---

## 2. Files & Directories

| Command | Explanation |
|---|---|
| `ls` | Lists files/folders in the current location |
| `ls -l` | Lists with details — permissions, size, owner, date |
| `ls -la` | Lists all, including hidden files (starting with `.`) |
| `ls -ld <folder>` | Shows details about the folder itself, not its contents |
| `ls -lR <folder>` | Lists recursively — folder and everything inside all subfolders |
| `mkdir <name>` | Creates a new, empty folder |
| `mkdir -p <path/sub>` | Creates nested folders in one go, even if parents don't exist |
| `touch <filename>` | Creates a new, empty file (or updates timestamp if it exists) |
| `cp <source> <dest>` | Copies a file |
| `cp -r <source> <dest>` | Copies an entire folder and its contents |
| `mv <source> <dest>` | Moves a file, or renames it |
| `rm <filename>` | Deletes a file permanently |
| `rm -r <folder>` | Deletes a folder and everything inside it |
| `cd <folder>` | Changes into that folder |
| `cd ..` | Moves up one level to the parent folder |
| `cd ~` | Jumps to the home directory |

---

## 3. Reading & Writing Files

| Command | Explanation |
|---|---|
| `echo "text"` | Prints text to the screen |
| `echo "text" > file` | Writes text into a file, overwriting existing content |
| `echo "text" >> file` | Appends text to the end of a file, keeping existing content |
| `cat <filename>` | Prints the entire content of a file to the screen |
| `head -n <N> <filename>` | Shows the first N lines of a file |
| `tail -n <N> <filename>` | Shows the last N lines of a file (great for recent log entries) |
| `tail -f <filename>` | Follows a file live, showing new lines as they're added |
| `tee -a <filename>` | Prints output to screen AND appends it to a file simultaneously |
| `nano <filename>` | Opens a simple text editor to create/edit a file |

---

## 4. File Permissions

| Command | Explanation |
|---|---|
| `ls -l <file>` | Shows current permissions in `-rwxrwxrwx` format |
| `chmod +x <file>` | Adds execute permission (symbolic) |
| `chmod -x <file>` | Removes execute permission |
| `chmod +w <file>` | Adds write permission |
| `chmod 755 <file>` | Numeric: owner=rwx, group=r-x, others=r-x |
| `chmod 644 <file>` | Numeric: owner=rw-, group=r--, others=r-- |
| `chmod 444 <file>` | Numeric: read-only for everyone, including owner |
| `chmod 640 <file>` | Numeric: owner=rw-, group=r--, others=nothing |
| `chmod -R <perm> <folder>` | Applies permission change recursively to a folder and its contents |

---

## 5. File Ownership

| Command | Explanation |
|---|---|
| `chown <user> <file>` | Changes the owner of a file |
| `chgrp <group> <file>` | Changes the group assigned to a file |
| `chown <user>:<group> <file>` | Changes both owner and group in one command |
| `chown -R <user>:<group> <folder>` | Changes ownership recursively (folder + everything inside) |

---

## 6. User & Group Management

| Command | Explanation |
|---|---|
| `useradd -m <username>` | Creates a new user, `-m` also creates their home directory |
| `passwd <username>` | Sets or changes a user's password |
| `groupadd <groupname>` | Creates a new group |
| `usermod -aG <group> <username>` | Adds an existing user to a group (`-a` = append, don't remove other groups) |
| `groups <username>` | Shows which groups a user belongs to |
| `grep <pattern> /etc/passwd` | Searches the user list file to confirm a user exists |
| `grep <pattern> /etc/group` | Searches the group list file to confirm a group exists |
| `sudo -u <username> <command>` | Runs a command as if you were a different user (test their permissions) |

---

## 7. Process Management

| Command | Explanation |
|---|---|
| `ps aux` | Lists every running process, with owner, CPU%, memory%, command |
| `ps aux --sort=-%cpu` | Sorts processes by CPU usage, highest first |
| `top` | Live, real-time view of CPU/memory usage and running processes |
| `htop` | A nicer, interactive version of `top` |
| `kill <PID>` | Asks a process to stop gracefully |
| `kill -9 <PID>` | Force-kills a process immediately, no cleanup time |
| `pgrep <name>` | Finds a process's ID by searching for its name |

---

## 8. Services & systemd

| Command | Explanation |
|---|---|
| `systemctl status <service>` | Checks if a service is running, and shows recent status |
| `systemctl start <service>` | Starts a service |
| `systemctl stop <service>` | Stops a service |
| `systemctl restart <service>` | Stops and starts a service in one command |
| `systemctl enable <service>` | Makes a service start automatically on every boot |
| `systemctl is-enabled <service>` | Confirms whether auto-start on boot is enabled |
| `journalctl -u <service> -n 50` | Shows the last 50 log lines for a specific service |
| `journalctl -u <service> -f` | Follows a service's logs live, in real-time |
| `journalctl -xe` | Shows recent, detailed system-wide logs |

---

## 9. Networking

| Command | Explanation |
|---|---|
| `ip addr` | Shows all network interfaces and their IP addresses |
| `ping <host>` | Tests connectivity to a target |
| `ping -c <N> <host>` | Sends only N test pings, then stops automatically |
| `curl <url>` | Fetches a URL's response — tests if a site/API is reachable |
| `curl -I <url>` | Fetches ONLY the response headers, not the full page |
| `dig <domain>` | Performs a DNS lookup for a domain name |
| `ss -tulnp` | Shows open/listening network ports and which process uses each |
| `netstat -tulnp` | Older tool, similar to `ss` — shows active connections/ports |

---

## 10. Disk & Storage

| Command | Explanation |
|---|---|
| `df -h` | Shows disk space used/available on each mounted partition, human-readable |
| `du -sh <path>` | Shows total disk space used by a folder, human-readable, summarized |
| `du -sh /var/log/* \| sort -h \| tail -5` | Finds the 5 largest items inside `/var/log` |
| `lsblk` | Lists all block devices (disks) attached to the machine |
| `free -h` | Shows memory (RAM) usage, human-readable |

---

## 11. LVM (Logical Volume Manager)

| Command | Explanation |
|---|---|
| `sudo -i` | Switches fully into the root user for the session |
| `pvs` | Lists existing Physical Volumes |
| `vgs` | Lists existing Volume Groups |
| `lvs` | Lists existing Logical Volumes |
| `pvcreate <disk>` | Initializes a raw disk as a Physical Volume |
| `vgcreate <vg-name> <disk1> <disk2>` | Combines Physical Volumes into a Volume Group |
| `lvcreate -L <size> -n <lv-name> <vg-name>` | Creates a Logical Volume of a given size from a Volume Group |
| `mkfs.ext4 <lv-path>` | Formats a Logical Volume with the EXT4 filesystem |
| `mount <lv-path> <mount-point>` | Attaches a formatted volume to a folder, ready to use |
| `lvextend -L +<amount> <lv-path>` | Grows an existing Logical Volume by adding more space |
| `resize2fs <lv-path>` | Expands the filesystem to use the volume's newly added space |

---

## Quick Workflow Reference (typical troubleshooting flow)

```
uname -a                     → check system info
cat /etc/os-release          → check OS version
top / free -h / df -h        → check CPU, memory, disk
ss -tulnp                    → check open ports
systemctl status <service>   → check if the service is running
journalctl -u <service> -n 50 → check its recent logs
tail -n 50 /var/log/syslog   → check general system logs
```

## Quick Permission/Ownership Fix Flow

```
ls -l <file>                       → check current owner/group/permissions
sudo chown <user>:<group> <file>   → fix ownership if wrong
chmod <perm> <file>                → fix permissions if needed
```

## Quick LVM Setup Flow

```
lsblk                                          → see available disks
sudo pvcreate <disk1> <disk2>                   → make Physical Volumes
sudo vgcreate <vg-name> <disk1> <disk2>          → combine into a Volume Group
sudo lvcreate -L <size> -n <lv-name> <vg-name>    → carve out a Logical Volume
sudo mkfs.ext4 <lv-path>                          → format it
sudo mount <lv-path> <mount-point>                → mount it, ready to use
```
