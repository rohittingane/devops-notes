# 🐚 Master Shell Scripting Cheat Sheet (Day 16 – Day 21)

**A complete, beginner-friendly reference.** Every concept explained word-by-word — what it is, why we use it, and how to use it — so that even someone who has never written a single line of shell script can follow along and start writing their own.

---

# 📖 Part 0: The Absolute Basics — What IS a Shell Script?

### What is a "shell," first of all?

When you type a command like `ls` or `cd` into your terminal, something has to actually READ what you typed, understand it, and run it. That "something" is called the **shell** — it's a program that sits between you and the operating system, translating your typed commands into actions.

Common shells: `bash`, `sh`, `zsh`. We'll focus on **bash** (Bourne Again SHell), the most widely used one.

### What is a "shell script," then?

Instead of typing commands one at a time, every single time, a **shell script** is just a text file containing a LIST of commands, saved together, so you can run all of them at once by executing that one file.

**Simple analogy:** Typing commands manually is like giving someone spoken directions one turn at a time, in person, every single trip. A shell script is like writing those directions down once on paper — now anyone (including future-you) can follow the exact same route instantly, without you repeating yourself.

### Why do DevOps engineers rely on shell scripts so heavily?

- **Automation** — instead of manually running 10 commands to deploy an app, one script does it in one click
- **Consistency** — the exact same steps run the exact same way, every single time, with no typos or forgotten steps
- **Repeatability** — backups, health checks, log cleanups can run automatically on a schedule (via `cron`), with zero human involvement

**Real DevOps example:** A deployment that manually required "pull latest code → install dependencies → restart the app → check if it's healthy" becomes a single script: `./deploy.sh`, runnable by anyone, anytime, identically.

---

# ✍️ Part 1: Writing & Running Your First Script

### The Shebang — `#!/bin/bash`

**This must be the very FIRST line of every script.** It tells the operating system exactly which program (interpreter) should be used to run the rest of the file.
```bash
#!/bin/bash
echo "Hello, DevOps!"
```

**Word by word:**
- `#!` — a special two-character marker (called "shebang") that the OS specifically looks for at the start of a file
- `/bin/bash` — the exact file path to the bash program on this system

**Why it matters, concretely:** Without this line, if someone runs your script, the system might use a DIFFERENT shell as a default, which could interpret certain syntax slightly differently — leading to confusing bugs that only show up on some machines and not others. The shebang guarantees consistency everywhere.

### Making a script runnable, and running it

```bash
chmod +x script.sh    # give the file "execute" permission
./script.sh            # run it — the "./" means "look in the current folder"
```

**Alternative way, without needing execute permission:**
```bash
bash script.sh         # explicitly tells bash to run this file's contents
```

### Comments — notes for humans, ignored by the computer

Anything after a `#` symbol on a line is a comment — completely ignored when the script runs. It exists purely to explain WHY something is written a certain way, for the next person reading it (which is often you, 6 months later, having forgotten your own logic).

```bash
# This entire line is a comment, explaining the next command
echo "Hello"   # this part after the # is also a comment
```

**Good habit:** Comment WHY you did something unusual, not WHAT a simple command obviously does. `# retry 3 times because the API is occasionally slow to wake up` is useful; `# print hello` above an `echo "hello"` is not.

---

# 📦 Part 2: Variables — Storing Values

### What is a variable, in plain words?

A variable is just a **named box** that holds a piece of information (text, a number, a file path) so you can refer to it by name later, instead of retyping the actual value everywhere.

```bash
NAME="Rohit"
echo $NAME          # prints: Rohit
```

### Critical syntax rule: NO SPACES around the `=` sign

```bash
NAME="Rohit"     # ✅ correct
NAME = "Rohit"   # ❌ WRONG — bash will think "NAME" is a command to run, and error out
```
This trips up almost every beginner at least once — bash is very strict about this specific spacing rule.

### `$NAME` vs `"$NAME"` vs `'$NAME'` — the quoting rules that matter

```bash
NAME="Rohit Tingane"
echo $NAME       # prints: Rohit Tingane  (BUT risky — see below)
echo "$NAME"     # prints: Rohit Tingane  (SAFEST — always preserves the value exactly)
echo '$NAME'     # prints: $NAME (literally!) — single quotes prevent ANY variable expansion
```

**Why does the unquoted version say "risky"?** If a variable contains spaces or special characters, leaving it unquoted can cause bash to accidentally split it into multiple separate pieces (called "word splitting"), breaking your script in subtle ways. **Golden rule: almost always wrap variables in double quotes (`"$VAR"`) unless you have a specific reason not to.**

