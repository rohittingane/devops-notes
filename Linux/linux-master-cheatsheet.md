# 🐧 Master Linux Cheat Sheet (Day 03 – Day 13)

**A complete, beginner-friendly reference.** Every command explained word-by-word — what it is, why we use it, and how to use it — so that even someone who has never touched Linux before can follow along.

---

# 📖 Part 0: The Absolute Basics — How Linux Actually Works

### The 3-layer structure of Linux

Linux, at its core, is organized into three big layers:

```
┌─────────────────────────────┐
│   User Space (you & apps)   │  ← where YOU work: commands like ls, top, ps
├─────────────────────────────┤
│   systemd (PID 1)           │  ← the very first program that starts everything else
├─────────────────────────────┤
│   Kernel                    │  ← the core engine — talks directly to hardware
└─────────────────────────────┘
```

### 1. Kernel — the "engine room"
The kernel is the innermost core of Linux. It directly manages the CPU, memory (RAM), storage disks, and hardware devices. Every other program on the system talks to hardware only THROUGH the kernel — nothing bypasses it.

**Simple analogy:** The kernel is like a building's electrical and plumbing system — invisible to residents, but everything (lights, water, AC) depends on it working correctly.

### 2. User Space — "where you and your apps live"
This is where everything you actually see and type happens — commands like `ls`, `pwd`, `ps`, `top` all run here. User space programs talk to the kernel using something called **system calls** — a formal, controlled way of asking the kernel to do something (like "please read this file" or "please give me more memory").

### 3. systemd — "the very first thing that wakes up"
`systemd` is the very first program the kernel starts once it finishes booting. It almost always gets **Process ID (PID) 1** — the lowest possible ID, since it's the first process ever created. From there, `systemd` is responsible for starting every other service (SSH, Nginx, Docker, etc.), stopping them, restarting them if they crash, and tracking their status.

### What is a "process," really?
A process is simply **one running instance of a program**. If you run the same program twice (e.g., open two terminal windows), that's two separate processes, each with its own unique ID (PID).

### Process states — what "state" is a process in right now?

| State (shorthand) | Meaning |
|---|---|
| **R** (Running) | Actively using the CPU right now |
| **S** (Sleeping) | Waiting for something (user input, a file, a network response) — not using CPU |
| **T** (Stopped) | Paused/frozen deliberately |
| **Z** (Zombie) | Finished running, but its exit info hasn't been "collected" yet by its parent process — a leftover entry in the process table |

---

# 📂 Part 1: The Linux File System — Where Everything Lives

