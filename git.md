# 🔀 Git & Version Control

> **Interview-Ready | Practical Commands | Workflow Focused**
> *Questions sourced from real technical interviews 2024–2026*

---

## Table of Contents

1. [Core Concepts](#1-core-concepts)
2. [Essential Commands](#2-essential-commands)
3. [Branching & Merging](#3-branching--merging)
4. [Rebase vs Merge](#4-rebase-vs-merge)
5. [Advanced Commands](#5-advanced-commands)
6. [Git Workflows](#6-git-workflows)
7. [Undoing Mistakes](#7-undoing-mistakes)
8. [Interview Q&A](#8-interview-qa)

---

## 1. Core Concepts

### How Git Works Internally

```
Working Directory → Staging Area (Index) → Local Repository → Remote Repository
      ↑                   ↑                      ↑                   ↑
   Your files          git add             git commit            git push
```

```
Three states of a file:
  Modified:  Changed in working directory, not yet staged.
  Staged:    Marked to go into the next commit (git add).
  Committed: Safely stored in local Git repository.
```

### Git Objects

```
Blob:   Content of a file (no filename, just content).
Tree:   Directory — maps filenames to blobs (snapshot of a directory).
Commit: Points to a tree + metadata (author, message, parent commits).
Tag:    Named pointer to a specific commit.

Everything in Git is content-addressed by SHA-1 hash.
Commit hash = SHA-1(tree + parent + author + message + timestamp)
→ This is why Git history is tamper-evident.
```

### HEAD

```
HEAD = pointer to the current commit (what your working directory is based on).

Normally: HEAD → branch name → latest commit on that branch.
  HEAD → main → abc123

Detached HEAD: HEAD points directly to a commit, not a branch.
  HEAD → abc123 (dangerous — new commits will be lost unless you create a branch)

After git checkout <commit_hash>:
  HEAD → abc123   (detached)
```

---

## 2. Essential Commands

### Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"  # VS Code as editor
git config --list                               # view all config
```

### Starting a Repository

```bash
git init                    # initialise new repo in current directory
git clone <url>             # clone remote repo
git clone <url> my-folder   # clone into specific folder
```

### Basic Workflow

```bash
# Check status
git status                  # what's modified, staged, untracked?
git diff                    # changes in working dir vs staged
git diff --staged           # changes staged vs last commit

# Staging
git add file.py             # stage specific file
git add .                   # stage all changes
git add -p                  # interactively stage chunks (patch mode)

# Committing
git commit -m "feat: add sentiment analysis endpoint"
git commit --amend          # edit the last commit (message or files)
git commit --amend --no-edit  # amend without changing message (add forgotten file)
```

### Viewing History

```bash
git log                     # full log
git log --oneline           # condensed one line per commit
git log --oneline --graph   # visualise branch structure
git log -n 5                # last 5 commits
git log --author="Amogh"    # commits by specific author
git log -- path/to/file     # commits that touched a file
git show abc123             # show details of specific commit
git blame file.py           # who wrote each line and when
```

### Remote Operations

```bash
git remote -v                           # list remote connections
git remote add origin <url>             # add remote
git fetch origin                        # download remote changes (don't merge)
git pull origin main                    # fetch + merge
git pull --rebase origin main           # fetch + rebase (cleaner history)
git push origin main                    # push local commits to remote
git push -u origin feature-branch      # push and set upstream tracking
git push --force-with-lease            # force push safely (checks no one else pushed)
```

---

## 3. Branching & Merging

> **Interview Q:** *Explain the difference between git merge and git rebase.*

### Branch Commands

```bash
git branch                      # list local branches
git branch -a                   # list all branches (local + remote)
git branch feature-xyz          # create new branch
git checkout feature-xyz        # switch to branch
git checkout -b feature-xyz     # create AND switch (shorthand)
git switch -c feature-xyz       # modern equivalent of checkout -b
git branch -d feature-xyz       # delete branch (safe — won't delete unmerged)
git branch -D feature-xyz       # force delete branch
git branch -m old-name new-name # rename branch
```

### Merge

```bash
git checkout main
git merge feature-xyz           # merge feature into main

# Types of merge:
# Fast-Forward: no divergence, just moves pointer forward (no merge commit)
# Three-Way:    divergence exists, creates a merge commit
```

```
Fast-Forward Merge:
main:    A → B
feature:         C → D

After merge:
main:    A → B → C → D     (pointer moved, no merge commit)

Three-Way Merge:
main:    A → B → E
feature:     C → D

After merge:
main:    A → B → E → M     (M is the merge commit)
               ↗
         C → D
```

### Handling Merge Conflicts

```bash
# Git marks conflict in the file:
<<<<<<< HEAD
result = model.predict_v1(data)    # your version
=======
result = model.predict_v2(data)    # incoming version
>>>>>>> feature-xyz

# Steps to resolve:
1. Open the file, choose which version to keep (or combine both)
2. Remove the conflict markers (<<<<<<<, =======, >>>>>>>)
3. git add conflicted_file.py
4. git commit                       # finalises the merge
# Or: git merge --abort            # cancel entire merge
```

---

## 4. Rebase vs Merge

> **Interview Q:** *When would you use rebase vs merge? What is the golden rule of rebasing?*

### Rebase

```bash
git checkout feature-xyz
git rebase main

# What it does:
# 1. Finds common ancestor of feature and main
# 2. Temporarily removes feature's commits
# 3. Fast-forwards feature to tip of main
# 4. Replays feature's commits on top, one by one
# 5. Each replayed commit gets a NEW SHA hash (history is rewritten)
```

```
Before rebase:
main:    A → B → C
feature: A → D → E

After: git rebase main (on feature branch)
main:    A → B → C
feature: A → B → C → D' → E'   (D and E are NEW commits — same changes, new hashes)
```

### Merge vs Rebase Comparison

| | Merge | Rebase |
|---|---|---|
| History | Preserves exact history with merge commits | Linear, clean history |
| Merge commit | Yes (three-way) | No |
| Original commits | Preserved (same SHAs) | Rewritten (new SHAs) |
| Traceability | Shows when branches diverged | Harder to see original branch point |
| Conflicts | Resolve once | Resolve at each replayed commit |
| Use for | Shared/public branches | Local/feature branches |

### The Golden Rule of Rebasing

```
NEVER rebase commits that have been pushed to a shared public branch.

Why: Rebase rewrites history (new SHA hashes).
     If others have pulled the original commits, their history now diverges.
     They'll have duplicate commits when they try to pull.
     This causes chaos in collaborative repos.

✓ Safe to rebase: your local feature branch not yet pushed (or only you use it)
❌ Never rebase: main, develop, or any branch others have pulled
```

### Interactive Rebase

```bash
git rebase -i HEAD~4    # interactively edit last 4 commits

# Opens editor showing commits:
pick abc123 feat: add data loader
pick def456 fix: typo in data loader
pick ghi789 feat: add model training
pick jkl012 wip: debug stuff

# Commands you can use:
pick    = keep commit as-is
reword  = keep commit, edit message
edit    = pause to amend this commit
squash  = combine with previous commit (keep both messages)
fixup   = combine with previous commit (discard this message)
drop    = delete this commit entirely

# Common use: squash WIP commits before merging PR
pick abc123 feat: add data loader
squash def456 fix: typo in data loader   ← combines into one clean commit
pick ghi789 feat: add model training
drop jkl012 wip: debug stuff             ← remove debug commit
```

---

## 5. Advanced Commands

### Cherry-Pick

```bash
# Apply a specific commit from one branch to another
git cherry-pick <commit-hash>

# Use case: Bug fix is on feature branch; need it in main too
git checkout main
git cherry-pick abc123    # applies only that commit to main

# Cherry-pick a range
git cherry-pick abc123..def456

# When to use: Backport a specific bug fix to a release branch
#              Apply a specific feature without merging the entire branch
```

### Stash

```bash
# Temporarily save work-in-progress without committing
git stash                   # stash current changes
git stash push -m "WIP: debugging model loading"  # with message
git stash list              # show all stashes
git stash pop               # apply most recent stash and remove it
git stash apply stash@{2}   # apply specific stash (keep it in stash list)
git stash drop stash@{0}    # delete a stash
git stash clear             # delete all stashes

# Use case: You're in the middle of feature work and urgently need to fix a bug on main
git stash          # save your WIP
git checkout main
git checkout -b hotfix
# ... fix the bug, commit, merge ...
git checkout feature-branch
git stash pop      # restore your WIP
```

### Reset

```bash
# Undo commits — understand the three modes
git reset --soft HEAD~1    # undo last commit, keep changes STAGED
git reset --mixed HEAD~1   # undo last commit, keep changes in working dir (default)
git reset --hard HEAD~1    # undo last commit, DISCARD all changes (destructive!)

# Example scenarios:
git reset --soft HEAD~1    # "I committed too early, need to add more files"
git reset --mixed HEAD~1   # "I want to redo this commit message and changes"
git reset --hard HEAD~1    # "Completely undo this commit, I don't want those changes"

# CAUTION: --hard discards work permanently (unless you have it in reflog)
```

### Reflog — Your Safety Net

```bash
# Git records every movement of HEAD in the reflog
git reflog              # shows last 90 days of HEAD movements

# Output:
# abc123 HEAD@{0}: commit: feat: add prediction endpoint
# def456 HEAD@{1}: reset: moving to HEAD~1
# ghi789 HEAD@{2}: commit: WIP: debugging

# Recover from accidental hard reset:
git reset --hard HEAD~3    # oops, lost 3 commits
git reflog                 # find the commit hash from before the reset
git reset --hard abc123    # restore to that point

# Recover deleted branch:
git branch recovered-branch ghi789   # create branch at old commit
```

### Tags

```bash
git tag v1.0.0                      # lightweight tag (pointer to commit)
git tag -a v1.0.0 -m "Release 1.0" # annotated tag (recommended, has metadata)
git push origin v1.0.0              # push specific tag
git push origin --tags              # push all tags
git tag -d v1.0.0                   # delete local tag
git push origin --delete v1.0.0     # delete remote tag

# In ML: tag commits that correspond to model releases
git tag -a model-v2.3 -m "BERT fine-tuned on sentiment, 94% accuracy"
```

---

## 6. Git Workflows

### Gitflow

```
Long-running branches:
  main     → production-ready code, tagged with releases
  develop  → integration branch for features

Short-lived branches:
  feature/xxx    → new feature (branches off develop, merges back to develop)
  release/1.2.0  → release prep (branches off develop, merges to main + develop)
  hotfix/xxx     → urgent production fix (branches off main, merges to main + develop)

Best for: Scheduled release cycles, versioned software, larger teams.
```

### Trunk-Based Development (Preferred at most tech companies)

```
Single main branch (trunk). Short-lived feature branches (1-2 days max).
Feature flags for incomplete features. Frequent merges to main (multiple per day).

Best for: Continuous deployment, fast-moving teams, microservices.
Most used at: Google, Facebook, most modern startups.

Advantages:
  → Fewer merge conflicts (small, frequent merges)
  → Simpler model
  → Supports continuous integration
```

### Feature Branch Workflow (Most Common)

```
1. Create branch from main:  git checkout -b feature/sentiment-api
2. Make commits on feature branch
3. Push to remote:           git push origin feature/sentiment-api
4. Open Pull Request (PR)
5. Code review + CI passes
6. Merge to main
7. Delete feature branch

In ML projects: each experiment or model version gets its own branch.
```

---

## 7. Undoing Mistakes

```bash
# Undo staged changes (unstage a file)
git restore --staged file.py        # modern
git reset HEAD file.py              # old way

# Undo changes in working directory (dangerous — permanent!)
git restore file.py                 # discard working dir changes
git checkout -- file.py             # old way

# Create a new "undo" commit (safe for shared branches)
git revert HEAD                     # reverts last commit with new commit
git revert abc123                   # reverts specific commit with new commit
# vs git reset which rewrites history (unsafe for shared branches)

# Fix last commit
git add forgotten_file.py
git commit --amend --no-edit        # add file to last commit without changing message

# Remove file from last commit
git reset HEAD~ file.py             # unstage from last commit
git commit --amend

# Accidentally committed to main instead of feature branch
git branch feature-xyz              # create branch at current position
git reset --hard HEAD~1             # move main back one commit
git checkout feature-xyz            # continue on feature branch
```

---

## 8. Interview Q&A

**Q: What is the difference between git fetch and git pull?**
> `git fetch` downloads commits, branches, and tags from the remote but does NOT merge them into your local branch. Your local branch stays unchanged. You can then inspect the changes before merging. `git pull` = `git fetch` + `git merge` (or `git fetch` + `git rebase` if configured). `git pull --rebase` is generally preferred to keep a linear history. Rule: use `git fetch` + review + `git merge` when you want control; use `git pull` for convenience on your own branches.

**Q: What is the difference between git reset and git revert?**
> `git reset` moves the branch pointer backward, rewriting history — it's as if the commits never happened. Destructive. Only use on local/private branches. `git revert` creates a NEW commit that undoes the changes of a specified commit, preserving history. Safe for shared/public branches because it doesn't rewrite history. Rule: `reset` for local cleanup, `revert` for undoing changes on shared branches.

**Q: What is a detached HEAD state?**
> HEAD normally points to a branch, which points to the latest commit. Detached HEAD means HEAD points directly to a commit hash, not a branch. This happens when you `git checkout <commit-hash>` or `git checkout <tag>`. In this state, new commits you make aren't on any branch — if you switch branches, those commits will be lost (garbage collected). Fix: `git branch new-branch-name` to save your work before switching away.

**Q: How do you resolve merge conflicts?**
> Open the conflicted file(s). Git marks conflicts with `<<<<<<<`, `=======`, `>>>>>>>`. Choose which version to keep (or manually combine both), then remove the markers. `git add` the resolved file. `git commit` to finalise. If you want to abort: `git merge --abort`. For complex conflicts, use a merge tool: `git mergetool` (opens VS Code, IntelliJ, or configured diff tool).

**Q: What is the difference between git cherry-pick and git merge?**
> `git merge` brings ALL commits from a branch. `git cherry-pick` applies a specific single commit (or a range) to the current branch without bringing the entire branch history. Use cherry-pick when: you need one specific bug fix from a branch but not its other changes, or you need to backport a fix to an older release branch.

**Q: What is `git stash` and when would you use it?**
> `git stash` temporarily saves your uncommitted changes (both staged and unstaged) and reverts your working directory to the last commit. Your work is saved in a stash stack. Use it when: you're mid-feature and need to quickly switch context (urgent bug fix on main), you want to pull latest changes from remote before committing your work, or you accidentally started work on the wrong branch.

**Q: How do you squash multiple commits into one?**
> Use interactive rebase: `git rebase -i HEAD~N` (where N is the number of commits). In the editor, change `pick` to `squash` (or `fixup`) for commits you want to combine. All squashed commits merge into the first `pick` commit. This is commonly done before merging a PR to clean up "WIP: debugging" and "fix typo" commits into one meaningful commit. Remember: only squash commits that haven't been pushed to a shared branch.

**Q: What is a .gitignore file and what should be in it for an ML project?**
```
# Common .gitignore for ML projects:
__pycache__/
*.pyc
.env                    # environment variables (API keys, passwords)
*.pkl                   # model files (too large for git, use git-lfs or model registry)
*.pt                    # PyTorch model files
*.h5                    # Keras model files
data/                   # raw data (too large, use DVC or cloud storage)
wandb/                  # wandb run logs
mlruns/                 # MLflow runs
.ipynb_checkpoints/     # Jupyter checkpoints
venv/
.DS_Store
```

---

*Sources: InterviewBit Git Interview Questions 2025, LabEx Git Interview Q&A, DevOps Training Institute Git Branching Questions, Dev.to Git Interview Questions.*
