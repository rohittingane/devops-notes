# Git Commands — Complete Reference Table

A quick-reference table of every Git command, organized by category.

---

## 1. Setup & Configuration

| Command | Explanation |
|---|---|
| `git --version` | Checks whether Git is installed and shows the installed version |
| `git config --global user.name "Name"` | Sets the name attached as author to every commit (applies to all repos on this machine) |
| `git config --global user.email "email"` | Sets the email linked to every commit — should match your GitHub email |
| `git config --list` | Displays all current Git configuration settings, to verify setup |

---

## 2. Repository Management

| Command | Explanation |
|---|---|
| `mkdir <folder-name>` | Creates a new empty folder |
| `cd <folder-name>` | Moves into that folder |
| `git init` | Converts a normal folder into a Git repository by creating a hidden `.git` directory |
| `git status` | Shows current state — modified, staged, or untracked files |
| `git clone <url>` | Downloads a full copy of an existing repository onto your machine |

---

## 3. File Management

| Command | Explanation |
|---|---|
| `touch <filename>` | Creates a new, empty file |
| `ls -la` | Lists all files/folders including hidden ones (like `.git`), with details |
| `ls -la .git` | Explores Git's internal folder structure (HEAD, config, objects, refs) |
| `pwd` | Prints the full path of the current working directory |

---

## 4. Core Workflow — Add, Commit, Log

| Command | Explanation |
|---|---|
| `git add <filename>` | Moves a file's changes into the staging area, marking it ready to commit |
| `git add .` | Stages all changed files in the current folder and subfolders at once |
| `git commit -m "message"` | Permanently saves staged changes as a new commit, with a description |
| `git log` | Shows full commit history — hash, author, date, message |
| `git log --oneline` | Shows a compact, single-line version of commit history |
| `git diff` | Shows exact line-by-line changes not yet staged |
| `git diff --staged` | Shows exact line-by-line changes that ARE staged, not yet committed |

---

## 5. Branching

| Command | Explanation |
|---|---|
| `git branch` | Lists all branches; `*` marks the one you're currently on |
| `git branch <name>` | Creates a new branch (doesn't switch to it) |
| `git switch <name>` | Switches to an existing branch |
| `git switch -c <name>` | Creates a new branch AND switches to it in one step |
| `git checkout <name>` | Older command to switch branches (also restores files) |
| `git branch -d <name>` | Deletes a branch (only if fully merged) |
| `git branch -D <name>` | Force-deletes a branch, even if not merged |
| `git branch -m <old> <new>` | Renames a branch |

---

## 6. Remote Repositories (GitHub)

| Command | Explanation |
|---|---|
| `git remote add origin <url>` | Connects local repo to a GitHub repository, nicknamed `origin` |
| `git remote -v` | Lists all connected remotes and their URLs |
| `git push -u origin <branch>` | Uploads local commits to GitHub; `-u` sets this as the default upstream |
| `git push` | Uploads commits (after `-u` has been set once) |
| `git pull origin <branch>` | Downloads and automatically merges latest changes from GitHub |
| `git fetch origin` | Downloads latest changes from GitHub WITHOUT merging them yet |
| `git remote add upstream <url>` | Connects to the original repo you forked from |

---

## 7. Merge, Rebase & Squash

| Command | Explanation |
|---|---|
| `git merge <branch>` | Combines another branch's changes into the current branch |
| `git merge --no-ff <branch>` | Forces a merge commit even if a fast-forward is possible |
| `git rebase <branch>` | Replays current branch's commits on top of another branch, for clean linear history |
| `git rebase -i HEAD~<n>` | Interactive rebase — lets you squash/edit/reorder the last `n` commits |
| `git rebase --abort` | Cancels an in-progress rebase and returns to the state before it started |
| `git rebase --continue` | Continues a rebase after manually resolving a conflict |

---

## 8. Stash

| Command | Explanation |
|---|---|
| `git stash` | Temporarily saves uncommitted changes, giving a clean working directory |
| `git stash list` | Shows all currently stashed items |
| `git stash apply` | Brings back the most recent stash, but keeps a copy in the list |
| `git stash pop` | Brings back the most recent stash AND removes it from the list |
| `git stash drop` | Deletes a specific stash without applying it |
| `git stash clear` | Deletes all stashes |

---

## 9. Cherry-Pick

| Command | Explanation |
|---|---|
| `git cherry-pick <commit-id>` | Applies one specific commit from another branch onto the current branch |
| `git cherry-pick --abort` | Cancels a cherry-pick that hit a conflict |
| `git cherry-pick --continue` | Continues a cherry-pick after resolving a conflict manually |

---

## 10. Undoing Changes — Reset & Revert

| Command | Explanation |
|---|---|
| `git reset --soft HEAD~1` | Removes the last commit, keeps changes staged |
| `git reset --mixed HEAD~1` | Removes the last commit, keeps changes unstaged (this is the default mode) |
| `git reset --hard HEAD~1` | Removes the last commit AND deletes the changes completely (destructive) |
| `git revert <commit-id>` | Creates a new commit that undoes an earlier commit's changes, safely, without rewriting history |
| `git restore <filename>` | Discards uncommitted changes in a file, reverting it to the last commit |
| `git restore --staged <filename>` | Unstages a file (removes it from staging area, keeps the actual edits) |
| `git reflog` | Shows a log of where HEAD has pointed recently — can help recover "lost" commits |

---

## 11. Inspecting & Comparing

| Command | Explanation |
|---|---|
| `git show <commit-id>` | Shows full details and changes of one specific commit |
| `git diff <branch1> <branch2>` | Shows differences between two branches |
| `git blame <filename>` | Shows who last modified each line of a file, and in which commit |

---

## 12. GitHub CLI (`gh`)

| Command | Explanation |
|---|---|
| `gh --version` | Checks if GitHub CLI is installed |
| `gh auth login` | Logs into your GitHub account from the terminal |
| `gh auth status` | Confirms you're logged in |
| `gh repo create <name> --public --add-readme --clone` | Creates a new GitHub repo, adds a README, and clones it locally |
| `gh repo view` | Shows details of the current repository |
| `gh repo list` | Lists your repositories |
| `gh repo delete <name> --yes` | Deletes a repository |
| `gh issue create --title "..." --body "..."` | Creates a new issue |
| `gh issue list` | Lists issues in a repository |
| `gh issue view <number>` | Views details of a specific issue |
| `gh issue close <number>` | Closes an issue |
| `gh pr create --title "..." --body "..."` | Creates a pull request |
| `gh pr list` | Lists pull requests |
| `gh pr view <number>` | Views details of a specific PR |
| `gh pr merge <number> --merge/--squash/--rebase` | Merges a PR using the specified method |
| `gh run list` | Lists recent GitHub Actions workflow runs |
| `gh run view <run-id>` | Views details of a specific workflow run |
| `gh workflow list` | Lists all configured GitHub Actions workflows |

---

## Quick Workflow Reference (typical day-to-day flow)

```
git status                          → check what's changed
git add <file>                      → stage changes
git commit -m "message"             → save a snapshot
git push                            → upload to GitHub
git pull                            → download latest changes
git branch <name> / git switch <name>   → work on a new feature safely
git merge <branch>                  → bring feature back into main
```
