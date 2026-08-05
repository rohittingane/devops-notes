# Shell Scripting Commands — Complete Reference Table (Day 16–21)

A quick-reference table of every Shell Scripting concept practiced, organized part-wise.

---

## 1. Basics — Shebang, Comments, Running Scripts

| Syntax | Explanation |
|---|---|
| `#!/bin/bash` | First line of every script — tells the OS which interpreter to use |
| `chmod +x script.sh` | Makes a script file executable |
| `./script.sh` | Runs the script (needs execute permission) |
| `bash script.sh` | Runs the script explicitly with bash (no execute permission needed) |
| `# comment text` | A comment — ignored when the script runs, explains code to humans |

---

## 2. Variables

| Syntax | Explanation |
|---|---|
| `NAME="value"` | Creates a variable — NO spaces allowed around `=` |
| `echo $NAME` | Prints a variable's value (unquoted — risky with spaces) |
| `echo "$NAME"` | Prints a variable's value (quoted — safest, always preserves it exactly) |
| `echo '$NAME'` | Prints the literal text `$NAME` — single quotes block variable expansion |
| `read -p "prompt: " VAR` | Asks the user for input and stores it in `VAR` |

---

## 3. Command-Line Arguments

| Variable | Explanation |
|---|---|
| `$0` | The script's own name/path |
| `$1`, `$2`, ... | The 1st, 2nd, etc. argument passed to the script |
| `$#` | Total number of arguments passed |
| `$@` | All arguments, as a full list |
| `$?` | Exit code of the last command that ran (0 = success) |

---

## 4. If / Elif / Else (Conditionals)

| Syntax | Explanation |
|---|---|
| `if [ condition ]; then ... fi` | Runs code only if the condition is true |
| `if ... elif [ cond2 ]; then ... fi` | Adds an additional condition to check if the first was false |
| `if ... else ... fi` | Runs code if NONE of the above conditions were true |
| `[ "$a" = "$b" ]` | String: equal |
| `[ "$a" != "$b" ]` | String: not equal |
| `[ -z "$a" ]` | String: true if empty |
| `[ -n "$a" ]` | String: true if NOT empty |
| `[ "$a" -eq "$b" ]` | Number: equal |
| `[ "$a" -ne "$b" ]` | Number: not equal |
| `[ "$a" -lt "$b" ]` | Number: less than |
| `[ "$a" -gt "$b" ]` | Number: greater than |
| `[ "$a" -le "$b" ]` | Number: less than or equal |
| `[ "$a" -ge "$b" ]` | Number: greater than or equal |
| `[ -f file ]` | True if a regular file exists |
| `[ -d dir ]` | True if a directory exists |
| `[ -e path ]` | True if the path exists (file or directory) |
| `[ -r file ]` | True if readable |
| `[ -w file ]` | True if writable |
| `[ -x file ]` | True if executable |
| `[ -s file ]` | True if file exists and is not empty |
| `cond1 && cond2` | AND — runs the right side only if left side is true |
| `cond1 \|\| cond2` | OR — runs the right side only if left side is false |
| `! condition` | NOT — flips true/false |

---

## 5. Case Statements

| Syntax | Explanation |
|---|---|
| `case "$VAR" in` | Starts a case block, checking the value of `$VAR` |
| `pattern) commands ;;` | If `$VAR` matches `pattern`, run these commands |
| `*) commands ;;` | Catch-all — matches anything not covered above |
| `esac` | Ends the case block |

---

## 6. Loops

| Syntax | Explanation |
|---|---|
| `for x in list; do ... done` | Runs once for each item in the list |
| `for ((i=0; i<5; i++)); do ... done` | C-style counting loop |
| `while [ condition ]; do ... done` | Repeats WHILE the condition stays true |
| `until [ condition ]; do ... done` | Repeats UNTIL the condition becomes true |
| `((count++))` | Increases a variable's value by 1 |
| `continue` | Skips the rest of the current loop iteration, moves to next |
| `break` | Stops the loop entirely, immediately |
| `for file in *.log; do ... done` | Loops over every file matching a pattern |
| `while read line; do ... done < file.txt` | Loops over a file, one line at a time |

---

## 7. Functions

