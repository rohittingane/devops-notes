# 🐳 Master Docker Cheat Sheet (Day 29 – Day 32)

**A complete, beginner-friendly reference.** Every command is explained word-by-word — what it is, why we use it, and how to use it — so that even someone who has never touched Docker before can follow along.

---

# 📖 Part 0: The Basics You Must Know First

Before touching any command, understand these 4 words. Everything else builds on top of them.

### 1. **Image**
Think of an image like a **recipe** or a **blueprint**. It's a read-only file that contains everything needed to run an application — the code, the software it depends on, and its settings. You don't "run" a recipe, you use it to cook something.

Examples of images: `nginx`, `mysql`, `ubuntu`, `alpine`

### 2. **Container**
A container is the **actual dish cooked from that recipe** — a running (or stopped) copy created from an image. You can create many containers from the same image, just like you can cook the same dish multiple times from one recipe.

### 3. **Docker Daemon (`dockerd`)**
This is the background engine that does all the real work — building images, starting/stopping containers, managing networks and storage. You never talk to it directly.

### 4. **Docker Client**
This is what YOU type into — the `docker` command in your terminal. It sends your instructions to the Docker Daemon, which then does the actual work.

**Simple flow:**
```
You type "docker run nginx"
        |
        v
Docker Client sends the request
        |
        v
Docker Daemon does the work (finds image, creates container, starts it)
        |
        v
Container is running
```

### 5. **Docker Registry (Docker Hub)**
This is like an **online library of recipes (images)**. When you ask for an image Docker doesn't have locally, it downloads (pulls) it from here automatically. Default registry = **Docker Hub**.

### 6. Containers vs Virtual Machines (VMs) — why containers are popular

| Containers | Virtual Machines |
|---|---|
| Share the host computer's operating system | Carry their own full operating system |
| Very lightweight (few MBs) | Heavy (several GBs) |
| Start in seconds | Take minutes to boot |
| Use less RAM/CPU | Use much more RAM/CPU |

**In plain words:** A VM is like renting a whole separate house just to store one box. A container is like putting that box in a shared storage locker — much faster and cheaper, but you share the building (OS) with others.

---

# 📦 Part 1: Installing & First Steps with Docker

### `sudo apt update`
**What it means, word by word:**
- `sudo` → "do this as an administrator" (needed for system-level changes)
- `apt` → Ubuntu's package manager (a tool that installs software)
- `update` → refresh the list of available software versions

**Why we use it:** Makes sure your system knows about the latest available software before installing anything.

### `sudo apt install docker.io -y`
- `install docker.io` → install the Docker software package
- `-y` → automatically answer "yes" to any confirmation prompts, so it doesn't stop and wait for you

**Why:** This is literally the command that installs Docker onto your Linux machine.

### `docker --version`
**Meaning:** Asks Docker to tell you which version is installed.
**Use case:** Right after installing, to confirm it worked.
```
Output: Docker version 28.x.x
```

### `sudo systemctl status docker`
- `systemctl` → the tool that manages background services on Linux
- `status docker` → "tell me if the Docker service is currently running"

**Why:** Confirms the Docker engine (daemon) is actually alive and working, not just installed.

### `sudo systemctl enable docker`
**Meaning:** Tells Linux "start the Docker service automatically every time this machine reboots."
Without this, you'd have to manually start Docker every time you restart your server.

### `docker run hello-world`
This is the traditional **"first command"** every Docker learner runs.

**What actually happens step by step:**
1. Docker looks for the `hello-world` image on your machine
2. It's not there (first time), so Docker downloads it from Docker Hub
3. Docker creates a container from that image
4. The container runs, prints a friendly message, and exits

**Why it matters:** It's a safe, tiny way to confirm your entire Docker setup (client → daemon → registry → container) works end-to-end.

---

# 🚀 Part 2: Running Real Containers

### `docker run -d -p 8080:80 --name my-nginx nginx`

Let's break this **entire command word by word**, since it's the most-used command in Docker:

| Word/Flag | Meaning |
|---|---|
| `docker run` | "Create a new container and start it" |
| `-d` | **detached mode** — run in the background, give me my terminal back immediately |
| `-p 8080:80` | **port mapping** — connect host port `8080` to container port `80` (explained more below) |
| `--name my-nginx` | give this container a friendly, easy-to-remember name instead of a random one |
| `nginx` | which image to use — here, the Nginx web server |

