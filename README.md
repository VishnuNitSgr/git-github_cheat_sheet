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


# PUT Pre-Signed URL – Metadata to Direct S3 Upload (Revision Guide)

This README explains **exactly how PUT pre-signed URLs work**, focusing on **metadata flow**, **server responsibility**, and **direct upload to S3**. Perfect for **revision + interviews**.

---

## 1️⃣ Core Idea (One Line)

> Client server ko **file metadata** deta hai, server us metadata ke basis par **PUT pre-signed URL** generate karta hai, aur client us URL ke through **direct S3 me file upload** karta hai.

---

## 2️⃣ Why This Flow Is Used

* S3 bucket private rehta hai
* AWS credentials expose nahi hote
* Backend par load nahi aata
* Upload fast aur scalable hota hai

---

## 3️⃣ Step-by-Step Flow (Clear Mental Model)

```
Client ──(metadata)──▶ Server
Server ──(pre-signed URL)──▶ Client
Client ──(actual file)──▶ S3
```

---

## 4️⃣ Step 1: Client → Server (Metadata Only)

Client server ko **sirf file ki details** bhejta hai:

* File name
* Content-Type
* (Optional) file size
* (Optional) custom metadata

Example request:

```json
{
  "fileName": "photo.png",
  "contentType": "image/png"
}
```

⚠️ **Important:**
Client **actual file server ko upload nahi karta**.

---

## 5️⃣ Step 2: Server Generates PUT Pre-Signed URL

Server (Node.js) S3 ke liye ek `PutObjectCommand` banata hai:

```js
import { S3Client, PutObjectCommand } from "@aws-sdk/client-s3";
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";

const s3 = new S3Client({ region: "ap-south-1" });

const command = new PutObjectCommand({
  Bucket: "my-private-bucket",
  Key: "uploads/photo.png",
  ContentType: "image/png"
});

const uploadUrl = await getSignedUrl(s3, command, {
  expiresIn: 300 // 5 minutes
});
```

👉 Server ka role **sirf URL generate karna** hai.

---

## 6️⃣ Step 3: Server → Client (Pre-Signed URL)

Server client ko response me deta hai:

```json
{
  "uploadUrl": "https://s3.amazonaws.com/..."
}
```

URL:

* Temporary hota hai
* Sirf **PUT method** allow karta hai
* Expiry ke baad invalid ho jata hai

---

## 7️⃣ Step 4: Client → S3 (Direct Upload)

Client is URL ka use karke file upload karta hai:

```js
await fetch(uploadUrl, {
  method: "PUT",
  headers: {
    "Content-Type": "image/png"
  },
  body: file
});
```

✅ Upload **direct S3 me hota hai**
❌ Backend completely bypass ho jata hai

---

## 8️⃣ Very Important Rule ⚠️ (Exam + Interview Favorite)

> **Jo metadata server ne pre-signed URL banate time diya hota hai, wahi metadata upload ke time match hona chahiye.**

Mismatch hua to error aayega:

* `SignatureDoesNotMatch`
* `403 Forbidden`

Example mismatch:

* Server: `image/png`
* Client: `image/jpeg`

❌ Upload fail

---

## 9️⃣ What Node.js Does vs What It Does NOT

### Node.js DOES:

* File metadata receive karta hai
* Pre-signed URL generate karta hai
* Security & expiry control karta hai

### Node.js DOES NOT:

* Actual file upload
* File streaming
* File storage

---

## 🔍 Common Misconception (Clear It Once)

❌ "Node.js S3 me upload karta hai"
✅ **Client S3 me upload karta hai using pre-signed URL**

---

## 10️⃣ Interview One-Liners 💼

* Pre-signed URL backend se generate hota hai
* Client metadata bhejta hai, file nahi
* Upload direct S3 me hota hai
* Metadata mismatch se signature error aata hai

---

## 11️⃣ Quick Revision Summary 🚀

* PUT pre-signed URL = secure upload
* Server only signs request
* Client uploads directly to S3
* Metadata must match exactly

---

📌 **If you understand this README, you fully understand PUT pre-signed URLs in S3** 💯