### Reading input from the user

```bash
read -p "Enter your name: " NAME
echo "Hello, $NAME"
```
- `-p "..."` shows a prompt message before waiting for input
- Whatever the user types gets stored into the `NAME` variable

**Real use:** Interactive setup scripts, e.g. "Which environment do you want to deploy to? (dev/staging/prod)"

### Command-line arguments — passing values INTO a script when you run it

When you run `./script.sh hello world`, the script can access those extra words:

| Variable | Meaning |
|---|---|
| `$0` | the script's own name/path |
| `$1` | the FIRST argument passed (`hello`) |
| `$2` | the SECOND argument passed (`world`) |
| `$#` | the TOTAL number of arguments passed |
| `$@` | ALL the arguments, as a list |
| `$?` | the exit code of the LAST command that ran (0 = success, anything else = some kind of failure) |

```bash
#!/bin/bash
echo "Script name: $0"
echo "First arg: $1"
echo "Total args: $#"
```
**Real use:** Custom tools like `./deploy.sh production` or `./backup.sh --force` — the script behaves differently depending on what you typed after its name.

---

# 🔀 Part 3: Conditionals — Making Decisions in a Script

### `if / elif / else / fi` — the basic decision structure

```bash
if [ -f "file.txt" ]; then
    echo "File exists"
elif [ -d "file.txt" ]; then
    echo "It's actually a directory"
else
    echo "Not found at all"
fi
```

**Structure explained:**
- `if [ condition ]; then` → "IF this condition is true, do the following"
- `elif [ another condition ]; then` → "ELSE IF that wasn't true, but THIS is, do this instead"
- `else` → "if NONE of the above were true, do this"
- `fi` → marks the END of the if-block (it's literally "if" spelled backwards — bash's way of closing blocks)

### The `[ ]` square brackets — what are they, really?

They're actually a COMMAND (called `test`), just written with a friendlier-looking bracket syntax. The spaces around the brackets `[ condition ]` are **mandatory** — `[condition]` (no spaces) will cause an error, since bash needs to see `[` as a separate, standalone command.

### String comparisons

```bash
[ "$a" = "$b" ]     # true if strings are EQUAL
[ "$a" != "$b" ]    # true if strings are NOT equal
[ -z "$a" ]          # true if the string is EMPTY (zero length)
[ -n "$a" ]          # true if the string is NOT empty
```
**Real use:** `if [ "$ENV" = "production" ]` before applying extra-careful safety checks in a deployment script.

### Integer (number) comparisons — NOT the same symbols as strings!

```bash
[ "$a" -eq "$b" ]   # equal
[ "$a" -ne "$b" ]   # not equal
[ "$a" -lt "$b" ]   # less than
[ "$a" -gt "$b" ]   # greater than
[ "$a" -le "$b" ]   # less than or equal
[ "$a" -ge "$b" ]   # greater than or equal
```
⚠️ **Common beginner mistake:** using `=` to compare NUMBERS. `[ "$a" = "$b" ]` compares them as TEXT, which can behave unexpectedly for numbers with different formatting (like `"05"` vs `"5"`). Always use `-eq`, `-gt`, etc. for actual numeric comparisons.

**Real use:** Monitoring scripts — `if [ "$DISK_USAGE" -gt 80 ]` to trigger an alert when disk usage crosses 80%.

### File test operators — checking if something exists, before touching it

```bash
[ -f file ]   # true if it's a regular FILE that exists
[ -d dir ]    # true if it's a DIRECTORY that exists
[ -e path ]   # true if the path exists AT ALL (file or directory)
[ -r file ]   # true if it's readable
[ -w file ]   # true if it's writable
[ -x file ]   # true if it's executable
[ -s file ]   # true if the file exists AND is not empty
```
**Real use:** `if [ -f .env ]` before an app starts — checking a config file actually exists before relying on it, instead of crashing with a confusing error later.

### Logical operators — combining conditions

```bash
[ -f file ] && echo "exists"          # AND — run the right side ONLY IF the left side was true
[ -f file ] || echo "not found"       # OR — run the right side ONLY IF the left side was FALSE
! [ -f file ] && echo "missing"       # NOT — flips true/false
```
**Real use:** `curl -sf http://localhost:8080 || systemctl restart myapp` — "try to check the app is healthy, and IF THAT FAILS, restart it."