### Understanding `-p 8080:80` (Port Mapping) in depth
Format is always: `-p <host_port>:<container_port>`

- The application (Nginx) inside the container is listening on port `80` — that's fixed, it's Nginx's default.
- But your container is "hidden" inside Docker's internal network. To reach it from your browser, you need to open a door on the **host machine** — that's the host port (`8080`).

**Simple analogy:** Think of the container as a room inside a building with no windows to the street. Port mapping is like installing a window (`8080`) on the building's outer wall that connects directly to that room's inner door (`80`). Now people outside (your browser) can look through window `8080` and see what's happening at door `80` inside.

So in your browser, you'd type:
```
http://<server-ip>:8080
```
And it shows you what's running on port `80` inside the container.

### `docker ps`
**Meaning:** "Show me all containers that are currently RUNNING."
```
CONTAINER ID   IMAGE   STATUS         PORTS
266b8ee69dc2   nginx   Up 24 minutes  0.0.0.0:8080->80/tcp
```

### `docker ps -a`
`-a` = **all**. Shows every container — running, stopped, exited — everything that still exists on the machine.

### `docker run -it ubuntu bash`
- `-i` → **interactive**, keep the input channel open so you can type
- `-t` → **tty**, give you a proper terminal-like screen
- `ubuntu` → the image to use
- `bash` → the command to run inside the container (opens a shell so you can type commands live)

**Why used together (`-it`):** Without both flags, you either can't type anything, or the output looks broken. Together, they give you a normal-feeling terminal *inside* the container.

**Once inside**, some basic Linux commands you'd use to explore:
```bash
pwd              # print working directory — "where am I right now?"
ls                # list files/folders in the current location
cat /etc/os-release   # show which OS version this container is based on
whoami            # show which user you're logged in as (usually 'root' inside containers)
hostname          # show the container's unique ID/name (its network identity)
exit              # leave the container's shell and return to your normal terminal
```

### `docker stop <name>`
Sends a **graceful shutdown signal** — asks the application inside nicely to finish up and close.

### `docker rm <name>`
**Removes (permanently deletes)** a container. It must be stopped first, or you'll get an error (unless you use `docker rm -f`, which forces stop + remove together).

### `docker logs <name>`
Shows you everything the application inside the container has printed to its console — extremely useful for checking if something started correctly or crashed.

### `docker exec -it <name> bash`
Similar to `docker run -it`, but the key difference is:
- `docker run` → creates a **brand-new** container from an image
- `docker exec` → goes **inside an already-running** container

**Why you'd use this:** To peek inside, debug, or manually configure something in a container that's already up and working.

---

# 🖼️ Part 3: Docker Images & Container Lifecycle