| Syntax | Explanation |
|---|---|
| `myfunc() { ... }` | Defines a function named `myfunc` |
| `myfunc arg1 arg2` | Calls the function, passing arguments |
| `$1` (inside a function) | The function's own first argument (separate from the script's `$1`) |
| `return 0` | Returns a success status code from a function |
| `return 1` | Returns a failure status code from a function |
| `echo "value"` (inside a function) | The way to actually return DATA from a function (not just a status code) |
| `result=$(myfunc)` | Captures a function's echoed output into a variable |
| `local varname="value"` | Creates a variable that's private to the function only |

---

## 8. Text Processing Commands

| Command | Explanation |
|---|---|
| `grep "pattern" file` | Search for lines matching a pattern |
| `grep -i "pattern" file` | Case-insensitive search |
| `grep -r "pattern" ./folder` | Recursive search through all files in a folder |
| `grep -c "pattern" file` | Count matching lines |
| `grep -n "pattern" file` | Show line numbers with matches |
| `grep -v "pattern" file` | Invert match — show non-matching lines |
| `awk '{print $1}' file` | Print the first column of each line |
| `awk -F: '{print $1}' file` | Use `:` as the column separator |
| `awk '$3 > 50 {print $1}' file` | Print column 1 only where column 3 exceeds 50 |
| `sed 's/old/new/' file` | Replace first match of "old" with "new" per line |
| `sed 's/old/new/g' file` | Replace ALL matches per line |
| `sed -i 's/old/new/g' file` | Edit the file in-place, saving the change |
| `cut -d: -f1 file` | Extract field 1, using `:` as delimiter |
| `sort file` | Sort lines alphabetically |
| `sort -n file` | Sort lines numerically |
| `sort -r file` | Sort in reverse order |
| `sort -u file` | Sort and remove duplicates |
| `uniq` | Remove duplicate ADJACENT lines (use after `sort`) |
| `uniq -c` | Count occurrences of each line |
| `tr 'a-z' 'A-Z'` | Translate lowercase to uppercase |
| `tr -d 'x'` | Delete a specific character |
| `wc -l file` | Count lines |
| `wc -w file` | Count words |
| `wc -c file` | Count characters/bytes |
| `head -n 10 file` | Show first 10 lines |
| `tail -n 10 file` | Show last 10 lines |
| `tail -f file` | Follow a file live, showing new lines as they arrive |
| `command1 \| command2` | Pipe — feed command1's output as command2's input |

---

## 9. Error Handling & Debugging

| Syntax | Explanation |
|---|---|
| `echo $?` | Shows the exit code of the last command (0 = success) |
| `exit 0` | Ends the script successfully |
| `exit 1` | Ends the script indicating a failure |
| `set -e` | Script stops immediately if ANY command fails |
| `set -u` | Using an undefined variable throws an error, instead of silently being blank |
| `set -o pipefail` | A pipeline fails if ANY command in it fails, not just the last one |
| `set -euo pipefail` | The recommended combined "safety header" for scripts |
| `set -x` | Debug mode — prints each command before running it |
| `set +x` | Turns debug mode off |
| `trap cleanup EXIT` | Runs the `cleanup` function automatically whenever the script exits |

---

## 10. Useful Date & File Commands (used inside real scripts)

| Command | Explanation |
|---|---|
| `date +%F` | Prints today's date (e.g., `2026-08-06`) |
| `date +%F_%H-%M-%S` | Prints a full timestamp, safe for filenames |
| `tar -czf backup.tar.gz /path` | Compresses a folder into a `.tar.gz` archive |
| `find /path -type f -mtime +7` | Finds files older than 7 days |
| `find /path -type f -mtime +7 -delete` | Finds AND deletes files older than 7 days |
| `xargs` | Takes piped input and uses it as arguments to another command |
| `systemctl is-active --quiet <service>` | Silently checks if a service is running (true/false result) |

---

## 11. Scheduling (Cron)

| Command | Explanation |
|---|---|
| `crontab -e` | Opens your personal schedule file for editing |
| `0 2 * * * /path/script.sh` | Cron syntax: run `script.sh` at 2:00 AM every day |
| `crontab -l` | Lists your currently scheduled cron jobs |

---

## Quick Workflow Reference (typical script structure)

```bash
#!/bin/bash
set -euo pipefail              # safety header

# 1. Configuration variables at the top
CONFIG_VAR="value"

# 2. Functions defined
my_function() {
    local temp="$1"
    echo "$temp"
}

# 3. Main logic — conditionals & loops
if [ -f "somefile" ]; then
    for item in list; do
        echo "$item"
    done
fi

# 4. Cleanup guaranteed via trap
trap 'echo "cleaning up"' EXIT
```