Unlike Windows (which has `C:\`, `D:\` drives), Linux has ONE single tree, starting from `/` (called "root," not to be confused with the root USER).

```
/                 ← everything starts here
├── home/         ← normal users' personal folders
├── root/         ← the root user's own personal folder
├── etc/          ← configuration files for almost everything
├── var/log/      ← log files (very important for troubleshooting)
├── tmp/          ← temporary files, may get wiped on reboot
├── bin/          ← essential system commands (ls, cp, etc.)
├── usr/bin/      ← commands for regular installed applications
└── opt/          ← third-party/optional software
```

### `/` (Root)
The absolute starting point of the entire filesystem — every single file and folder on the system exists somewhere underneath this one root.
```bash
ls -l /
```

### `/home`
Contains a personal folder for every regular (non-root) user — this is where a user's own files, downloads, and personal settings live.
```bash
ls -l /home
```

### `/root`
The home directory belonging specifically to the **root** (administrator) user — separate from `/home`, since root is a special, privileged account.
```bash
ls -l /root
```

### `/etc`
Short for "et cetera," but in practice it means: **configuration files live here.** Nearly every service (SSH, Nginx, Docker) stores its settings somewhere inside `/etc`.
```bash
ls -l /etc
cat /etc/hostname     # shows this machine's network name
```
**Why it matters for troubleshooting:** If a service is misbehaving, checking its config file in `/etc` is often step one.

### `/var/log`
Stores log files — records of what happened, when, and (often) why something went wrong. This is one of the MOST important folders for any DevOps engineer, since debugging almost always starts here.
```bash
ls -l /var/log
```

### `/tmp`
Temporary storage — files here might automatically get deleted after a reboot. Good for short-lived, disposable data; never store anything important here long-term.
```bash
ls -l /tmp
```

### `/bin`
Contains essential, core command-line tools that the system needs even in its most basic state (like `ls`, `cp`, `mv`).
```bash
ls -l /bin
```

### `/usr/bin`
Contains command-line tools for regular installed applications (as opposed to the bare-minimum tools in `/bin`).
```bash
ls -l /usr/bin
```

### `/opt`
Where third-party software (not part of the core OS) often gets installed — think of it as "optional add-ons" territory.
```bash
ls -l /opt
```

---

# 🖥️ Part 2: Checking Basic System Information

### `uname -a`
Shows kernel version, machine architecture, and other core system identity details — often the very first command run when logging into an unfamiliar server, just to know what you're working with.

### `cat /etc/os-release`
Shows exactly which Linux distribution and version is installed (e.g., Ubuntu 26.04). `cat` here just means "print this file's contents to the screen."

### `whoami`
Prints the username you're currently logged in as.

### `hostname`
Prints this machine's network name/identity.

### `pwd`
- Stands for **p**rint **w**orking **d**irectory
- Shows the full path of the folder you're currently standing in

---

# 📁 Part 3: File & Directory Basics

### `ls`
- Lists files and folders in the current location
- `ls -l` → adds detail (permissions, size, owner, date) — the "long" format
- `ls -la` → same, but also shows **hidden** files (ones starting with a dot, like `.git`)
- `ls -ld <folder>` → shows details about the FOLDER ITSELF, not what's inside it
- `ls -lR <folder>` → lists recursively — the folder AND everything inside every subfolder

### `mkdir <name>`
- **m**a**k**e **dir**ectory — creates a new, empty folder
- `mkdir -p <path/with/subfolders>` → creates nested folders in one go, even if the parent folders don't exist yet
```bash
mkdir -p heist-project/vault    # creates both "heist-project" AND "vault" inside it
```

### `touch <filename>`
Creates a brand-new, completely empty file instantly. If the file already exists, `touch` just updates its "last modified" timestamp instead.

### `cp <source> <destination>`
Copies a file (or with `-r`, an entire folder) from one location to another.
```bash
cp /etc/hosts /tmp/hosts-copy
```

### `mv <source> <destination>`
Moves a file, OR renames it (renaming is just "moving" a file to a new name in the same folder).

### `rm <filename>`
Deletes a file permanently (no recycle bin — be careful!). `rm -r <folder>` deletes an entire folder and its contents.

### `cd <folder>`
- **c**hange **d**irectory — moves you into that folder
- `cd ~` or just `cd` → jumps straight to your home directory
- `cd ..` → moves UP one level (to the parent folder)

---

# ✍️ Part 4: Reading & Writing Files (File I/O)

### `echo "text"`
Simply prints the given text to the screen. On its own, it's just a print statement — but combined with `>` or `>>`, it becomes a way to write into files.

### `>` (overwrite redirect)
Takes whatever output comes before it, and writes it into a file — **completely replacing** any existing content in that file.
```bash
echo "Learning Linux file operations." > notes.txt
```
⚠️ Careful: if `notes.txt` already had content, it's now GONE, replaced entirely.

### `>>` (append redirect)
Same idea, but **adds to the end** of the file instead of erasing what's already there.
```bash
echo "Practicing file read and write commands." >> notes.txt
```

### `cat <filename>`
Short for con**cat**enate. Prints the ENTIRE content of a file to the screen at once. Best for short files.

### `head -n <number> <filename>`
Shows only the FIRST `<number>` lines of a file. Useful for quickly peeking at a huge file without printing all of it.
```bash
head -n 5 /etc/passwd     # shows just the first 5 lines
```

### `tail -n <number> <filename>`
Shows only the LAST `<number>` lines of a file — extremely useful for checking the most RECENT entries in a log file (since logs are usually appended to at the bottom).
```bash
tail -n 20 /var/log/nginx/access.log
```

### `tee -a <filename>`
Does TWO things at once: prints the output to your screen AND appends it into a file simultaneously. `-a` means append (without it, `tee` would overwrite the file instead).
```bash
echo "Some message" | tee -a notes.txt
```
The `|` (pipe) symbol here means "take the output of the command on the left, and feed it as input into the command on the right."

---

# 🔐 Part 5: File Permissions — Who Can Do What

### The permission format, decoded: `-rwxrwxrwx`

Every file/folder has 10 characters describing its permissions:
```
-   rwx   rwx   rwx
│    │     │     │
│  Owner Group Others
│
└─ file type (- = regular file, d = directory)
```

Each group of 3 letters means:
- **r** = read (can view the content)
- **w** = write (can modify/delete the content)
- **x** = execute (can RUN it, if it's a script/program) OR enter it (if it's a folder)

**So `-rw-rw-r--` means:**
- Owner: read + write (no execute)
- Group: read + write (no execute)
- Others: read only

### Numeric (octal) permission values

| Permission | Value |
|---|---|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

You ADD these numbers together for each of the 3 categories (owner, group, others):
- `7` = 4+2+1 = read + write + execute (everything)
- `6` = 4+2 = read + write (no execute)
- `5` = 4+1 = read + execute (no write)
- `4` = read only

**So `chmod 755` means:** Owner gets 7 (rwx), Group gets 5 (r-x), Others get 5 (r-x) — a very common setting for folders/scripts you want others to be able to use but not modify.

### `chmod <permissions> <file>`
Changes a file/folder's permissions.
```bash
chmod +x script.sh        # symbolic: ADD execute permission for everyone
chmod 755 project/        # numeric: owner=rwx, group=r-x, others=r-x
chmod 444 devops.txt      # numeric: read-only for everyone (owner included!)
chmod 640 notes.txt       # owner=rw-, group=r--, others=(nothing)
```

**Symbolic vs Numeric — when to use which:**
- Symbolic (`+x`, `-w`, `u+x`) → good for making a SMALL, specific change without affecting other permissions
- Numeric (`755`, `644`) → good for setting the ENTIRE permission set at once, clearly and precisely

### Common "Permission denied" gotchas (real mistakes made & fixed)

**Mistake: `./script.sh` fails with "Permission denied"**
Cause: the file doesn't have execute (`x`) permission yet.
Fix: `chmod +x script.sh`

**Mistake: `chmod +X script.sh` (capital X) doesn't behave as expected**
Capital `X` is a special conditional flag — it only adds execute permission if the file is ALREADY executable for someone, or if it's a directory. For a brand-new script, lowercase `x` is what you actually want.

**Mistake: `./ script.sh` (with a space) gives "Is a directory"**
The space breaks it — Linux reads `./` (current directory) and `script.sh` as two separate things instead of one path. Correct: `./script.sh` (no space).

**Mistake: writing to a read-only file gives "Permission denied"**
```bash
echo "test" >> devops.txt     # devops.txt is -r--r--r--
```
Fix: add write permission first: `chmod +w devops.txt`

---

# 👤 Part 6: File Ownership — Owner & Group

Every single file has exactly ONE owner (a user) and ONE group assigned to it. This is separate from permissions (permissions decide WHAT can be done; ownership decides WHO the "owner" and "group" categories in those permissions actually refer to).

### `chown <user> <file>`
Changes who OWNS a file.
```bash
sudo chown tokyo devops-file.txt
```

### `chgrp <group> <file>`
Changes which GROUP is assigned to a file.
```bash
sudo chgrp heist-team team-notes.txt
```

### `chown <user>:<group> <file>`
Changes BOTH the owner and the group in a single command.
```bash
sudo chown professor:heist-team project-config.yaml
```

### `chown -R <user>:<group> <folder>`
`-R` = **r**ecursive — applies the ownership change to the folder AND everything inside it (all files and subfolders), not just the top-level folder itself.
```bash
sudo chown -R professor:planners heist-project
```

### Why does ownership matter so much in real troubleshooting?

**Real scenario faced:** A log file was copied using `sudo cp ...`, which made **root** the owner of the copied file. Later, trying to read it as a normal user (`ubuntu`) gave `Permission denied` — even though the file technically had read permission for "others," something else was blocking it (owner-level restrictions or group mismatch). Fixed with:
```bash
sudo chown ubuntu:ubuntu ~/nginx-logs.txt
```
This is an extremely common real-world gotcha: files created via `sudo` often end up owned by `root`, causing confusing permission errors for the regular user later.

---

# 👥 Part 7: User & Group Management

### Why groups exist at all
Instead of setting permissions individually for every single user (which doesn't scale), Linux lets you create a GROUP, add multiple users to it, and then set ONE set of permissions for that entire group at once.

### `useradd -m <username>`
Creates a new user. The `-m` flag automatically also creates a home directory for them (e.g., `/home/tokyo`) — without `-m`, they'd have no personal folder.
```bash
sudo useradd -m tokyo
```

### `passwd <username>`
Sets (or changes) a user's password.
```bash
sudo passwd tokyo
```

### `groupadd <groupname>`
Creates a new group.
```bash
sudo groupadd developers
```

### `usermod -aG <group> <username>`
Adds an EXISTING user into a group.
- `-a` = **a**ppend (add to existing groups, don't remove them from others)
- `-G` = specify a **G**roup (or comma-separated list of groups) to add them to

⚠️ **Important gotcha:** `usermod` only modifies **ONE user at a time.** Writing `usermod -aG group user1,user2` does NOT work as expected — Linux reads `user1,user2` as one single (invalid) username, not two separate users. You must run it once per user:
```bash
sudo usermod -aG developers tokyo
sudo usermod -aG developers,admins berlin    # multiple GROUPS is fine, multiple USERS is not
```

### `groups <username>`
Shows which groups a specific user currently belongs to.

### `grep <pattern> /etc/passwd` / `grep <pattern> /etc/group`
`grep` searches for a specific text pattern inside a file. Used here to quickly confirm a user or group was actually created, by searching the system's master user/group list files.
```bash
grep tokyo /etc/passwd
```

### `sudo -u <username> <command>`
Runs a command AS IF you were a different user — extremely useful for testing whether a specific user actually has the permissions you think they have, without needing to fully log in as them.
```bash
sudo -u tokyo touch /opt/dev-project/tokyo.txt
```

### Real-world shared folder setup (putting it all together)
```bash
sudo mkdir /opt/dev-project
sudo chgrp developers /opt/dev-project    # assign the group
sudo chmod 775 /opt/dev-project            # owner+group get full access, others can only read/enter
sudo -u tokyo touch /opt/dev-project/tokyo.txt   # test as tokyo
```

---

# ⚙️ Part 8: Process Management & systemd

### `ps aux`
Lists EVERY currently running process on the system, with details (owner, CPU%, memory%, command). `aux` here is a combination of classic flags meaning "show all processes, for all users, in a detailed format."

### `ps aux --sort=-%cpu | head -10`
Shows the top 10 processes using the MOST cpu right now. `--sort=-%cpu` sorts by CPU usage descending; piping into `head -10` trims it to just the top 10.

### `top`
Opens a live, constantly-refreshing view of CPU usage, memory usage, and running processes — like a real-time dashboard. Press `q` to quit.

### `htop`
A more visual, colorful, interactive version of `top` (not always pre-installed, but nicer to use when available).

### `kill <PID>`
Asks a specific process (by its Process ID) to stop gracefully.
`kill -9 <PID>` → forces an immediate, non-negotiable termination (use only if a normal `kill` doesn't work, since it gives the process no chance to clean up first).

### `pgrep <process-name>`
Finds the Process ID of a running program by searching for its name, instead of scrolling through the entire `ps aux` list manually.

### `uptime`
Shows how long the system has been running since its last reboot, plus the "load average" (a rough measure of how busy the system has been recently).

### `free -h`
Shows memory (RAM) usage — how much is used, free, and available. `-h` means **h**uman-readable (shows "2.1G" instead of a raw byte count).

### `systemctl status <service>`
Shows whether a specific background service (like `ssh` or `nginx`) is currently running, and basic recent log lines.
```bash
systemctl status ssh
```

### `systemctl start / stop / restart <service>`
Starts, stops, or restarts a service.
```bash
sudo systemctl restart ssh
```

### `journalctl -u <service> -n 50`
Shows the last 50 log lines specifically for that service (services managed by `systemd` store their logs in a system called `journald`, and `journalctl` is the tool to read them).

### `journalctl -u <service> -f`
`-f` = **f**ollow — keeps the log view open and updates LIVE as new log entries come in, instead of showing a fixed snapshot. Great for watching what happens in real time while you trigger an action elsewhere.

### `journalctl -xe`
Shows recent, detailed system logs — often the first thing checked when something has gone wrong system-wide.

---

# 🌐 Part 9: Networking Troubleshooting

### `ip addr`
Shows all network interfaces on this machine and their assigned IP addresses.

### `ping <host>`
Sends small test signals to a target (like `google.com`) to check if it's reachable and how long the round-trip takes.
```bash
ping -c 4 google.com     # -c 4 = send only 4 pings, then stop automatically
```

### `curl <url>`
Fetches whatever a URL returns — used to test if a website/API is responding, without needing an actual browser.
```bash
curl -I https://google.com    # -I fetches ONLY the response headers, not the full page
curl localhost                 # test a web server running on this same machine
```

### `dig <domain>`
Performs a DNS lookup — asks "what IP address does this domain name actually point to?"

### `ss -tulnp`
Shows which network ports are currently open/listening on this machine, and which program is using each one.
- `-t` = TCP connections
- `-u` = UDP connections
- `-l` = only LISTENING ports (waiting for connections)
- `-n` = show numeric addresses/ports instead of trying to resolve names (faster)
- `-p` = show the process name using each port

### `netstat -tulnp`
An older, similar tool to `ss` — same idea, showing active network connections and listening ports.

---

# 💾 Part 10: Disk Usage & Storage Checks

### `df -h`
- **d**isk **f**ree — shows how much space is used/available on each mounted disk/partition
- `-h` = human-readable (shows "6.8G" instead of a huge raw number)

### `du -sh <path>`
- **d**isk **u**sage — shows how much space a specific folder (and everything inside it) is taking up
- `-s` = summarize (just the total, not every single file individually)
- `-h` = human-readable

```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```
Breaking this down:
- `du -sh /var/log/*` → size of everything inside `/var/log`
- `2>/dev/null` → hide any "permission denied" error messages by sending them to a black hole (`/dev/null`)
- `| sort -h` → sort the results by size, human-readable-aware
- `| tail -5` → show just the biggest 5 entries

---

# 💽 Part 11: LVM (Logical Volume Manager) — Flexible Storage

### Why LVM exists
Normally, disk partitions are fixed in size — resizing them later is painful and risky. **LVM adds a flexible layer on top of raw disks**, letting you combine multiple disks into one pool, and grow storage later WITHOUT repartitioning or downtime.

### The 3-layer LVM structure

```
Physical Volume (PV)   ← a raw disk, initialized for LVM use
        ↓
Volume Group (VG)      ← a "pool" combining one or more Physical Volumes
        ↓
Logical Volume (LV)    ← the actual usable storage carved out of that pool
```

**Simple analogy:** Think of Physical Volumes as separate water tanks, a Volume Group as a shared reservoir connecting all those tanks together, and a Logical Volume as a pipe drawing exactly the amount of water you need from that shared reservoir — and you can always add more tanks (PVs) to the reservoir later without disrupting the pipe.

### `sudo -i`
Switches you fully into the root (administrator) user for the rest of the session, so you don't need to type `sudo` before every single command. LVM operations need root privileges, so this is often done first.

### `lsblk`
Lists all block devices (disks) attached to the machine, in a simple tree view — shows you exactly what raw storage is available before you start.

### `pvs` / `vgs` / `lvs`
Show currently existing Physical Volumes / Volume Groups / Logical Volumes respectively — a quick status check before and after making changes.

### `pvcreate <disk>`
Initializes a raw disk, turning it into a Physical Volume that LVM can now manage.
```bash
sudo pvcreate /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1
```

### `vgcreate <vg-name> <disk1> <disk2>`
Creates a Volume Group by combining one or more Physical Volumes into a single storage pool with a given name.
```bash
sudo vgcreate rohit_vg /dev/nvme1n1 /dev/nvme2n1
```

### `lvcreate -L <size> -n <lv-name> <vg-name>`
Carves out a Logical Volume of a specific size from a Volume Group's pool.
- `-L 15G` → allocate 15 gigabytes
- `-n rohit_lv` → name this Logical Volume "rohit_lv"
```bash
sudo lvcreate -L 15G -n rohit_lv rohit_vg
```

### `mkfs.ext4 <lv-path>`
Formats the Logical Volume with the EXT4 filesystem (a common Linux filesystem type) — a Logical Volume must be formatted before it can actually store files, just like a brand-new physical disk would need formatting.
```bash
sudo mkfs.ext4 /dev/rohit_vg/rohit_lv
```

### `mount <lv-path> <mount-point-folder>`
Attaches the formatted Logical Volume to a specific folder in your filesystem — after this, anything saved into that folder is actually being stored on the Logical Volume.
```bash
sudo mkdir /mnt/rohit_lv_mount
sudo mount /dev/rohit_vg/rohit_lv /mnt/rohit_lv_mount
```

### `lvextend -L +<amount> <lv-path>`
Grows an EXISTING Logical Volume by adding more space to it — this is LVM's big advantage: no need to destroy and recreate anything.
```bash
sudo lvextend -L +200M /dev/rohit_vg/rohit_lv
```

### `resize2fs <lv-path>`
After extending the Logical Volume, the FILESYSTEM on top of it also needs to be told to actually use that extra space. `resize2fs` expands the EXT4 filesystem to match the volume's new, larger size — often done live, with zero downtime.
```bash
sudo resize2fs /dev/rohit_vg/rohit_lv
```

**Full real-world flow, start to finish:**
```bash
sudo -i
lsblk                                            # see available disks
sudo pvcreate /dev/nvme1n1 /dev/nvme2n1           # make them Physical Volumes
sudo vgcreate rohit_vg /dev/nvme1n1 /dev/nvme2n1  # combine into a Volume Group
sudo lvcreate -L 15G -n rohit_lv rohit_vg          # carve out a Logical Volume
sudo mkfs.ext4 /dev/rohit_vg/rohit_lv              # format it
sudo mount /dev/rohit_vg/rohit_lv /mnt/my_mount    # mount it, ready to use
# ... later, need more space ...
sudo lvextend -L +200M /dev/rohit_vg/rohit_lv       # grow it
sudo resize2fs /dev/rohit_vg/rohit_lv               # tell the filesystem to use the extra space
```

---

# ☁️ Part 12: Cloud Deployment Basics (AWS EC2 + Nginx Example)

### Real deployment scenario, walked through step-by-step

**1. Launch a cloud server (EC2 instance)** — via AWS Console, generating a key pair for secure access.

**2. Connect and verify:**
```bash
whoami
hostname
pwd
```

**3. Update package lists before installing anything:**
```bash
sudo apt update
```

**4. Install and verify the web server:**
```bash
sudo apt install nginx -y
nginx -v
systemctl status nginx
```

**5. Common gotcha — website not loading in the browser:**
Error seen: `ERR_CONNECTION_TIMED_OUT`
**Root cause:** Cloud providers (like AWS) block ALL incoming network traffic by default, except what you EXPLICITLY allow via a "Security Group." Installing Nginx doesn't automatically open the door for web traffic to reach it — that's a separate, deliberate step.
**Fix:** Add an inbound Security Group rule allowing HTTP traffic (port 80) from anywhere (`0.0.0.0/0`).

**6. Verify the fix worked, from two angles:**
```bash
curl localhost              # verify locally, ON the server itself
```
Then also check from a browser using the server's public IP — confirming it's reachable from OUTSIDE too.

**7. Checking access logs:**
```bash
sudo tail -20 /var/log/nginx/access.log
```

**8. Common gotcha — "Permission denied" after copying a log file:**
```bash
sudo cp /var/log/nginx/access.log ~/nginx-logs.txt
cat ~/nginx-logs.txt        # → Permission denied!
```
**Root cause:** Since the copy was done with `sudo`, the new file got created as owned by `root`, not by the regular logged-in user.
**Fix:**
```bash
sudo chown ubuntu:ubuntu ~/nginx-logs.txt
```

**Key lesson from this whole scenario:** Deploying an app is never JUST "install the software." Networking (security groups), permissions, and ownership all play a role in whether something actually works end-to-end — this mirrors real production troubleshooting.

---

# 🧭 Part 13: The General Troubleshooting Approach

A structured, repeatable method for diagnosing ANY Linux issue — this mindset matters more than memorizing any single command:

```
Problem → Investigation → Logs → Root Cause → Fix
```

### A general troubleshooting runbook flow

**1. Check the service's basic health first:**
```bash
systemctl status <service>
```

**2. Then check its recent logs:**
```bash
journalctl -u <service> -n 50
```

**3. Watch logs live if the issue is ongoing:**
```bash
journalctl -u <service> -f
```

**4. Check the service's configuration file** (usually under `/etc/`):
```bash
cat /etc/<service>/config-file
```

### A general system health check flow (before diving into a specific service)
```bash
uname -a                    # kernel/system info
cat /etc/os-release         # OS version
top                         # CPU/memory snapshot
free -h                     # memory details
df -h                       # disk space
du -sh /var/log             # is a log folder eating disk space?
ss -tulnp                   # what ports are open/listening?
curl -I https://google.com  # is outbound internet working?
journalctl -u <service> -n 50   # recent logs for the service in question
tail -n 50 /var/log/syslog      # general system-wide recent logs
```

**Why this order matters:** Check the obvious, high-level things first (is it even running? is there disk space? is there memory?) before diving into deep logs — most issues turn out to be something simple (disk full, service not running, wrong permission) rather than something exotic.

---

# 🔑 Ultimate One-Line Summary Table

| Command | Plain-English meaning |
|---|---|
| `ls -la` | List all files/folders, including hidden ones, with details |
| `cd <folder>` | Move into a folder |
| `mkdir -p <path>` | Create a folder (and any missing parent folders) |
| `touch <file>` | Create an empty file |
| `cat` / `head -n` / `tail -n` | Print a whole file / just its start / just its end |
| `echo "x" > file` / `>> file` | Write text into a file (overwrite / append) |
| `chmod <perm> <file>` | Change what owner/group/others can do with a file |
| `chown <user>:<group> <file>` | Change who owns a file, and its group |
| `useradd -m` / `groupadd` / `usermod -aG` | Create a user / create a group / add a user to a group |
| `ps aux` / `top` | List all processes / live process monitor |
| `kill <pid>` / `kill -9 <pid>` | Stop a process gracefully / forcefully |
| `systemctl status/start/stop/restart <svc>` | Check or control a background service |
| `journalctl -u <svc> -n 50` | View recent logs for a specific service |
| `df -h` / `du -sh <path>` | Disk space overall / disk space used by a specific folder |
| `ip addr` / `ping` / `curl` / `ss -tulnp` | Networking checks — IP info / reachability / HTTP test / open ports |
| `pvcreate` / `vgcreate` / `lvcreate` | LVM: raw disk → Physical Volume → Volume Group → Logical Volume |
| `lvextend` + `resize2fs` | Grow an LVM Logical Volume and its filesystem live, without downtime |

---

# 💡 A Few Extra Things Worth Knowing (in simple words)

1. **`sudo` doesn't make you a different user permanently** — it just runs ONE command with temporary admin rights. `sudo -i`, on the other hand, actually switches your whole session to the root user until you exit it. Know the difference — `sudo -i` is more powerful and should be used more carefully.

2. **Files created via `sudo` are owned by root — this WILL bite you later.** This exact gotcha showed up twice in real practice (the nginx log file, and the `/opt/dev-project` folder). The pattern to remember: if a regular user suddenly gets "Permission denied" on something THEY just tried to interact with, check `ls -l` first — there's a good chance it's owned by `root` from an earlier `sudo` command.

3. **`/var/log` is usually the first place to look when ANYTHING goes wrong** — web servers, databases, SSH, custom apps — almost everything writes some kind of log there (or lets `journalctl` read it). Getting comfortable with `tail`, `grep`, and `journalctl` on log files is one of the highest-value Linux skills for real DevOps work.

4. **A "Zombie" process isn't dangerous, just annoying.** It means a process finished, but its parent process hasn't yet acknowledged/cleaned it up. A few zombies are totally normal on a busy system; large NUMBERS of them piling up usually points to a bug in whatever program keeps creating them (it's not cleaning up after its own child processes properly).

5. **Ports don't open themselves just because a service is running.** This is the single biggest "why isn't my server reachable" gotcha for anyone deploying on cloud platforms (AWS, GCP, Azure) — installing and starting Nginx is not enough; the cloud provider's firewall (Security Group, in AWS's case) ALSO has to explicitly allow that traffic in. Always check both layers: (1) is the app actually running and listening (`ss -tulnp`), AND (2) is the cloud firewall allowing that port through.

6. **LVM's biggest real-world value isn't the initial setup — it's the ability to grow storage later without downtime.** In traditional partitioning, running out of disk space often means a very risky, disruptive resize/migration process. With LVM, as long as you have spare Physical Volume space in the Volume Group, `lvextend` + `resize2fs` can grow a live, in-use filesystem in seconds, with zero downtime — hugely valuable for production database servers that must stay online.

---


