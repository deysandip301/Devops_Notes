# Git Refreshers – Concise Notes

## What is Git & GitHub?
- Git: Distributed version control system (DVCS) for tracking code changes.
- GitHub: Cloud platform for hosting Git repositories and collaboration.

## Types of Version Control
- Local: Only on your machine.
- Remote: Central server (e.g., SVN).
- Distributed: Each user has full repo (e.g., Git).

## Install & Configure Git
- Install: `apt install git` or `yum install git`
- Check: `git --version`
- Configure:
  - `git config --global user.name "Your Name"`
  - `git config --global user.email "you@example.com"`
  - `git config --global core.editor "vim"`

## SSH Setup
- Generate key: `ssh-keygen -t rsa -b 4096 -C "email@example.com"`
- Add public key to GitHub for passwordless push/pull.

## Git Workflow (Core Areas)
- Working Directory: Where you edit files.
- Staging Area (Index): Where you prepare files for commit (`git add`).
- Local Repository: Where commits are stored (`git commit`).
- Remote Repository: Hosted on GitHub (`git push`/`git pull`).

## Common Git Commands
- Initialize repo: `git init`
- Check status: `git status`
- Add file(s): `git add file.txt` or `git add .`
- Commit: `git commit -m "message"`
- Set remote: `git remote add origin <url>`
- Push: `git push origin main`
- Clone: `git clone <url>`
- Pull: `git pull`
- View log: `git log --oneline --graph --decorate --all`
- Diff: `git diff` (unstaged), `git diff --cached` (staged)

## Branching & Merging
- List branches: `git branch`
- Create branch: `git branch feature`
- Switch branch: `git switch feature` or `git checkout feature`
- Merge: `git merge feature`
- Delete branch: `git branch -d feature`
- Rebase: `git rebase main`

## Undo & Stash
- Unstage: `git restore --staged file.txt`
- Amend commit: `git commit --amend`
- Stash changes: `git stash`
- Apply stash: `git stash apply`

## Remote Management
- List remotes: `git remote -v`
- Add remote: `git remote add origin <url>`
- Change remote: `git remote set-url origin <url>`
- Fetch: `git fetch origin`

## Tags
- Create tag: `git tag v1.0.0`
- Annotated tag: `git tag -a v1.0.0 -m "msg"`
- Push tags: `git push origin --tags`

## Ignore & Attributes
- Ignore files: Add patterns to `.gitignore`
- Check ignored: `git status --ignored`
- Attributes: `.gitattributes` for filters, eol, etc.

## Safety & Best Practices
- View config: `git config --list`
- Fetch all/prune: `git fetch --all --prune`
- See merged: `git branch --merged`
- Use `git help <command>` or `<command> -h` for quick docs.

---
Focus on: add, commit, push, pull, branch, merge, stash, log, diff, status, and config.