### `case` statements — cleaner than many `elif`s, for matching one variable against many options

```bash
case "$1" in
    start) echo "Starting..." ;;
    stop)  echo "Stopping..." ;;
    *)     echo "Usage: $0 {start|stop}" ;;
esac
```
- Each option ends with `;;` (two semicolons)
- `*)` is the "catch-all" — matches anything not covered above (like a default/else)
- `esac` closes the case block ("case" spelled backwards)

**Real use:** Classic service control scripts responding to `start`, `stop`, `restart`, `status` as an argument.

---

# 🔁 Part 4: Loops — Repeating Actions

### `for` loop — repeat once for each item in a list

```bash
for fruit in apple banana mango; do
    echo "$fruit"
done
```
Reads as: "FOR each word in this list, one at a time, call it `fruit`, and run the block between `do` and `done`."

**C-style version** (for counting):
```bash
for ((i=0; i<5; i++)); do
    echo "$i"
done
```
This runs 5 times, with `i` being `0, 1, 2, 3, 4`.

**Real use:** Looping through a list of server names to run the same deployment command on each one via SSH.

### `while` loop — repeat AS LONG AS a condition stays true

```bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done
```
`((count++))` increases the `count` variable by 1 each time — without this, the loop would run FOREVER, since the condition would never become false.

**Real use:** Retry logic — keep checking every few seconds until a freshly-deployed service becomes healthy.

### `until` loop — the exact opposite of `while` — repeat UNTIL a condition becomes true

```bash
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done
```
**Real use:** Waiting until a database's port opens (becomes ready) before starting the app that depends on it.

### Loop control: `continue` and `break`

```bash
for i in 1 2 3 4 5; do
    [ $i -eq 3 ] && continue   # SKIP the rest of this one iteration, move to the next
    [ $i -eq 5 ] && break      # STOP the entire loop immediately
    echo "$i"
done
```
**Real use:** `continue` to skip already-processed files during a backup scan; `break` to stop scanning early once too many errors are found.

### Looping over files in a folder

```bash
for file in *.log; do
    echo "Processing $file"
done
```
`*.log` is a "wildcard pattern" — matches every file ending in `.log` in the current folder.

**Real use:** Compressing/archiving every log file during a nightly log-rotation job.

### Looping over the lines of a file

```bash
while read line; do
    echo "Server: $line"
done < servers.txt
```
This reads `servers.txt` one line at a time, storing each line into `line`, running the loop body for each one.

**Real use:** Reading a list of server names/IPs from a file, and SSH-ing into each one to run a health check.

---

# 🧩 Part 5: Functions — Reusable Blocks of Code

### What is a function, and why bother?

