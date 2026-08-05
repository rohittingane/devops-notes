# 🌿 Master Git Cheat Sheet (Day 22 – Day 27)

**A complete, beginner-friendly reference.** Every command explained word-by-word — what it is, why we use it, and how to use it — so that even someone who has never touched Git before can follow along.

---

# 📖 Part 0: The Absolute Basics — What is Git, in Plain Words?

### What problem does Git solve?

Imagine you're writing an important document. You keep saving new versions: `document_final.docx`, `document_final2.docx`, `document_FINAL_ACTUALLY.docx`. It gets messy fast, and you can never be 100% sure which version is the real latest one, or go back cleanly to an older one.

**Git solves exactly this problem for code** — but in a much smarter way. Instead of creating messy copies, Git remembers every single change you ever make, with a timestamp, an author, and a message describing what changed. You can jump back to any point in time instantly.

### Git = a "Save Game" system for your code

Think of Git like a video game's save system:
```
Working Code (playing the game)
     ↓
   Git (save points)
     ↓
Commit 1 → Commit 2 → Commit 3 (save slots)
     ↓
Complete History (all your progress, always recoverable)
```
If something breaks after Commit 3, you can reload Commit 2 instantly — nothing is lost.

### Why do teams NEED Git (not just "nice to have")?

| Without Git | With Git |
|---|---|
| Accidentally delete code = gone forever | Restore any previous version in seconds |
| No record of who changed what, or why | Every commit stores author, email, exact time, and a message |
| Two people editing the same file = chaos, overwritten work | Clean, trackable collaboration; conflicts are shown and resolved safely |

**Real scenario:** A developer adds a new feature, but it accidentally breaks the live website. With Git:
```
Developer writes Version 1 (working)
        ↓
   Pushes to Git
        ↓
Adds new feature → Bug introduced → Website breaks
        ↓
   git checkout <previous_working_commit>
        ↓
   Website working again ✅ (in seconds, not hours)
```

### Git vs GitHub — a common confusion

- **Git** = the actual tool/software that tracks changes, running on your own computer.
- **GitHub** = a website that stores a copy of your Git project online, so others can see it, collaborate, and you have a backup outside your own machine.

**Simple analogy:** Git is like Microsoft Word (the tool doing the work locally). GitHub is like Google Drive (where you upload/share a copy of your document online).

---

# 🛠️ Part 1: Installing & Setting Up Git

### `git --version`
**Meaning:** Asks Git to tell you which version is installed.
**Why:** Confirms Git is actually installed on your machine before doing anything else.
```
Output: git version 2.43.0
```

### `git config --global user.name "Your Name"`
Breaking this down word by word:
- `git config` → change a Git **setting**
- `--global` → apply this setting to **every** Git project on this computer (not just one folder)
- `user.name` → the specific setting being changed — your **display name**
- `"Your Name"` → the actual value you're setting it to

**Why this matters:** Every single commit you ever make will be stamped with this name, so people (and you, later) know who made each change.
```bash
git config --global user.name "Rohit Tingane"
```

### `git config --global user.email "your-email@example.com"`
Same idea, but sets the **email** attached to your commits. This should match the email on your GitHub account, so GitHub can correctly link your commits to your profile.

### `git config --list`
**Meaning:** "Show me every Git setting currently configured."
**Why:** A quick sanity check — DevOps golden rule: *"Trust, but verify."* Always confirm your setup actually saved correctly.

---

# 📁 Part 2: Creating Your First Repository

### What is a "repository" (repo)?
A repository is simply a project folder that Git is actively "watching" — remembering every change, storing history, and managing branches inside it.
```
Repository = A normal project folder + Git's memory attached to it
```

### `mkdir <folder-name>`
- `mkdir` = **m**a**k**e **dir**ectory — creates a brand new, empty folder.
```bash
mkdir devops-git-practice
```

### `cd <folder-name>`
- `cd` = **c**hange **d**irectory — moves you INTO that folder in the terminal.
```bash
cd devops-git-practice
```