### `docker pull <image>`
**Meaning:** Manually download an image from Docker Hub *without* running a container yet.
```bash
docker pull nginx
```
(Normally `docker run` does this automatically if the image isn't present, but `pull` lets you download it ahead of time.)

### `docker images`
Lists every image you currently have stored locally, along with its size.

**Something to notice:** `alpine` images are tiny (a few MB) because Alpine Linux is a stripped-down, minimal OS — great for lightweight containers. `ubuntu` and `nginx` are bigger because they include more built-in tools.

### `docker inspect <image_or_container>`
Gives you a **deep, detailed JSON report** about an image or container — its ID, creation date, environment variables, network settings, mounted volumes, and more. Think of it as "show me everything Docker knows about this."

### `docker rmi <image>`
`rmi` = **remove image**. Deletes an image permanently.
> ⚠️ You'll get an error if any container (even a stopped one) is still using that image — you must remove the container first.

### `docker image history <image>`
Shows every **layer** that was used to build an image, and how much disk space each layer takes.

**What is a "layer," in simple words?**
Every instruction used to build an image (installing something, copying a file, etc.) creates a new stacked "layer" on top of the previous one — like stacking transparent sheets of paper, each adding something new. Docker reuses layers that haven't changed, which makes builds and downloads much faster.

```
Base Image (Layer 1)
     |
Layer 2 (e.g., installed a package)
     |
Layer 3 (e.g., copied app files)
     |
Final Image
```

---

## Container Lifecycle — the full journey a container goes through

A container isn't just "on" or "off" — it moves through several distinct states:

```
docker create   →  Created (exists, but not started)
docker start    →  Running
docker pause    →  Paused (frozen, uses no CPU, but stays in memory)
docker unpause  →  Running again
docker stop     →  Exited (asked nicely to shut down)
docker restart  →  Running (stopped and started again in one command)
docker kill     →  Exited (forced, immediate shutdown — no cleanup time given)
docker rm       →  Removed (permanently deleted)
```

### `docker create --name <name> <image>`
Creates a container but does **NOT** start it. Useful when you want to prepare something in advance without it running yet.

### `docker start <name>`
Starts a container that already exists (was created or was previously stopped).

### `docker pause <name>` / `docker unpause <name>`
`pause` freezes a running container completely (like hitting pause on a video) — it stops using CPU but stays in memory, ready to resume instantly with `unpause`.

### `docker kill <name>` vs `docker stop <name>`
- `stop` = polite request, gives the app time to save/close things properly
- `kill` = immediate force-stop, no warning, no cleanup time (like pulling the power plug)

Use `stop` normally; use `kill` only if a container is stuck/unresponsive.

---

## 🧹 Docker Cleanup Commands

Over time, unused images/containers/volumes pile up and eat disk space. These commands clean up:

### `docker container prune`
Deletes **all stopped** containers in one shot. Asks for confirmation first.

### `docker image prune -a`
`-a` = all. Deletes **all images not currently used** by any container (not just "dangling"/untagged ones).

### `docker volume prune`
Deletes all volumes **not attached to any container**.

### `docker system df`
Shows a summary: how much disk space Docker is using overall (images + containers + volumes + build cache), and how much of that can be reclaimed (freed up) by cleaning.

### `docker stop $(docker ps -q)`
A combo command:
- `docker ps -q` → lists only the **container IDs** (quiet mode, no extra columns) of running containers
- Wrapping it in `$( )` means "run this first, and use its output as input to the outer command"
- So the full command means: **"Stop every currently running container, all at once."**

---

# 🏗️ Part 4: Dockerfile – Building Your Own Images

### What is a Dockerfile?
A **plain text file** (no extension, just named `Dockerfile`) containing step-by-step instructions that Docker follows to build a custom image — like a written recipe you create yourself instead of using someone else's.

**Why it matters:** Instead of manually installing software and configuring a server every single time, you write it once in a Dockerfile, and Docker builds the exact same environment every time, on any machine.

```
Your Dockerfile (recipe)
        |
   docker build (cooking)
        |
   Docker Image (the dish, ready to serve)
        |
   docker run (serving/eating it)
        |
   Running Container
```

### Basic Dockerfile Instructions Explained

#### `FROM <image>`
**Always the first line.** Chooses the **base image** to build on top of — you're not starting from nothing, you're starting from something that already exists.
```dockerfile
FROM ubuntu
```
Meaning: "Start with a plain Ubuntu Linux system, and I'll add my stuff on top."

#### `RUN <command>`
Executes a command **during the image build process** (not when the container runs later — this happens once, while baking the image).
```dockerfile
RUN apt update && apt install -y curl
```
Meaning: "While building this image, update the package list and install `curl`."

#### `WORKDIR <path>`
Sets the **default folder** that all future instructions (and the container itself, once running) will operate from.
```dockerfile
WORKDIR /app
```
Meaning: "From now on, treat `/app` as the 'home base' folder inside this container."

#### `COPY <source> <destination>`
Copies files **from your computer (host)** into the image being built.
```dockerfile
COPY index.html .
```
Meaning: "Take the `index.html` file sitting next to this Dockerfile, and put it into the current working directory inside the image."

#### `EXPOSE <port>`
**Documentation only** — it tells anyone reading the Dockerfile "this app expects to use this port," but it does NOT actually open/map the port (you still need `-p` in `docker run` for that).
```dockerfile
EXPOSE 80
```

#### `CMD [...]`
Defines the **default command** that runs automatically when a container starts from this image.
```dockerfile
CMD ["echo", "Hello Docker"]
```
**Important:** CMD can be **overridden** — if you run `docker run <image> ls`, Docker ignores the CMD and runs `ls` instead.

### `docker build -t <name>:<tag> .`
Builds an image from a Dockerfile.

| Part | Meaning |
|---|---|
| `-t <name>:<tag>` | **tag** — give your image a name and version label (like `my-app:v1`) |
| `.` | the **build context** — meaning "look for the Dockerfile in the current folder, and use everything in this folder as available files to COPY" |

```bash
docker build -t my-ubuntu:v1 .
```

---

## CMD vs ENTRYPOINT — a common confusion, explained simply

### CMD
```dockerfile
FROM ubuntu
CMD ["echo", "Hello"]
```
- Acts like a **suggestion/default**.
- If you run `docker run my-image` → prints `Hello`
- If you run `docker run my-image ls` → the `ls` command **completely replaces** CMD, so it lists files instead

### ENTRYPOINT
```dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
```
- Acts like a **fixed, permanent instruction** that always runs.
- If you run `docker run my-image Rohit` → the word `Rohit` gets **appended as an argument** to `echo`, so it prints `Rohit`
- You can't easily replace the `echo` part — it's locked in.

**Simple analogy:**
- CMD is like a suggested outfit — you can change it if you want something else.
- ENTRYPOINT is like a uniform — the main piece is fixed, but you can accessorize (add arguments) on top.

**When to use which:**
- Use **CMD** when a container might run different commands depending on the situation.
- Use **ENTRYPOINT** when a container should always run one specific app (like a monitoring tool or an API server that should never do anything else).

---

## `.dockerignore` — what and why

A file (just like `.gitignore`) that tells Docker **which files/folders to skip** when building an image, even if they exist in your project folder.

```
node_modules
.git
*.md
.env
```

**Why this matters:**
- Smaller "build context" → faster builds
- Prevents accidentally copying sensitive files (like `.env` with passwords) into your image
- Avoids copying huge, unnecessary folders (like `node_modules`)

---

## Build Cache & Layer Ordering — why it matters

Every instruction in a Dockerfile creates a layer, and Docker **caches** (remembers) each layer. If you rebuild an image and an instruction hasn't changed, Docker reuses the old cached layer instead of redoing the work — saving huge amounts of build time.

**But here's the catch:** if you change an EARLY instruction, every layer AFTER it becomes invalid and must be rebuilt, even if those later instructions themselves didn't change.

**Best practice — order your Dockerfile like this:**
```dockerfile
FROM ubuntu
RUN apt install dependencies      # rarely changes → put early, benefits from caching
COPY application-code .           # changes often → put later
```
This way, when you just tweak your app's code, Docker doesn't have to reinstall all your dependencies again — it reuses that cached layer and only rebuilds the last step.

---

# 💾 Part 5: Volumes — Solving the "Data Disappears" Problem

### The Core Problem
Any file/data created **inside** a running container lives in that container's own private, temporary storage space (called the writable layer). The moment you delete that container (`docker rm`), that entire storage space — and everything in it — is destroyed forever.

**Real example:** Run a MySQL container, create a database with real data in it, then remove the container. Run a brand-new MySQL container — the database is completely gone. This is normal, expected Docker behavior, not a bug.

### `docker volume create <name>`
Creates a **named volume** — a piece of storage that Docker manages on your behalf, kept safely OUTSIDE any single container's lifecycle. Even if you delete every container using it, the volume itself (and its data) stays intact until you explicitly delete the volume too.

```bash
docker volume create mysql-data
```

### `docker volume ls`
Lists every volume that currently exists on the machine.

### `docker volume inspect <name>`
Shows detailed info about a volume, most importantly its **Mountpoint** — the real, physical folder path on your host machine where the data is actually stored (usually something like `/var/lib/docker/volumes/<name>/_data`).

### `docker volume rm <name>`
Permanently deletes a volume and everything inside it. Use carefully — this is irreversible.

### Attaching a volume to a container: `-v <volume_name>:<container_path>`
```bash
docker run -d -v mysql-data:/var/lib/mysql mysql:latest
```
| Side | Meaning |
|---|---|
| `mysql-data` (left of `:`) | the named volume, managed by Docker |
| `/var/lib/mysql` (right of `:`) | the folder INSIDE the container where the app (MySQL) normally stores its data |

**What this does:** Instead of MySQL's data being saved inside the container's own temporary storage, it gets redirected to live inside the `mysql-data` volume instead. Now, even if this exact container is deleted, a brand-new container attached to the SAME volume will see all the same data — nothing is lost.

---

# 📁 Part 6: Bind Mounts — Linking a Real Host Folder

### The Idea
A **bind mount** is different from a named volume. Instead of Docker managing a hidden storage location for you, YOU choose an actual, visible folder on your host machine, and link it directly into the container.

### Attaching a bind mount: `-v <host_folder_path>:<container_folder_path>`
```bash
docker run -d -v ~/nginx-site:/usr/share/nginx/html nginx:latest
```
| Side | Meaning |
|---|---|
| `~/nginx-site` (left) | a real folder on YOUR computer, that you created and can see/edit directly |
| `/usr/share/nginx/html` (right) | Nginx's default folder inside the container, where it looks for website files to serve |

**What happens:** These two folders become literally the SAME storage. Anything you change in `~/nginx-site` on your host instantly appears inside the container too — no restart, no rebuild needed. This is why bind mounts are amazing for development (edit code on your host, see changes live in the running container).

### Named Volume vs Bind Mount — side by side

| | Named Volume | Bind Mount |
|---|---|---|
| Who manages the location? | Docker (hidden folder) | You (any folder you pick) |
| Can you see/edit it directly? | Not easily | Yes, anytime |
| Best for | Databases, production data Docker should manage safely | Development — live code editing |
| Survives container removal? | Yes | Yes (it's just a host folder, was never tied to the container anyway) |

---

# 🌐 Part 7: Docker Networking — How Containers Talk to Each Other

### The Default Situation
When Docker is installed, it automatically creates a network called **`bridge`**, and every container joins it automatically unless told otherwise.

### `docker network ls`
Lists all networks on the machine. You'll normally see at least 3 defaults:
- `bridge` — the default network every container joins automatically
- `host` — container shares the host machine's network directly (no isolation)
- `none` — container gets no networking at all (fully isolated)

### `docker network inspect <name>`
Shows detailed info about a network:
```json
"Subnet": "172.17.0.0/16",
"Gateway": "172.17.0.1"
```
- **Subnet** — think of it as the "range of house numbers" available on this street; every container gets one address (IP) from within this range
- **Gateway** — the "main entrance/exit gate" of the network, connecting containers to the outside world (like the internet or the host machine)

### The BIG Limitation of the default `bridge` network
On the default bridge, containers **can talk to each other using IP addresses, but NOT by container name.**

```bash
docker exec container1 ping container2      # ❌ FAILS — "Name or service not known"
docker exec container1 ping 172.17.0.3       # ✅ WORKS
```

**Why?** Docker only runs its built-in "name lookup service" (called embedded DNS) on custom networks, not on the old, legacy default bridge network.

**Why this matters in real life:** If your app's code says `connect_to_database("db-server")`, and both containers are on the default bridge, this line will FAIL. You'd have to hardcode an IP address instead — and that IP can change every time the container restarts, breaking your app repeatedly.

### `docker network create <name>`
Creates your own **custom bridge network**. This is the fix to the above problem.
```bash
docker network create my-app-net
```

### Attaching a container to a custom network: `--network <name>`
```bash
docker run -d --name app1 --network my-app-net nginx:latest
docker run -d --name app2 --network my-app-net nginx:latest
```

### Now name-based communication WORKS:
```bash
docker exec app1 ping -c 3 app2     # ✅ SUCCESS!
```

**Why does this work now?** On a custom network, Docker runs an embedded DNS resolver — every container's name gets automatically mapped to its current IP address in an internal lookup table. So even if a container restarts and gets a brand-new IP, other containers can still reach it using its unchanging NAME.

**This is exactly why real production setups (and tools like `docker-compose`) always use custom networks** — so your app's code can reference other services by a stable name, never worrying about IP changes.

### `ping -c 3 <target>`
| Part | Meaning |
|---|---|
| `ping` | send a small test signal to check if a target is reachable |
| `-c 3` | send only 3 test signals, then stop automatically (without this, ping runs forever until you press Ctrl+C) |
| `<target>` | the container name (works only on custom networks) or IP address (works everywhere) |

> ⚠️ Common gotcha: lightweight images like `nginx` and `mysql` don't come with `ping` pre-installed. You'll need to install it manually first:
> ```bash
> docker exec <container> apt update
> docker exec <container> apt install -y iputils-ping
> ```

---

# 🏗️ Part 8: Putting It All Together — Real Multi-Container Setup

A realistic mini production setup combines everything above:

```bash
# 1. Create a custom network (so containers can find each other by name)
docker network create shop-net

# 2. Create a volume (so the database's data survives container restarts)
docker volume create shop-db-data

# 3. Run the database, on the custom network, with the volume attached
docker run -d --name shop-db \
  -e MYSQL_ROOT_PASSWORD=ROOT \
  --network shop-net \
  -v shop-db-data:/var/lib/mysql \
  mysql:latest

# 4. Run the app, on the SAME custom network
docker run -d --name shop-app --network shop-net nginx:latest

# 5. Confirm the app can reach the database purely by NAME
docker exec shop-app ping -c 3 shop-db
```

**What this achieves:**
- The database's data is protected (won't vanish if the container is recreated)
- The app can always reach the database using the simple name `shop-db` — no IP hardcoding, no breakage on restarts

This is essentially a simplified, manual version of what tools like `docker-compose` automate for you in real projects.

---

# 🗄️ Bonus: MySQL Quick Commands (used inside containers)

```sql
CREATE DATABASE dbname;          -- make a new database
USE dbname;                      -- switch into that database
SHOW DATABASES;                  -- list every database that exists
CREATE TABLE tablename (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    role VARCHAR(50)
);                                -- create a table with named columns
INSERT INTO tablename (name, role)
VALUES ('Rohit', 'DevOps Engineer');   -- add one row of data
SELECT * FROM tablename;         -- show every row in the table
EXIT;                             -- leave the MySQL shell
```

Connecting to MySQL from inside a container:
```bash
mysql -u root -pROOT
```
⚠️ **Common mistake:** `-p ROOT` (with a space) breaks the command — MySQL thinks "ROOT" is a database name, not a password. Always write it as `-pROOT` (no space) when passing the password directly on the command line.

---

# 🔑 Ultimate One-Line Summary Table

| Command | Plain-English meaning |
|---|---|
| `docker run` | Create and start a new container from an image |
| `docker ps` / `docker ps -a` | Show running containers / show ALL containers |
| `docker exec -it <c> bash` | Go inside an already-running container |
| `docker stop` / `docker kill` | Politely stop / forcefully stop a container |
| `docker rm` / `docker rmi` | Delete a container / delete an image |
| `docker logs` | See what a container has printed to its console |
| `docker build -t name .` | Build a custom image from a Dockerfile |
| `FROM` / `RUN` / `COPY` / `CMD` | Dockerfile instructions: base image / run a setup command / copy files in / default startup command |
| `docker volume create` | Make Docker-managed storage that survives container deletion |
| `-v vol:/path` | Attach a named volume to a container |
| `-v /host/path:/path` | Bind-mount a real host folder into a container |
| `docker network create` | Make a custom network with name-based container discovery |
| `--network <name>` | Attach a container to a specific network |
| `docker system df` / `prune` commands | Check and free up Docker's disk usage |
| `-p host:container` | Open a "window" from your host machine into a container's internal port |

---

# 💡 A Few Extra Things Worth Knowing (in simple words)

1. **Images are read-only, containers are not.** Once you `docker run` an image, Docker adds a thin writable layer on top just for that container. This is why one image can create hundreds of independent, isolated containers without them interfering with each other.

2. **Restarting ≠ Rebuilding.** `docker restart` just stops and starts the same container again — it does NOT rebuild the image or reset any volume-backed data. Data in a volume survives restarts easily; it's only `docker rm` (removal) that's the real danger point for data stored inside the container itself.

3. **Exit code 137 usually means "killed by the system"** — most commonly because the machine ran out of memory or disk space, and Linux/Docker had to forcibly stop the container to protect the system. If you see this, check `df -h` (disk space) and `free -h` (memory) first.

4. **Docker Hub is just one registry among many.** Companies often run their own private registries (like AWS ECR, Google Artifact Registry, or a self-hosted one) for storing internal images that shouldn't be public.

5. **The `latest` tag is not magic.** When you write `mysql:latest`, Docker just pulls whatever image happens to be tagged "latest" at that moment — it is NOT automatically the newest, safest, or most stable version for production use. In real projects, it's safer to pin an exact version (e.g., `mysql:8.0.36`) so your setup doesn't unexpectedly change when the "latest" tag gets reassigned to a newer release.

6. **Detached mode (`-d`) doesn't mean "safer" or "faster"** — it just means the container runs in the background instead of taking over your terminal. The container behaves identically either way; `-d` is purely about your terminal's convenience.

---