A function is a named, reusable chunk of script logic — instead of copy-pasting the same 5 lines of code in 10 different places (and having to fix all 10 copies if something's wrong), you write it ONCE as a function, and just "call" it by name wherever needed.

### Defining and calling a function

```bash
greet() {
    echo "Hello, $1!"
}
greet "DevOps"      # calling the function, passing "DevOps" as an argument
```
**Notice:** Inside a function, `$1` refers to the argument passed to the FUNCTION (not to the whole script) — functions have their own separate `$1`, `$2`, etc.

### Return values — functions can signal success/failure, OR actually return data

```bash
# Signaling success (0) or failure (1) — used for conditional checks
check_even() {
    if (( $1 % 2 == 0 )); then
        return 0   # 0 conventionally means "success" in shell scripting
    else
        return 1   # any non-zero number means "failure"
    fi
}
check_even 4 && echo "It's even"

# Actually RETURNING a value/data (not just success/fail) — use echo + capture it
get_sum() {
    echo $(( $1 + $2 ))
}
result=$(get_sum 3 4)     # captures the function's "echo" output into a variable
echo "Sum is: $result"
```

⚠️ **Important, often-confusing point:** `return` in bash can ONLY send back a number (0-255), meant as a status code — NOT actual data like text or calculated values. To get real data OUT of a function, the function should `echo` it, and the caller captures that output using `$(...)`.

### `local` variables — keeping a function's variables private to itself

```bash
myfunc() {
    local temp="only visible inside this function"
    echo "$temp"
}
```
Without `local`, a variable created inside a function is actually GLOBAL by default in bash — meaning it could accidentally overwrite a same-named variable elsewhere in a large script. `local` prevents this leakage.

**Real use:** A `restart_service()` function that takes a service name as an argument, so the same function can restart nginx, mysql, or any other service identically.

---

# 🔎 Part 6: Text Processing Tools — The Real Power Tools

These commands are what make shell scripting genuinely powerful for real DevOps work — searching, filtering, and reshaping text data.

### `grep` — search for a pattern inside text

```bash
grep "error" file.txt        # find lines containing "error"
grep -i "error" file.txt     # -i = case-insensitive (matches "Error", "ERROR" too)
grep -r "TODO" ./src         # -r = recursive, search every file inside a folder
grep -c "error" file.txt     # -c = count how many matching lines, instead of showing them
grep -n "error" file.txt     # -n = show the line NUMBER next to each match
grep -v "error" file.txt     # -v = invert — show lines that DON'T match
```
**Real use:** Searching application logs for `"error"` or `"exception"` during an incident, to quickly find what went wrong.

### `awk` — process text COLUMN BY COLUMN

```bash
awk '{print $1}' file.txt              # print just the first column of every line
awk -F: '{print $1}' /etc/passwd       # -F: sets ":" as the column separator (instead of the default space)
awk '$3 > 50 {print $1}' data.txt      # only process lines where column 3 is greater than 50
```
**Why it's so useful:** Most command output (like `df -h` or `ps aux`) is organized into columns. `awk` lets you grab EXACTLY the column you need, instead of the whole messy line.

**Real use:** Parsing `df -h` output to pull out just the disk-usage-percentage column for a monitoring script.

### `sed` — find and replace text (a "stream editor")

```bash
sed 's/old/new/' file.txt         # replace the FIRST match of "old" with "new", per line
sed 's/old/new/g' file.txt        # replace ALL matches (g = global) on each line
sed -i 's/foo/bar/g' file.txt     # -i = edit the file IN PLACE (saves the change directly)
```
**Real use:** Automatically updating a config value (e.g., `APP_ENV=dev` → `APP_ENV=prod`) during a deployment script, without manually opening the file.

### `cut` — extract specific columns by position

```bash
cut -d: -f1 /etc/passwd     # -d: sets ":" as delimiter, -f1 = extract only field 1
cut -d, -f2,3 data.csv       # extract fields 2 AND 3 from a comma-separated file
```
**Real use:** Pulling just the username column out of `/etc/passwd`, or a specific column from a CSV export.

### `sort` — arrange lines in order

```bash
sort file.txt          # alphabetical order
sort -n file.txt        # -n = numerical order (important! alphabetical sort treats "10" as coming before "9")
sort -r file.txt        # -r = reverse order
sort -u file.txt         # -u = unique — removes duplicate lines while sorting
```

### `uniq` — remove/count duplicate ADJACENT lines

```bash
sort file.txt | uniq          # removes consecutive duplicate lines (must be sorted first!)
sort file.txt | uniq -c       # -c = also show HOW MANY times each line appeared
```
**Important:** `uniq` only removes duplicates that are NEXT to each other — this is why it's almost always used AFTER `sort` first, so identical lines end up grouped together.

**Real use:** Counting how many times each IP address appears in an access log, to spot suspicious traffic patterns.

### `tr` — translate or delete individual characters

```bash
echo "hello" | tr 'a-z' 'A-Z'   # convert lowercase to uppercase
echo "hello" | tr -d 'l'         # -d = delete a specific character entirely
```

### `wc` — count lines, words, or characters

```bash
wc -l file.txt   # -l = count LINES
wc -w file.txt   # -w = count WORDS
wc -c file.txt   # -c = count CHARACTERS/bytes
```
**Real use:** Quickly checking how many error lines exist in a log, or confirming a file isn't accidentally empty after a transfer.

### `head` / `tail` — see just the start or end of a file

```bash
head -n 10 file.txt    # first 10 lines
tail -n 10 file.txt    # last 10 lines
tail -f app.log        # -f = FOLLOW the file live, showing new lines as they're written
```
**Real use:** `tail -f` is one of THE most-used commands during a live incident — watching an application's log in real time to catch an error the moment it happens.

### The `|` (pipe) symbol — chaining commands together

You'll notice many examples above use `|`. This takes the OUTPUT of the command on the left, and feeds it as INPUT to the command on the right — letting you chain multiple simple tools together into one powerful combined command.
```bash
cat access.log | grep "404" | wc -l
```
Reads as: "Show the file → keep only lines with '404' → count how many lines are left." Three simple tools combined into one useful answer: "how many 404 errors happened?"

---

# 🛡️ Part 7: Error Handling & Debugging — Writing SAFE Scripts

### Exit codes — how a command reports success or failure

Every command, when it finishes, sets an "exit code" — `0` always means success; any OTHER number means some kind of failure occurred.

```bash
some_command
echo $?     # shows the exit code of the command that JUST ran
```

### `set -e` — stop the whole script immediately if ANY command fails

```bash
#!/bin/bash
set -e
```
Without this, bash's DEFAULT behavior is to just keep going even after a command fails — which can be dangerous. If step 2 of a deployment fails, you probably don't want step 3 to blindly run on top of a broken state.

### `set -u` — treat using an UNDEFINED variable as an error

```bash
set -u
```
Without this, a typo like `$DATABSE_URL` (missing an "A") silently evaluates to an EMPTY string instead of throwing an error — potentially deploying with a blank, broken config, without any warning. `set -u` catches this immediately.

### `set -o pipefail` — catch failures hidden inside a pipe chain

```bash
set -o pipefail
```
Normally, a pipeline like `curl badurl | grep "ok"` only reports the EXIT CODE of the LAST command (`grep`), even if `curl` itself completely failed. `pipefail` makes the WHOLE pipeline report failure if ANY part of it failed — critical for reliable health-check scripts.

### The recommended "safety header" for almost every serious script

```bash
#!/bin/bash
set -euo pipefail
```
This single line combines all three protections above. **This is considered a best-practice starting point for any real, production-facing shell script.**

### `set -x` — debug mode, see every command as it executes

```bash
set -x    # turn ON — prints each command (and its actual values) right before running it
set +x    # turn OFF
```
**Real use:** Temporarily added when a script is misbehaving, to see EXACTLY which command ran, and with what actual variable values, right at the moment something went wrong.

### `trap` — guarantee cleanup happens, even if the script crashes

```bash
cleanup() {
    echo "Cleaning up temp files..."
    rm -f /tmp/tempfile
}
trap cleanup EXIT
```
This says: "No matter HOW this script ends — success, failure, or crash — always run the `cleanup` function before actually exiting."

**Real use:** Making sure temporary files, lock files, or open SSH tunnels always get removed/closed, even if the script dies unexpectedly halfway through.

---

# 🏗️ Part 8: Real-World Script Patterns (Complete Examples)

### Health Check Script
```bash
#!/bin/bash
set -euo pipefail

THRESHOLD=80
DISK=$(df -h / | awk 'NR==2{gsub("%","",$5); print $5}')

if [ "$DISK" -gt "$THRESHOLD" ]; then
    echo "⚠️  ALERT: Disk usage above ${THRESHOLD}%!"
fi
```
**What's happening:** `df -h /` gets disk usage info; `awk 'NR==2{...}'` grabs only the SECOND line of output (the actual data row, skipping the header); `gsub("%","",$5)` strips the `%` symbol from the 5th column so it can be compared as a plain number.

### Backup Script with Rotation
```bash
#!/bin/bash
set -euo pipefail

SRC_DIR="/var/www/myapp"
BACKUP_DIR="/backups"
DATE=$(date +%F_%H-%M-%S)
BACKUP_FILE="$BACKUP_DIR/myapp_backup_$DATE.tar.gz"

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_FILE" "$SRC_DIR"
echo "Backup created: $BACKUP_FILE"

# keep only the last 5 backups, delete anything older
cd "$BACKUP_DIR"
ls -tp myapp_backup_*.tar.gz | tail -n +6 | xargs -I {} rm -- {}
```
**What's happening:** `date +%F_%H-%M-%S` generates a timestamp for a unique filename; `tar -czf` compresses the folder into one file; the last line lists backups newest-first (`ls -tp`), skips the first 5 (`tail -n +6`), and deletes anything left over — a simple "keep only the 5 most recent" rotation.

### Service Watchdog
```bash
#!/bin/bash
SERVICE="nginx"

if ! systemctl is-active --quiet "$SERVICE"; then
    echo "$(date): $SERVICE is down. Restarting..." >> /var/log/watchdog.log
    systemctl restart "$SERVICE"
fi
```
**What's happening:** `systemctl is-active --quiet` silently checks if a service is running (no output, just a true/false result); if it's NOT active, the script logs the event and restarts it. Meant to be run automatically every few minutes via a scheduler (like `cron`).

---

# 🧵 Part 9: Bonus — Scheduling Scripts to Run Automatically (`cron`)

A shell script by itself only runs when someone (or something) triggers it. To run it AUTOMATICALLY on a schedule (e.g., "every night at 2 AM"), Linux uses a scheduler called **cron**.

```bash
crontab -e     # opens your personal schedule file for editing
```
A cron schedule line looks like this:
```
0 2 * * * /home/user/backup.sh
```
Reading left to right: minute (`0`), hour (`2`), day of month (`*` = any), month (`*` = any), day of week (`*` = any), then the command to run. This example means: "run `backup.sh` at 2:00 AM, every single day."

**Why this matters for shell scripting:** Most real-world shell scripts (backups, log cleanup, health checks) aren't run manually — they're written once, then scheduled via `cron` to run forever, unattended.

---

# 🔑 Ultimate One-Line Summary Table

| Concept | Plain-English meaning |
|---|---|
| `#!/bin/bash` | Tells the OS which program should run this script |
| `VAR="value"` | Stores a value in a named variable (no spaces around `=`) |
| `"$VAR"` | Safely use a variable's value (always prefer double quotes) |
| `$1`, `$2`, `$#`, `$@` | Arguments passed to the script when it was run |
| `if [ cond ]; then ... fi` | Run code only if a condition is true |
| `for x in list; do ... done` | Repeat once per item in a list |
| `while [ cond ]; do ... done` | Repeat WHILE a condition stays true |
| `function() { ... }` | Define a reusable, named block of code |
| `grep` | Search text for a pattern |
| `awk` | Process text column-by-column |
| `sed` | Find and replace text |
| `sort` / `uniq` | Order lines / remove duplicates |
| `set -euo pipefail` | Make a script fail loudly and safely instead of silently continuing |
| `trap cleanup EXIT` | Guarantee a cleanup step runs, no matter how the script ends |
| `\| ` (pipe) | Feed one command's output as the next command's input |

---

# 💡 How to Write a CLEAN Shell Script — My Own Extra Tips

1. **Always start with the safety header.** `#!/bin/bash` followed immediately by `set -euo pipefail` should be the first two lines of almost every serious script you write. It costs nothing and catches entire categories of silent, dangerous bugs before they cause real damage.

2. **Quote your variables. Always. Even when you "know" it's fine.** `"$VAR"` should be your default habit, not an afterthought — the day you forget, on a variable that unexpectedly contains a space or a special character, is the day a script does something you didn't intend.

3. **Name your variables in CAPITALS for constants/config, lowercase for temporary/loop variables.** This isn't required by bash, but it's a widely followed convention that makes scripts far easier to read at a glance — you instantly know `$BACKUP_DIR` is a fixed setting, while `$i` or `$count` is just a loop counter.

4. **Put configuration (paths, thresholds, names) at the TOP of the script, as variables — never buried in the middle of logic.** If someone needs to change the backup folder location or the disk-usage alert threshold, they should be able to do it by editing ONE line near the top, not hunting through 50 lines of logic.

5. **Add a one-line comment above anything non-obvious — especially "why," not "what."** A comment like `# retry logic because the API takes ~10s to wake up after deploy` is genuinely useful. A comment like `# print message` above `echo "message"` adds nothing.

6. **Test file/directory existence BEFORE acting on them, not after.** Checking `[ -f "$CONFIG_FILE" ]` before trying to read it produces a clean, understandable error message ("config file missing") instead of a confusing crash deep inside your script's logic.

7. **Prefer functions over copy-pasting the same 5 lines in multiple places.** If you find yourself writing near-identical blocks of code more than twice, that's a strong signal it should become a function instead — future changes then only need to happen in ONE place.

8. **Always give scripts meaningful exit codes, and check them where it matters.** Don't just let a script silently "work most of the time" — explicit `exit 0` (success) and `exit 1` (failure) at key points make it possible for OTHER scripts, or CI/CD pipelines, to correctly detect whether your script actually succeeded.

9. **Log important actions to a file, not just the screen, for anything that runs unattended (via cron).** If a backup script runs at 2 AM with nobody watching, and it fails, a log file (`>> /var/log/mybackup.log`) is the ONLY way you'll ever find out what happened.

10. **Before writing a complex one-off command, ask: "will I need to run this again?"** If yes — even "probably" — it belongs in a saved `.sh` script file, not just typed once into the terminal and forgotten. That's the entire point of shell scripting: turning "I did this once" into "this always works, reliably, whenever needed."

---

*Master Cheat Sheet — Day 16 to Day 21 of #90DaysOfDevOps — TrainWithShubham*