### `git init`
**Meaning:** "Turn this ordinary folder into a Git repository."
**What happens internally:** Git creates a hidden `.git` folder inside, which is where ALL the tracking magic (history, branches, everything) actually gets stored.

**Real-world comparison:**
```
payment-service/   (no Git)   → changes untracked, no accountability, risky
payment-service/   (with Git) → every single change is tracked and recoverable
```

### `ls -la`
- `ls` = **l**i**s**t files/folders in the current location
- `-l` = show details (size, permissions, date)
- `-a` = show **all**, including hidden files (like the `.git` folder, which starts with a dot)

**Why use `-la` here specifically:** To confirm the hidden `.git` folder was actually created after `git init`.

### `git status`
**Meaning:** "Tell me the current state of my repository — what's changed, what's staged, what's untracked."
This is probably the command you'll run more than any other in Git — always safe, never changes anything, just reports information.

---

# 🔄 Part 3: The Core Git Workflow — Add, Commit, Log

### The 3 stages every file goes through

```
Working Directory  →  Staging Area  →  Repository (committed history)
   (edit files)         (git add)         (git commit)
```

| Stage | What it means, simply | Command to move here |
|---|---|---|
| **Working Directory** | Your files as they currently sit on your computer, being edited | (just editing normally) |
| **Staging Area** | A "waiting room" — files you've marked as "yes, include this in my next save point" | `git add <file>` |
| **Repository** | The permanent, saved history — a real commit has been made | `git commit -m "message"` |

**Why does Git even have a "staging area" — why not just save everything directly?**
It gives you control. Maybe you edited 5 files, but only 3 of them are actually ready to be saved together as one logical change. Staging lets you pick exactly which changes go into which commit — like choosing exactly which items go into one shopping bag, even if you have more items lying around.

### `touch <filename>`
Creates a new, empty file instantly.
```bash
touch git-commands.md
```

### `git add <filename>`
**Meaning:** "Move this file's current changes into the staging area — mark it ready to be saved."
```bash
git add git-commands.md
```
You can also add EVERYTHING at once:
```bash
git add .
```
The `.` means "current folder and everything inside it."

### `git commit -m "message"`
**Meaning:** "Permanently save everything currently in the staging area, as one snapshot, with this description attached."
- `-m` = **m**essage flag — lets you write the commit message directly in the same line, instead of opening a separate text editor
```bash
git commit -m "Added Git commands documentation"
```

**Why the message matters so much:** Six months from now, when you (or a teammate) look at the history, a clear message like `"Fixed login button not responding on mobile"` is infinitely more useful than a vague one like `"updated stuff"`.

### `git log`
**Meaning:** "Show me the complete history of commits made so far — who, when, and what message."

### `git log --oneline`
Same thing, but compressed to a **single line per commit** (just a short ID + the message) — much easier to scan quickly when history gets long.

---

# 🌿 Part 4: Branching — Working Without Breaking Things

### What is a branch, really?

A branch is an **independent, parallel timeline** of your project. It lets you experiment, build a new feature, or fix a bug — completely separately from your main, stable code — without risking breaking it.

**Simple analogy:** Imagine you're writing a book. Your `main` branch is the published, finished chapters. A `feature` branch is like a separate notebook where you draft a risky new chapter idea — if it doesn't work out, you just throw that notebook away; the published book was never touched.

```
main
 ├── Stable, working code
 └── feature-login  (a separate timeline)
      └── Login feature being built here — main stays untouched
```

### `git branch`
**Meaning:** "List all branches that currently exist in this repository."
The branch with a `*` next to it is the one you're currently "standing on."
```
Output:
* feature-1
  main
```

### `git branch <name>`
Creates a brand-new branch with that name — but does **NOT** move you onto it yet, you stay where you were.
```bash
git branch feature-1
```

### `git switch <name>`
**Meaning:** "Move me onto this existing branch."
```bash
git switch feature-1
```

### `git switch -c <name>`
The `-c` flag = **c**reate. This does TWO things in one command: creates a new branch AND switches to it immediately.
```bash
git switch -c feature-2
```

