# Docker Commands — Complete Reference Table (Day 29–32)

A quick-reference table of every Docker command practiced, organized by category.

---

## 1. Installation & Service Management

| Command | Explanation |
|---|---|
| `sudo apt update` | Refreshes the list of available software versions before installing anything |
| `sudo apt install docker.io -y` | Installs Docker on Ubuntu; `-y` auto-confirms the installation |
| `docker --version` | Checks whether Docker is installed and shows the version |
| `sudo systemctl status docker` | Checks if the Docker background service (daemon) is currently running |
| `sudo systemctl enable docker` | Makes Docker start automatically every time the machine reboots |
| `sudo systemctl is-enabled docker` | Confirms whether auto-start on boot is enabled |

---

## 2. Running Containers

| Command | Explanation |
|---|---|
| `docker run <image>` | Creates and starts a new container from an image |
| `docker run -d <image>` | Runs the container in the background (detached mode) |
| `docker run -it <image> bash` | Runs interactively with a terminal attached, opening a shell inside |
| `docker run --name <name> <image>` | Gives the container a custom, memorable name |
| `docker run -p <host_port>:<container_port> <image>` | Maps a host port to a container port, so it's reachable from outside |
| `docker run -e KEY=VALUE <image>` | Sets an environment variable inside the container |
| `docker run -v <volume>:<path> <image>` | Attaches a named volume to a container path |
| `docker run -v <host_path>:<container_path> <image>` | Bind-mounts a host folder into a container path |
| `docker run --network <name> <image>` | Attaches the container to a specific Docker network |

---

## 3. Managing Running Containers

| Command | Explanation |
|---|---|
| `docker ps` | Lists all currently RUNNING containers |
| `docker ps -a` | Lists ALL containers — running, stopped, exited |
| `docker stop <container>` | Gracefully stops a running container (asks nicely, gives time to close) |
| `docker kill <container>` | Immediately force-stops a container (no cleanup time given) |
| `docker restart <container>` | Stops and starts the same container again in one command |
| `docker pause <container>` | Freezes a running container (stops CPU usage, stays in memory) |
| `docker unpause <container>` | Resumes a paused container |
| `docker rm <container>` | Deletes a stopped container permanently |
| `docker rm -f <container>` | Force stops AND removes a container in one step |
| `docker create --name <name> <image>` | Creates a container without starting it |
| `docker start <container>` | Starts a container that already exists but isn't running |

---

## 4. Inspecting & Debugging Containers

| Command | Explanation |
|---|---|
| `docker logs <container>` | Shows everything the container has printed to its console |
| `docker logs -f <container>` | Follows logs live/in real-time as new output appears |
| `docker exec -it <container> bash` | Opens an interactive shell inside an already-running container |
| `docker exec <container> <command>` | Runs a single command inside a running container without opening a full shell |
| `docker inspect <container_or_image>` | Shows a detailed JSON report — IPs, mounts, env vars, network settings, etc. |

---

## 5. Images

| Command | Explanation |
|---|---|
| `docker pull <image>` | Manually downloads an image from Docker Hub |
| `docker images` | Lists all images currently stored locally, with sizes |
| `docker rmi <image>` | Deletes an image permanently (must remove containers using it first) |
| `docker image history <image>` | Shows every layer used to build an image and its size |
| `docker build -t <name>:<tag> .` | Builds a custom image from a Dockerfile in the current folder |

---

## 6. Dockerfile Instructions

| Instruction | Explanation |
|---|---|
| `FROM <image>` | Chooses the base image to build on top of (always the first line) |
| `RUN <command>` | Executes a command during the image build process |
| `WORKDIR <path>` | Sets the default working folder for later instructions and the running container |
| `COPY <source> <destination>` | Copies files from the host into the image |
| `EXPOSE <port>` | Documents which port the app expects to use (doesn't actually open it) |
| `CMD [...]` | Sets the default startup command; can be overridden at `docker run` time |
| `ENTRYPOINT [...]` | Sets a fixed startup command; extra arguments get appended, not replaced |

---

## 7. Volumes (Data Persistence)

| Command | Explanation |
|---|---|
| `docker volume create <name>` | Creates a named, Docker-managed volume for persistent storage |
| `docker volume ls` | Lists all volumes on the machine |
| `docker volume inspect <name>` | Shows volume details, including its real host location (`Mountpoint`) |
| `docker volume rm <name>` | Permanently deletes a volume and its data |
| `docker volume prune` | Removes all volumes not attached to any container |
| `-v <volume>:<container_path>` (used inside `docker run`) | Attaches a named volume to a folder inside the container |

---

## 8. Bind Mounts

| Command | Explanation |
|---|---|
| `-v <host_path>:<container_path>` (used inside `docker run`) | Links a real host folder directly into a container folder — edits on host reflect instantly inside the container |

---

## 9. Networking

| Command | Explanation |
|---|---|
| `docker network ls` | Lists all Docker networks (`bridge`, `host`, `none`, and custom ones) |
| `docker network create <name>` | Creates a custom bridge network with built-in DNS (name-based container discovery) |
| `docker network inspect <name>` | Shows subnet, gateway, and containers connected to a network |
| `--network <name>` (used inside `docker run`) | Attaches a container to a specific network at startup |
| `ping -c 3 <target>` | Tests connectivity to another container by name (custom network only) or IP |

---

## 10. Cleanup / Disk Space

| Command | Explanation |
|---|---|
| `docker system df` | Shows disk space used by images, containers, volumes, and build cache |
| `docker container prune` | Removes all stopped containers |
| `docker image prune -a` | Removes all images not currently used by any container |
| `docker volume prune` | Removes all volumes not attached to any container |
| `docker stop $(docker ps -q)` | Stops every currently running container in one command |
| `df -h` | (Linux command, not Docker) Shows host machine's disk space usage |
| `free -h` | (Linux command, not Docker) Shows host machine's memory (RAM) usage |

---

## 11. MySQL Commands (used inside containers)

| Command | Explanation |
|---|---|
| `mysql -u root -pROOT` | Logs into MySQL as root, with the password attached directly (no space after `-p`) |
| `CREATE DATABASE dbname;` | Creates a new database |
| `USE dbname;` | Switches into a specific database |
| `SHOW DATABASES;` | Lists every database that exists |
| `CREATE TABLE tablename (...);` | Creates a table with defined columns |
| `INSERT INTO tablename VALUES (...);` | Adds a row of data to a table |
| `SELECT * FROM tablename;` | Shows all rows in a table |
| `EXIT;` | Leaves the MySQL shell |

---

## Quick Workflow Reference (typical Docker flow)

```
docker pull <image>                        → get the image
docker run -d --name <n> <image>           → start a container
docker ps                                  → confirm it's running
docker exec -it <n> bash                   → go inside to check/configure
docker logs <n>                            → check what happened
docker stop <n> && docker rm <n>           → clean up when done
```

## Quick Volume + Network Combo (production-style setup)

```
docker network create <net-name>
docker volume create <vol-name>
docker run -d --name db --network <net-name> -v <vol-name>:/var/lib/mysql mysql
docker run -d --name app --network <net-name> <app-image>
docker exec app ping -c 3 db              → confirm app can reach db by name
```
