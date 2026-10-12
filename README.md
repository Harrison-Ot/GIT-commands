# Git Commands Cheat Sheet

A practical, grouped reference of Git commands. Groups are ordered roughly by how often you'll use them, and the ⭐ marks the commands you'll use daily.

## Table of Contents

1. [Setup & Configuration](#1-setup--configuration)
2. [Starting a Repository](#2-starting-a-repository)
3. [Daily Workflow (Most Used)](#3-daily-workflow-most-used)
4. [Branching](#4-branching)
5. [Merging & Rebasing](#5-merging--rebasing)
6. [Remote Repositories](#6-remote-repositories)
7. [Viewing History & Differences](#7-viewing-history--differences)
8. [Undoing Changes](#8-undoing-changes)
9. [Stashing](#9-stashing)
10. [Tagging](#10-tagging)
11. [Cherry-Pick & Patching](#11-cherry-pick--patching)
12. [Searching & Debugging](#12-searching--debugging)
13. [Submodules & Worktrees](#13-submodules--worktrees)
14. [Cleaning & Maintenance](#14-cleaning--maintenance)
15. [Aliases & Productivity](#15-aliases--productivity)
16. [.gitignore Basics](#16-gitignore-basics)
17. [Common Workflows](#17-common-workflows)
18. [Quick Troubleshooting](#18-quick-troubleshooting)

---

## The Top 15 Commands

If you only learn these, you can work with Git every day:

| Command | Purpose |
|---|---|
| `git clone <url>` | Download a repository |
| `git status` | See what changed |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Save staged changes |
| `git push` | Upload commits |
| `git pull` | Download and merge updates |
| `git fetch` | Download updates without merging |
| `git branch` | List/create branches |
| `git checkout` / `git switch` | Change branches |
| `git merge <branch>` | Combine branches |
| `git log --oneline` | View history |
| `git diff` | See unstaged changes |
| `git stash` | Temporarily shelve changes |
| `git reset` | Unstage or move HEAD |
| `git restore` | Discard changes in files |

---

## 1. Setup & Configuration

```bash
git --version                                   # ⭐ Check installed version
git config --global user.name "Your Name"       # ⭐ Set your name
git config --global user.email "you@email.com"  # ⭐ Set your email
git config --global init.defaultBranch main     # Default branch name for new repos
git config --global core.editor "code --wait"   # Set editor (VS Code)
git config --global pull.rebase false           # Pull strategy: merge
git config --global pull.rebase true            # Pull strategy: rebase
git config --global color.ui auto               # Colored output
git config --global core.autocrlf input         # Line endings (macOS/Linux)
git config --global core.autocrlf true          # Line endings (Windows)
git config --list                               # ⭐ Show all settings
git config --global --list                      # Show global settings only
git config user.name                            # Show a single value
git config --global --unset user.name           # Remove a setting
git config --global --edit                      # Open global config in editor
git help <command>                              # Open help for a command
git <command> --help                            # Same as above
```

**Config levels:** `--system` (all users) → `--global` (your user) → `--local` (current repo, default).

---

## 2. Starting a Repository

```bash
git init                          # ⭐ Create a new repo in current folder
git init <folder-name>            # Create a new repo in a new folder
git init --bare                   # Create a bare repo (for servers)
git clone <url>                   # ⭐ Clone a repo
git clone <url> <folder>          # Clone into a specific folder
git clone --depth 1 <url>         # Shallow clone (latest commit only)
git clone -b <branch> <url>       # Clone a specific branch
git clone --recurse-submodules <url>   # Clone including submodules
```

---

## 3. Daily Workflow (Most Used)

### Checking state

```bash
git status                  # ⭐ Show changed/staged/untracked files
git status -s               # Short status
git diff                    # ⭐ Unstaged changes
git diff --staged           # ⭐ Staged changes (same as --cached)
```

### Staging

```bash
git add <file>              # ⭐ Stage a file
git add .                   # ⭐ Stage everything in current directory
git add -A                  # Stage all changes (new, modified, deleted)
git add -u                  # Stage modified and deleted only (no new files)
git add -p                  # ⭐ Interactively stage chunks (hunks)
git add "*.js"              # Stage by pattern
```

### Committing

```bash
git commit -m "message"             # ⭐ Commit staged changes
git commit -am "message"            # ⭐ Stage tracked files and commit
git commit                          # Open editor for a longer message
git commit --amend                  # ⭐ Edit the last commit (message/contents)
git commit --amend --no-edit        # Add staged changes to last commit, keep message
git commit --allow-empty -m "msg"   # Create an empty commit (e.g. trigger CI)
git commit -S -m "msg"              # Sign the commit (GPG)
```

### Moving & removing files

```bash
git mv <old> <new>              # Rename/move a file
git rm <file>                   # Delete file and stage the deletion
git rm --cached <file>          # Stop tracking a file but keep it on disk
git rm -r --cached .            # Untrack everything (useful after editing .gitignore)
```

### Syncing

```bash
git pull                        # ⭐ Fetch + merge from remote
git push                        # ⭐ Upload commits
git push -u origin <branch>     # ⭐ Push and set upstream (first push of a branch)
```

---

## 4. Branching

```bash
git branch                          # ⭐ List local branches
git branch -a                       # List local + remote branches
git branch -r                       # List remote branches
git branch -v                       # Show last commit per branch
git branch -vv                      # Show tracking info
git branch <name>                   # ⭐ Create a branch
git branch -d <name>                # ⭐ Delete a merged branch
git branch -D <name>                # Force delete an unmerged branch
git branch -m <old> <new>           # Rename a branch
git branch -m <new>                 # Rename current branch
git branch --merged                 # Branches already merged into current
git branch --no-merged              # Branches not yet merged
git branch -u origin/<branch>       # Set upstream for current branch

git checkout <branch>               # ⭐ Switch branch (classic)
git checkout -b <branch>            # ⭐ Create and switch (classic)
git switch <branch>                 # ⭐ Switch branch (modern)
git switch -c <branch>              # ⭐ Create and switch (modern)
git switch -                        # Switch to previous branch
git checkout -b <branch> origin/<branch>   # Create local branch from remote
git checkout <commit-hash>          # Detached HEAD at a commit
```

**Delete a remote branch:**

```bash
git push origin --delete <branch>
```

---

## 5. Merging & Rebasing

### Merge

```bash
git merge <branch>                  # ⭐ Merge branch into current
git merge --no-ff <branch>          # Always create a merge commit
git merge --squash <branch>         # Combine all commits into one (then commit)
git merge --abort                   # ⭐ Cancel a conflicted merge
git merge --continue                # Continue after resolving conflicts
```

### Rebase

```bash
git rebase <branch>                 # ⭐ Replay current branch on top of another
git rebase main                     # Example: update feature branch with main
git rebase -i HEAD~3                # ⭐ Interactive rebase (squash/reword/drop last 3)
git rebase -i <commit>              # Interactive rebase from a commit
git rebase --continue               # Continue after resolving conflicts
git rebase --skip                   # Skip the current conflicting commit
git rebase --abort                  # Cancel the rebase
git pull --rebase                   # Pull using rebase instead of merge
```

**Interactive rebase keywords:** `pick`, `reword`, `edit`, `squash`, `fixup`, `drop`.

> ⚠️ Never rebase commits that have been pushed to a shared branch.

### Resolving conflicts

```bash
git status                          # Find conflicted files
# Edit files, remove <<<<<<<, =======, >>>>>>> markers
git add <file>                      # Mark as resolved
git commit                          # Finish merge (or git rebase --continue)
git checkout --ours <file>          # Keep your version
git checkout --theirs <file>        # Keep their version
git mergetool                       # Open configured merge tool
```

---

## 6. Remote Repositories

```bash
git remote -v                           # ⭐ List remotes
git remote add origin <url>             # ⭐ Add a remote
git remote remove <name>                # Remove a remote
git remote rename <old> <new>           # Rename a remote
git remote set-url origin <new-url>     # Change a remote's URL
git remote show origin                  # Detailed info about a remote
git remote prune origin                 # Remove stale remote-tracking branches

git fetch                               # ⭐ Download updates (no merge)
git fetch --all                         # Fetch from all remotes
git fetch --prune                       # Fetch and delete stale remote refs
git pull                                # ⭐ Fetch + merge
git pull origin <branch>                # Pull a specific branch
git pull --rebase                       # Fetch + rebase

git push                                # ⭐ Push current branch
git push origin <branch>                # Push specific branch
git push -u origin <branch>             # Push and track
git push --all                          # Push all branches
git push --tags                         # Push all tags
git push --force-with-lease             # ⭐ Safer force push
git push --force                        # ⚠️ Overwrite remote history (dangerous)
git push origin --delete <branch>       # Delete a remote branch
```

---

## 7. Viewing History & Differences

### Log

```bash
git log                                 # ⭐ Full history
git log --oneline                       # ⭐ One line per commit
git log --oneline --graph --all         # ⭐ Visual branch graph
git log --oneline --graph --decorate --all
git log -n 5                            # Last 5 commits
git log --stat                          # Files changed per commit
git log -p                              # Show diffs per commit
git log --author="Name"                 # Filter by author
git log --since="2 weeks ago"           # Filter by date
git log --until="2026-01-01"
git log --grep="fix"                    # Search commit messages
git log -- <file>                       # History of a file
git log -p -- <file>                    # Diffs of a file over time
git log --follow <file>                 # History including renames
git log main..feature                   # Commits in feature not in main
git log --pretty=format:"%h %an %s"     # Custom format
git reflog                              # ⭐ History of HEAD moves (recovery tool)
```

### Show & diff

```bash
git show <commit>                       # ⭐ Show a commit's details
git show <commit>:<file>                # Show a file as of a commit
git show --stat <commit>                # Summary of changes
git diff                                # Working dir vs staging
git diff --staged                       # Staging vs last commit
git diff HEAD                           # Working dir vs last commit
git diff <branch1> <branch2>            # Compare two branches
git diff <commit1> <commit2>            # Compare two commits
git diff --stat                         # Summary only
git diff --name-only                    # List changed file names
git diff main...feature                 # Changes on feature since branching from main
```

### Other info

```bash
git shortlog -sn                        # Commit counts per author
git ls-files                            # List tracked files
git ls-tree -r HEAD --name-only         # List files in the last commit
git rev-parse HEAD                      # Print full hash of HEAD
git rev-parse --short HEAD              # Print short hash
git describe --tags                     # Nearest tag description
```

---

## 8. Undoing Changes

### Discard working-directory changes

```bash
git restore <file>                      # ⭐ Discard changes in a file
git restore .                           # Discard all unstaged changes
git checkout -- <file>                  # Classic way to discard changes
```

### Unstage

```bash
git restore --staged <file>             # ⭐ Unstage a file
git reset <file>                        # Classic way to unstage
git reset                               # Unstage everything
```

### Reset (move HEAD / branch pointer)

```bash
git reset --soft HEAD~1                 # ⭐ Undo last commit, keep changes staged
git reset --mixed HEAD~1                # Undo last commit, keep changes unstaged (default)
git reset --hard HEAD~1                 # ⚠️ Undo last commit and DELETE changes
git reset --hard <commit>               # ⚠️ Jump back to a commit, discard everything after
git reset --hard origin/main            # ⚠️ Make local match remote exactly
```

| Mode | HEAD | Staging | Working Dir |
|---|---|---|---|
| `--soft` | moves | kept | kept |
| `--mixed` | moves | reset | kept |
| `--hard` | moves | reset | reset ⚠️ |

### Revert (safe for shared branches)

```bash
git revert <commit>                     # ⭐ Create a new commit that undoes a commit
git revert HEAD                         # Undo the last commit safely
git revert -m 1 <merge-commit>          # Revert a merge commit
git revert --no-commit <commit>         # Revert without committing yet
```

### Recovering lost work

```bash
git reflog                              # Find the lost commit hash
git checkout <hash>                     # Inspect it
git branch recovered <hash>             # Save it to a branch
git reset --hard <hash>                 # Or move the branch back to it
```

**Reset vs Revert:** use `reset` for local, unpushed work; use `revert` for anything already pushed/shared.

---

## 9. Stashing

```bash
git stash                               # ⭐ Shelve tracked changes
git stash push -m "message"             # ⭐ Stash with a name
git stash -u                            # Include untracked files
git stash -a                            # Include untracked and ignored files
git stash list                          # ⭐ List stashes
git stash show                          # Summary of latest stash
git stash show -p stash@{0}             # Full diff of a stash
git stash pop                           # ⭐ Apply latest stash and remove it
git stash apply                         # Apply latest stash and keep it
git stash apply stash@{2}               # Apply a specific stash
git stash drop stash@{0}                # Delete a stash
git stash clear                         # ⚠️ Delete all stashes
git stash branch <name>                 # Create a branch from a stash
```

---

## 10. Tagging

```bash
git tag                                 # List tags
git tag v1.0.0                          # Lightweight tag
git tag -a v1.0.0 -m "Release 1.0.0"    # ⭐ Annotated tag
git tag -a v1.0.0 <commit>              # Tag a past commit
git show v1.0.0                         # Show tag details
git push origin v1.0.0                  # ⭐ Push one tag
git push origin --tags                  # Push all tags
git tag -d v1.0.0                       # Delete local tag
git push origin --delete v1.0.0         # Delete remote tag
git checkout v1.0.0                     # Checkout a tag (detached HEAD)
git tag -l "v1.*"                       # Filter tags by pattern
```

---

## 11. Cherry-Pick & Patching

```bash
git cherry-pick <commit>                # ⭐ Apply a single commit to current branch
git cherry-pick <c1> <c2>               # Apply multiple commits
git cherry-pick A..B                    # Apply a range
git cherry-pick -n <commit>             # Apply changes without committing
git cherry-pick --continue              # Continue after conflict
git cherry-pick --abort                 # Cancel

git format-patch -1 <commit>            # Create a patch file from a commit
git format-patch main                   # Patches for all commits since main
git apply <patch-file>                  # Apply a patch (no commit)
git am <patch-file>                     # Apply a patch and commit
git diff > changes.patch                # Save diff as a patch
```

---

## 12. Searching & Debugging

```bash
git grep "text"                         # Search tracked files
git grep -n "text"                      # With line numbers
git log -S "text"                       # Find commits that added/removed "text"
git log -G "regex"                      # Find commits matching a regex in diffs
git blame <file>                        # ⭐ See who changed each line
git blame -L 10,20 <file>               # Blame specific lines

git bisect start                        # ⭐ Binary-search for a bug-introducing commit
git bisect bad                          # Mark current commit as bad
git bisect good <commit>                # Mark a known good commit
git bisect good / git bisect bad        # Repeat as Git checks out midpoints
git bisect reset                        # Finish and return to original branch

git fsck                                # Check repository integrity
```

---

## 13. Submodules & Worktrees

### Submodules

```bash
git submodule add <url> <path>          # Add a submodule
git submodule init                      # Initialize submodules
git submodule update                    # Fetch submodule content
git submodule update --init --recursive # ⭐ Init + update everything
git submodule update --remote           # Pull latest from submodule remotes
git submodule status                    # Show submodule status
git submodule foreach 'git pull'        # Run a command in each submodule
```

### Worktrees (multiple working directories, one repo)

```bash
git worktree add ../hotfix hotfix       # Check out a branch in a new folder
git worktree add -b new-feature ../feat # Create branch + worktree
git worktree list                       # List worktrees
git worktree remove ../hotfix           # Remove a worktree
git worktree prune                      # Clean up stale entries
```

---

## 14. Cleaning & Maintenance

```bash
git clean -n                            # ⭐ Dry run: show what would be deleted
git clean -f                            # Delete untracked files
git clean -fd                           # Delete untracked files and folders
git clean -fdx                          # ⚠️ Also delete ignored files
git gc                                  # Garbage collect / optimize repo
git prune                               # Remove unreachable objects
git count-objects -vH                   # Show repo size info
git remote prune origin                 # Clean stale remote branches
git fetch --prune                       # Fetch and prune
git archive --format=zip HEAD > repo.zip   # Export the repo as a zip
```

---

## 15. Aliases & Productivity

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.sw switch
git config --global alias.unstage "restore --staged"
git config --global alias.last "log -1 HEAD"
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Usage:

```bash
git st        # = git status
git lg        # = pretty graph log
git unstage file.txt
```

**Handy shell aliases** (add to `~/.bashrc` or `~/.zshrc`):

```bash
alias gs='git status'
alias ga='git add'
alias gaa='git add -A'
alias gc='git commit -m'
alias gp='git push'
alias gl='git pull'
alias gco='git checkout'
alias gb='git branch'
alias gd='git diff'
alias glg='git log --oneline --graph --decorate --all'
```

---

## 16. .gitignore Basics

```gitignore
# Dependencies
node_modules/
vendor/

# Environment & secrets
.env
.env.*
!.env.example

# Build output
dist/
build/
.next/

# Logs & OS files
*.log
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
```

```bash
git check-ignore -v <file>              # Why is this file ignored?
git rm -r --cached .                    # Re-apply .gitignore to tracked files
git add . && git commit -m "Apply .gitignore"
git update-index --assume-unchanged <file>   # Ignore local changes to a tracked file
```

---

## 17. Common Workflows

### First-time project setup

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <url>
git push -u origin main
```

### Feature branch workflow

```bash
git switch main
git pull
git switch -c feature/login
# ...work...
git add .
git commit -m "Add login form"
git push -u origin feature/login
# Open a Pull Request, then after merge:
git switch main
git pull
git branch -d feature/login
```

### Keep your branch updated with main

```bash
git fetch origin
git rebase origin/main        # or: git merge origin/main
```

### Fork workflow (contributing to open source)

```bash
git clone <your-fork-url>
git remote add upstream <original-repo-url>
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

### Squash last N commits

```bash
git rebase -i HEAD~N          # mark all but the first as "squash" or "fixup"
```

### Hotfix while mid-work

```bash
git stash -u
git switch -c hotfix/bug main
# fix, commit, push
git switch -
git stash pop
```

---

## 18. Quick Troubleshooting

| Problem | Fix |
|---|---|
| Committed to the wrong branch | `git reset --soft HEAD~1`, `git stash`, switch branch, `git stash pop`, commit |
| Wrong commit message | `git commit --amend -m "New message"` (before pushing) |
| Forgot a file in last commit | `git add <file>` then `git commit --amend --no-edit` |
| Undo last commit, keep changes | `git reset --soft HEAD~1` |
| Undo a pushed commit | `git revert <commit>` then `git push` |
| Accidentally deleted a branch | `git reflog` → `git branch <name> <hash>` |
| Merge went wrong | `git merge --abort` |
| Detached HEAD | `git switch -c <new-branch>` to keep your work |
| Push rejected (non-fast-forward) | `git pull --rebase` then `git push` |
| Committed a secret | Rotate the secret first, then clean history with `git filter-repo` or BFG |
| Large file blocked | Use Git LFS: `git lfs install` and `git lfs track "*.psd"` |
| `.gitignore` not working | `git rm -r --cached .` then re-add and commit |
| Wrong author on commits | `git commit --amend --reset-author --no-edit` |

---

## Commit Message Convention (Conventional Commits)

```
<type>(<scope>): <short description>

feat:     a new feature
fix:      a bug fix
docs:     documentation only
style:    formatting, no code change
refactor: code change that neither fixes a bug nor adds a feature
perf:     performance improvement
test:     adding or fixing tests
chore:    build process, tooling, dependencies
```

Examples:

```
feat(auth): add password reset flow
fix(api): handle null response from payments endpoint
docs(readme): add installation steps
```

---

## Safety Rules of Thumb

- Use `git status` and `git diff` before every commit.
- Prefer `git revert` over `git reset` on shared branches.
- Prefer `--force-with-lease` over `--force`.
- Commit small and often, with clear messages.
- Never commit secrets, `.env` files, or `node_modules`.
- Use `git clean -n` before `git clean -f`.
- When in doubt, `git reflog` can usually bring your work back.

---

## Contributing

Spotted a missing command or a mistake? Open an issue or submit a pull request.

## License

MIT
