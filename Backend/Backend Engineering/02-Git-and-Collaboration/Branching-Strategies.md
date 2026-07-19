# Branching Strategies

## What is it?
A **branch** is an independent line of development — letting you work on a feature or fix without affecting the main codebase until it's ready. Branching strategies are team conventions for how branches are created, named, and merged.

## Why care?
Uncoordinated branching in a team leads to painful merge conflicts and unstable main branches. A clear strategy is what lets multiple engineers ship code confidently, in parallel, without stepping on each other.

## Basic branch commands
```bash
git branch feature/user-auth        # create a branch
git checkout feature/user-auth        # switch to it
git checkout -b feature/user-auth       # create AND switch, in one command
git switch feature/user-auth              # modern equivalent of checkout for switching
git merge feature/user-auth                 # merge a branch into your current branch
git branch -d feature/user-auth                # delete a branch (after merging)
```

## Common strategies

### Trunk-Based Development (increasingly the industry default)
Everyone commits small, frequent changes directly to `main` (or via very short-lived feature branches merged within a day or two), protected by feature flags for anything not ready to fully ship. Favors continuous integration and fast feedback over long-lived branches.

### GitHub Flow (simple, widely used)
```
main (always deployable)
  └── feature/xyz branch → Pull Request → code review → merge to main → deploy
```
Simple: one main branch, short-lived feature branches, deploy from `main` after merge.

### Git Flow (more structured, common in versioned/release-based software)
```
main (production releases only)
develop (integration branch)
  ├── feature/xyz (new features, branch from develop)
  ├── release/1.2.0 (release prep/stabilization)
  └── hotfix/critical-bug (urgent production fixes, branch from main)
```
More ceremony/structure — better suited to software with distinct, scheduled release cycles (e.g., mobile apps, versioned enterprise software) rather than continuously-deployed web backends.

## Merge vs Rebase
```bash
git merge feature-branch      # preserves full history, creates a merge commit
git rebase main                 # replays your branch's commits on top of main — linear, cleaner history, but rewrites commit hashes
```
**Rule of thumb**: rebase your OWN local, unpushed/unshared branch to keep history clean; avoid rebasing branches that others have already pulled/based work on (rewrites shared history, causing painful conflicts for collaborators).

## Practical example
A team using GitHub Flow: an engineer creates `feature/add-payment-webhook`, commits incrementally, opens a Pull Request against `main` once ready, gets it code-reviewed (`Code-Review.md`), then merges — CI/CD automatically deploys `main` after merge (`10-DevOps-and-Deployment/CI-CD.md`), keeping the whole team's `main` branch always in a deployable state.
