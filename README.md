# git-github_cheat_sheet
# 📘 Git Cheat Sheet (README.md)

This README is a **clean Markdown version** of the Git Cheat Sheet PDF, with **simple explanations** so you understand *what each command does and when to use it*.

---

## 🧠 What is Git?

**Git** is a **distributed version control system** that helps you:

* Track code changes
* Work with multiple versions
* Collaborate with others safely

Git works **locally on your computer**. Platforms like **GitHub** use Git underneath.

---

## ⚙️ SETUP (One-Time Configuration)

These commands configure your identity for all repositories.

```bash
git config --global user.name "Your Name"
```

➡️ Sets your name (visible in commit history)

```bash
git config --global user.email "you@email.com"
```

➡️ Sets your email for commits

```bash
git config --global color.ui auto
```

➡️ Enables colored output in terminal (readability)

---

## 📁 SETUP & INIT (Starting a Repository)

```bash
git init
```

➡️ Initialize an existing folder as a Git repository

```bash
git clone <url>
```

➡️ Copy a remote repository (GitHub, GitLab) to your local system

---

## 📸 STAGE & SNAPSHOT (Daily Workflow)

### Check status

```bash
git status
```

➡️ Shows modified, staged, and untracked files

### Stage a file

```bash
git add <file>
```

➡️ Adds file to **staging area** (ready to commit)

### Unstage a file

```bash
git reset <file>
```

➡️ Removes file from staging, keeps changes

### See unstaged changes

```bash
git diff
```

➡️ Shows changes not staged yet

### See staged changes

```bash
git diff --staged
```

➡️ Shows changes that will go into next commit

### Commit changes

```bash
git commit -m "message"
```

➡️ Saves staged changes permanently

---

## 🌿 BRANCH & MERGE

### List branches

```bash
git branch
```

➡️ Shows all branches (`*` = current branch)

### Create new branch

```bash
git branch <branch-name>
```

➡️ Creates a new branch

### Switch branch

```bash
git checkout <branch>
```

➡️ Switches to another branch

### Merge branch

```bash
git merge <branch>
```

➡️ Merges specified branch into current branch

### View commit history

```bash
git log
```

➡️ Shows commit history

---

## 🌍 SHARE & UPDATE (Remote Repositories)

### Add remote

```bash
git remote add origin <url>
```

➡️ Connect local repo to remote repo

### Fetch updates

```bash
git fetch origin
```

➡️ Downloads changes (does NOT merge)

### Pull changes

```bash
git pull
```

➡️ Fetch + merge from remote

### Push changes

```bash
git push origin main
```

➡️ Uploads local commits to remote

---

## 📂 TRACKING PATH CHANGES

### Delete file

```bash
git rm <file>
```

➡️ Removes file and stages deletion

### Rename / move file

```bash
git mv old new
```

➡️ Renames file and stages change

```bash
git log --stat -M
```

➡️ Shows commits including moved/renamed files

---

## 🧳 TEMPORARY COMMITS (Git Stash)

Used when you **want to save work temporarily without committing**.

```bash
git stash
```

➡️ Saves current work temporarily

```bash
git stash list
```

➡️ Shows all stashed entries

```bash
git stash pop
```

➡️ Applies stash and removes it

```bash
git stash drop
```

➡️ Deletes stash permanently

---

## ✍️ REWRITE HISTORY (Advanced)

```bash
git rebase <branch>
```

➡️ Moves commits of current branch on top of another branch

```bash
git reset --hard <commit>
```

➡️ DANGEROUS: Deletes commits and resets working directory

---

## 🔍 INSPECT & COMPARE

```bash
git log branchB..branchA
```

➡️ Commits present in A but not in B

```bash
git diff branchB...branchA
```

➡️ Code difference between branches

```bash
git show <SHA>
```

➡️ Shows details of a specific commit

---

## 🚫 IGNORING FILES (.gitignore)

Prevents unwanted files from being tracked.

Example `.gitignore`:

```
logs/
*.notes
pattern*/
```

```bash
git config --global core.excludesfile <file>
```

➡️ Global ignore rules for all repositories

---

## 🎯 Interview Summary

* **Git** = version control system (local)
* **Commit** = permanent save
* **Stash** = temporary save
* **Branch** = parallel development
* **Remote** = GitHub/GitLab repo

---

✅ This README can be directly used in your **GitHub repo** for revision and interviews.