### `git switch` vs `git checkout` — what's the difference?

`git checkout` is the older, do-everything command (switches branches, restores files, and more — sometimes confusingly multi-purpose). `git switch` is the newer command created specifically JUST for switching branches — simpler and safer, less room for accidental mistakes.

```bash
git switch master      # newer, recommended way
git checkout master    # older way, still works
```

### What is `HEAD`?

`HEAD` is just a pointer that tells Git (and you) "which branch/commit am I currently standing on, right now." When you switch branches, `HEAD` moves to point at the new one.

### What happens to your files when you switch branches?

Git automatically updates the files in your folder to match whatever exists on the branch you switched TO. Files that only exist on a *different* branch will temporarily disappear from view (they're not deleted — they're just not part of the branch you're currently looking at).

**Example proving this:**
```bash
git switch feature-1
echo "Feature work" > feature.txt
git add .
git commit -m "Add feature.txt"
git switch main
ls
```
After switching back to `main`, `feature.txt` won't be visible — because it only exists inside the `feature-1` branch's history.

### `git branch -d <name>`
`-d` = **d**elete. Removes a branch you no longer need (Git will refuse if it has unmerged work, as a safety check).
```bash
git branch -d feature-2
```

---

# ☁️ Part 5: Pushing, Pulling & Working with GitHub

### `git remote add origin <repository-url>`
**Meaning:** "Connect my local repository to this online GitHub repository, and nickname that connection `origin`."

**Why `origin`?** It's just a conventional nickname — like saving a phone contact as "Mom" instead of typing the full phone number every time. `origin` almost always refers to YOUR main remote copy on GitHub.

### `git push -u origin <branch-name>`
**Meaning:** "Upload my local commits on this branch, up to GitHub."
- `-u` = sets this as the **default** upstream connection for this branch, so next time you can just type `git push` without specifying details again.
```bash
git push -u origin master
```

### `origin` vs `upstream` — a common source of confusion

| Term | Meaning |
|---|---|
| `origin` | The remote repository YOU push to and pull from — usually your own GitHub copy |
| `upstream` | The ORIGINAL repository you forked FROM — used to pull in updates made by the original project |

```
origin   → your GitHub repository (you have write access)
upstream → the original project's repository (you're just following along)
```

### `git pull origin <branch>`
**Meaning:** "Download the latest changes from GitHub, and automatically merge them into my current local branch."
```bash
git pull origin master
```

### `git fetch` vs `git pull` — the key difference

- **`git fetch`** → downloads the latest changes from GitHub, but does NOT touch your current files yet. It just shows you "here's what's new," letting you review before deciding to apply it.
- **`git pull`** → downloads AND immediately merges those changes into your current branch, in one step. It's literally `fetch` + `merge` combined.

**When to use which:** Use `fetch` when you want to be cautious and review incoming changes first. Use `pull` for quick, routine updates when you trust there won't be surprises.

### Clone vs Fork — another common confusion

**`git clone <url>`**
Downloads a full copy of an existing repository onto your own computer, so you can work on it locally.
```bash
git clone https://github.com/someone/project.git
```
Use clone when: it's your own repository, or you already have permission/access to make changes directly.

**Fork** (done on GitHub's website, not a terminal command)
Creates YOUR OWN personal copy of someone else's repository, under your own GitHub account. You can freely experiment and make changes there without touching the original project at all.

Use fork when: you want to contribute to an open-source project you don't own or have write access to.

### Keeping a fork updated with the original project

```bash
git remote add upstream <original-repo-url>   # connect to the original project
git fetch upstream                             # download its latest changes
git merge upstream/master                      # merge those changes into your fork
```

**Remote structure after this setup:**
```
origin   → your forked copy on GitHub
upstream → the original project you forked from
```

---

# 🔀 Part 6: Advanced Git — Merge, Rebase, Squash, Stash, Cherry-Pick

## 6.1 Merge — Combining Two Branches

**What merge does, simply:** Takes the changes from one branch and combines them into another (usually back into `main`).

### Type 1: Fast-Forward Merge
Happens when NOTHING new has been added to `main` while you were working on your separate branch. Git simply moves the `main` pointer forward to include your new commits — no extra "joining" commit needed, history stays a clean straight line.

### Type 2: Merge Commit
Happens when `main` DID get new changes while you were working separately. Git can't just move forward this time — it has to combine two different histories, so it creates a special commit with two parents, joining both branches together.

### Type 3: Merge Conflict
Happens when two branches changed the **exact same line** of the **exact same file**, differently. Git can't automatically decide which version is correct, so it stops and asks YOU (a human) to manually pick/fix the correct content.

**How to actually resolve a conflict, step by step:**
1. Git marks the conflicting section in the file with special markers:
   ```
   <<<<<<< HEAD
   your version of the line
   =======
   their version of the line
   >>>>>>> feature-branch
   ```
2. You manually edit the file, deciding what the final correct content should be, and delete those marker lines completely.
3. Save the file, then:
   ```bash
   git add <filename>
   git commit
   ```
   This tells Git "conflict resolved, here's the final version" and completes the merge.

```bash
git merge feature-branch
```

---

## 6.2 Rebase — Rewriting History to Look Clean

**What rebase does, simply:** Instead of creating a "joining" merge commit, rebase takes YOUR commits, temporarily sets them aside, and replays them one-by-one directly on top of the latest version of the target branch — as if you had started your work later than you actually did.

**Result:** A clean, completely straight-line history, with zero visible branching or merge commits.

```bash
git rebase main
```

### ⚠️ The Golden Rule of Rebase
**NEVER rebase commits that have already been pushed and shared with others.** Rebase changes each commit's unique ID (hash). If teammates already downloaded the original commits, and you rebase + push new ones, everyone's history now mismatches — causing confusing duplicate commits and conflicts for the whole team.

**When to use rebase vs merge:**
- **Rebase** → on your OWN private branch, before you've shared it with anyone, to keep history clean.
- **Merge** → once a branch is already shared/pushed, or on important shared/production branches where you want to preserve the true, honest record of what actually happened.

---

## 6.3 Squash Merge — Combining Many Small Commits Into One

**What it does:** If you made many tiny commits (like "fixed typo", "fixed typo again", "oops, one more fix"), squash merge bundles ALL of them into just **one single, clean commit** when merging into main.

**Trade-off:** You get a much cleaner, easier-to-read history — but you lose the fine-grained, step-by-step detail of exactly what changed and when, which can make debugging harder later (you can't tell exactly which of the 5 tiny changes introduced a bug, since they're all now one commit).

**When to use it:** Very common in real companies for Pull Requests — keeps the main project's history simple and readable, since nobody outside your branch cares about your 15 messy work-in-progress commits, just the final clean result.

---

## 6.4 Stash — Temporarily Setting Work Aside

**The problem it solves:** You're in the middle of editing something (not ready to commit yet), but suddenly need to switch to a different branch (e.g., an urgent bug fix request). Git normally won't let you switch branches cleanly if you have unsaved changes that would conflict.

**What stash does:** Temporarily "puts your unfinished work into a box," giving you a clean working directory to switch branches freely. Later, you can pull that work back out exactly where you left off.

```bash
git stash              # save my current unfinished changes into a temporary box
git switch other-branch # now free to switch branches safely
# ... do other urgent work here ...
git switch original-branch
git stash pop           # bring my saved changes back, AND remove them from the box
```

### `git stash apply` vs `git stash pop` — the key difference
- `git stash apply` → brings your saved changes back, but **keeps a copy** in the stash list (in case you need to apply it again elsewhere).
- `git stash pop` → brings your saved changes back **and deletes** that copy from the stash list — meant for one-time use.

```bash
git stash list    # see everything currently stashed
```

**Real-world scenario:** You're halfway through building a feature, your manager suddenly says "drop everything, there's a critical bug in production!" You run `git stash`, switch to fix the bug, push the fix, then switch back and run `git stash pop` to continue exactly where you left off — nothing lost, no messy half-finished commit needed.

---

## 6.5 Cherry-Pick — Grabbing One Specific Commit

**What it does:** Lets you copy just ONE specific commit from a branch, and apply only that change onto a different branch — without bringing along any of the other commits on that original branch.

```bash
git cherry-pick <commit-id>
```

**Real-world scenario:** A critical bug fix already exists on a development branch that isn't fully ready to be merged yet (it has other unfinished, risky features too). Instead of waiting for the whole feature to be done, you can cherry-pick just that ONE bug-fix commit straight onto the production/main branch immediately.

**Risks to know:**
- If the picked commit depends on other changes that were left behind on the original branch, things can break.
- The same logical change can end up with different commit IDs on different branches, making history a bit harder to trace over time.

---

# ⏪ Part 7: Undoing Mistakes — Reset vs Revert

Everyone makes commit mistakes. Git gives you TWO different, very different tools to undo them, depending on the situation.

## 7.1 `git reset` — Rewriting History (use only on YOUR OWN, not-yet-shared commits)

**What it does, simply:** Moves your branch pointer backward to an earlier commit, essentially making Git "forget" the commits after that point ever happened (from the branch's perspective).

There are 3 modes, and they differ in what happens to your ACTUAL file changes:

### `git reset --soft HEAD~1`
- `HEAD~1` means "one commit before where I currently am"
- **Commit removed** from history
- **Changes KEPT in the staging area** (ready to re-commit immediately, maybe with a better message)

**Use case:** You want to combine the last commit with new changes, or just fix its commit message.

### `git reset --mixed HEAD~1` (this is actually the DEFAULT mode if you don't specify one)
- **Commit removed** from history
- **Changes KEPT, but moved back to your working directory** (unstaged — you'd need to `git add` again before committing)

**Use case:** You want to remove a commit but keep the file changes available for further editing before re-committing.

### `git reset --hard HEAD~1`
- **Commit removed** from history
- **Changes are COMPLETELY DELETED** — gone, not staged, not in your working files, nowhere
- ⚠️ **This is destructive and cannot be undone easily.** Only use this if you're 100% sure you want to throw those changes away completely.

**Summary table:**

| Command | What happens to the commit? | What happens to the file changes? |
|---|---|---|
| `git reset --soft` | Removed | Kept — staged, ready to re-commit |
| `git reset --mixed` | Removed | Kept — but unstaged |
| `git reset --hard` | Removed | **Deleted completely** |

## 7.2 `git revert <commit-id>` — Safely Undoing on Shared Branches

**What it does, simply:** Instead of erasing history (like reset does), revert creates a **brand-new commit** that does the exact opposite of an earlier commit — effectively cancelling out its changes, while keeping the original commit visible in history too.

```bash
git revert e1d44fe
```
**Result in history:**
```
Revert "Commit Y"      ← new commit, undoing Y's changes
Commit Z
Commit Y               ← still visible, untouched
Commit X
```

## Reset vs Revert — when to use which

| | `git reset` | `git revert` |
|---|---|---|
| What it does | Moves the branch pointer back, rewriting history | Adds a NEW commit that cancels out an old one |
| Does it remove the original commit from history? | Yes | No — it stays, fully visible |
| Safe to use on a branch others have already pulled/shared? | **No** — rewrites shared history, causes chaos for teammates | **Yes** — perfectly safe, since it only adds new history |
| Best used for | Your own local, not-yet-pushed mistakes | Undoing something on a shared/pushed/production branch |

**Simple rule of thumb:** If nobody else has seen these commits yet, `reset` is fine. If it's already been pushed and others might have it, always use `revert` instead.

---

# 🏗️ Part 8: Branching Strategies — How Real Teams Organize Their Work

Different companies/teams use different overall strategies for how branches are structured and merged. Here are the 3 most common:

## 1. GitFlow
```
main
 |
develop
 |
feature branches
 |
release
 |
hotfix
```
**Used for:** Large teams, enterprise software, projects with scheduled release dates.
**Pros:** Very structured, clear separation between "in development" and "live/production" code.
**Cons:** More complex, slower to move fast.

## 2. GitHub Flow
```
main
 |
feature branch
 |
Pull Request (for review)
 |
Merge to main
```
**Used for:** Startups, fast-moving projects, continuous deployment setups.
**Pros:** Simple, fast, easy for small teams to follow.
**Cons:** Requires strong automated testing (since things go live quickly, mistakes reach production faster too).

## 3. Trunk-Based Development
```
main
 |
short-lived branches (often lasting less than a day)
 |
main (merged back quickly)
```
**Used for:** DevOps-mature teams with strong CI/CD automation.
**Pros:** Very fast integration, fewer messy merge conflicts (since branches don't live long enough to diverge much).
**Cons:** Demands excellent automated testing and disciplined small commits — risky without that safety net.

**Simple decision guide:**
- Small startup shipping fast → **GitHub Flow**
- Large team, planned release cycles → **GitFlow**
- Mature DevOps team, strong CI/CD pipeline → **Trunk-Based Development**

---

# 🖥️ Part 9: GitHub CLI (`gh`) — Managing GitHub from the Terminal

### What is GitHub CLI, and why use it?
Normally, to create repositories, issues, or pull requests, you'd open GitHub's website and click through menus. GitHub CLI (`gh`) lets you do ALL of this directly from your terminal — much faster, and easy to include inside automated scripts.

### Installing & logging in

```bash
sudo apt update
sudo apt install gh -y
gh --version              # confirm it installed
gh auth login             # log into your GitHub account
gh auth status            # confirm you're logged in correctly
```

### Creating a repository from the terminal

```bash
gh repo create gh-cli-demo \
  --public \
  --description "GitHub CLI practice" \
  --add-readme \
  --clone
```
| Flag | Meaning |
|---|---|
| `--public` | make the new repo publicly visible |
| `--description "..."` | set the one-line repo description |
| `--add-readme` | automatically create a starter README file |
| `--clone` | also immediately download a local copy onto your computer |

### Managing issues from the terminal

An "issue" on GitHub is basically a task/bug/todo tracked against a repository.
```bash
gh issue create --repo user/repo --title "Bug title" --body "Description here"
gh issue list --repo user/repo
gh issue view 1 --repo user/repo
gh issue close 1 --repo user/repo
```
**Real use case:** Automation scripts can automatically CREATE an issue whenever a CI/CD pipeline fails — no human needs to manually open GitHub and type it up.

### Managing Pull Requests (PRs) from the terminal

A Pull Request is a formal request to merge your branch's changes into another branch (usually `main`), often reviewed by teammates first.
```bash
gh pr create --repo user/repo --title "My PR" --body "What this changes"
gh pr list --repo user/repo
gh pr view 2 --repo user/repo
gh pr merge 2 --repo user/repo --merge
```

**3 ways to merge a PR via `gh`:**
```bash
--merge     # creates a normal merge commit
--squash    # combines all commits in the PR into one clean commit
--rebase    # replays the PR's commits cleanly on top of the target branch
```

### Monitoring GitHub Actions (CI/CD pipelines) from the terminal

```bash
gh run list --repo user/repo         # see recent pipeline runs
gh run view <run-id> --repo user/repo # see details of one specific run
gh workflow list --repo user/repo     # see all configured automation workflows
```

### A few extra useful `gh` tricks

```bash
gh api user                    # make a raw API call — get info about your account
gh gist create                 # share a small code snippet online
gh release create              # publish a new software release/version
gh search repos kubernetes      # search GitHub for repositories about a topic
gh alias set pv "pr view"      # create your own shortcut command
```

---

# 🎨 Part 10: Making Your GitHub Profile Look Professional

Your GitHub profile is often the FIRST thing a recruiter or hiring manager checks — sometimes even before your resume. A few quick, high-impact improvements:

### 1. Profile README
Create a special repository with the **exact same name as your GitHub username** (e.g., if your username is `rohittingane`, create a repo called `rohittingane`). Any `README.md` you put inside THIS specific repo automatically shows up at the top of your GitHub profile page.

**Keep it short (15-20 lines) and include:**
- A brief intro — who you are, what you're learning right now
- What you're currently working on (e.g., "Currently doing #90DaysOfDevOps")
- Skills/tools you know (Linux, Git, Docker, Python, etc.)
- Links to your best repositories
- How to reach you (LinkedIn, email, etc.)

### 2. Organize your repositories properly
- Use clear, hyphenated names (`shell-scripts`, not `Shell Scripts` or `myProject123`)
- Add a one-line description to every repo (visible right under the repo name on GitHub)
- Every repo should have its own `README.md` explaining what's inside and why

### 3. Pin your best 6 repositories
GitHub lets you "pin" up to 6 repositories to the top of your profile — choose the ones that best represent your actual skills and effort, not random old test repos.

### 4. Clean up
- Delete or archive repos that are empty, abandoned, or irrelevant
- **Very important:** double-check no repo accidentally contains secrets (passwords, API keys, `.env` files) — check your commit HISTORY too, not just the current files, since old commits can still expose secrets even if you later delete the file.

---

# 🔑 Ultimate One-Line Summary Table

| Command | Plain-English meaning |
|---|---|
| `git init` | Turn this folder into a Git-tracked repository |
| `git status` | Show me what's changed / staged / untracked right now |
| `git add <file>` | Mark this file's changes as ready to be saved |
| `git commit -m "msg"` | Permanently save the staged changes as one snapshot |
| `git log` / `git log --oneline` | Show commit history (detailed / compact) |
| `git branch` | List branches / create a new one |
| `git switch <name>` | Move onto a different branch |
| `git merge <branch>` | Combine another branch's changes into the current one |
| `git rebase <branch>` | Replay my commits cleanly on top of another branch |
| `git stash` / `git stash pop` | Temporarily set aside unfinished work / bring it back |
| `git cherry-pick <id>` | Copy just one specific commit onto the current branch |
| `git reset --soft/mixed/hard` | Undo commits locally (varying how much gets deleted) |
| `git revert <id>` | Safely undo a commit by adding a new opposite commit |
| `git remote add origin <url>` | Connect this local repo to a GitHub repository |
| `git push` / `git pull` | Upload my commits / download + merge others' commits |
| `git clone <url>` | Download a full copy of an existing repository |
| `gh repo create` / `gh pr create` / `gh issue create` | Manage GitHub repos/PRs/issues from the terminal |

---

# 💡 A Few Extra Things Worth Knowing (in simple words)

1. **A commit is never truly "lost" immediately, even after `reset --hard`.** Git keeps a safety log called the `reflog` for a while, which can sometimes recover "deleted" commits in emergencies (`git reflog` is worth knowing exists, even if it's rarely needed).

2. **Commit messages matter more than people think.** A good habit: start with a short summary line (under ~50 characters), and if needed, add more detail below it. "Fix login bug" is fine for small changes; for bigger ones, explain WHY the change was made, not just WHAT changed — the "what" is already visible in the code diff itself.

3. **`main` vs `master`** — older repositories often used `master` as the default branch name; newer ones default to `main`. They mean exactly the same thing technically — just a naming convention that shifted over time industry-wide.

4. **A Pull Request (PR) is not a Git feature — it's a GitHub (or GitLab/Bitbucket) feature.** Git itself only handles branches and merging; the "request a review before merging" workflow is something these hosting platforms added on top of Git.

5. **Rebasing rewrites history — reverting doesn't.** This is the single most important distinction to remember: if you're not 100% sure who else might have your branch's commits already, always lean toward the SAFER option (`revert`, `merge`) instead of the history-rewriting ones (`rebase`, `reset --hard`).

6. **GitHub CLI (`gh`) isn't required to use GitHub** — everything it does can also be done through the website. But for repetitive tasks, or automating things inside scripts/CI pipelines, `gh` saves enormous time compared to manually clicking through a browser every time.

---

