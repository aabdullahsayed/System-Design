# Git Basics

## What is it?
Git is a distributed version control system — it tracks every change to your codebase over time, letting multiple people collaborate without overwriting each other's work, and letting you safely revert mistakes.

## Why care?
Every backend job uses Git. It's not optional tooling — it's the substrate all team collaboration and deployment pipelines are built on.

## Core commands
```bash
git init                       # start a new repo
git clone <url>                  # copy an existing remote repo locally
git status                         # what's changed since last commit
git add file.py                     # stage a specific file
git add .                             # stage everything changed
git commit -m "message"                 # save a snapshot of staged changes
git push origin main                      # upload commits to the remote
git pull origin main                        # download + merge remote changes
git log --oneline                             # compact commit history
```

## The 3 areas (understand this mental model)
```
Working Directory  →  Staging Area  →  Repository (committed history)
     (git add)              (git commit)
```
- **Working directory**: your actual files, as you're editing them.
- **Staging area (the "index")**: changes marked ready to be included in the next commit.
- **Repository**: the permanent, committed history.

## Undoing things (know these — you WILL need them)
```bash
git restore file.py              # discard uncommitted changes to a file
git restore --staged file.py       # unstage a file (keep the changes, just un-stage them)
git reset --soft HEAD~1              # undo last commit, keep changes staged
git revert <commit-hash>               # create a NEW commit that undoes a previous one (safe for shared history)
git reset --hard <commit-hash>           # DANGEROUS: discard all changes back to a commit, permanently
```
**Golden rule**: `git revert` is safe for shared/pushed history (adds a new commit); `git reset --hard` rewrites history and should generally be avoided on branches others are also using.

## Writing good commit messages
```
Bad:  "fix bug"
Good: "Fix null pointer exception when user email is missing during signup"
```
A good commit message explains WHAT changed and WHY — future-you (and your team) will thank you when debugging via `git blame` or `git log` months later.

## Practical example
You're mid-way through a feature and realize you need to fix an unrelated urgent bug: `git stash` temporarily shelves your in-progress changes, letting you switch branches cleanly, fix the bug, then `git stash pop` to resume exactly where you left off.
