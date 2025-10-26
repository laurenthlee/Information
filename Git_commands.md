# 0) Uploading files to GitHub (Setup once) 

### A) Install + identify yourself

```bash
git --version                 # ensure Git is installed
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

### B) Choose how you authenticate (pick one)

**SSH (recommended)**

1. Make a key:

```bash
# Linux/macOS
ssh-keygen -t ed25519 -C "you@example.com"

# Windows (PowerShell)
ssh-keygen -t ed25519 -C "you@example.com"
```

2. Copy the public key (the file ending in `.pub`) and add it to GitHub → **Settings → SSH and GPG keys**.
3. Test:

```bash
ssh -T git@github.com
```

**HTTPS with a Personal Access Token (PAT)**

* Create a token in GitHub **Settings → Developer settings → Personal access tokens**.
* Give it `repo` scope.
* When Git asks for a password, paste the token (not your GitHub password).

---

# 1) Push a **brand-new local folder** to a new GitHub repo

> First create an empty repo on GitHub (no README/license). Then:

```bash
cd /path/to/your/project

git init -b main
git add -A
git commit -m "chore: initial commit"

# Pick ONE remote style:
# SSH:
git remote add origin git@github.com:<USER>/<REPO>.git
# OR HTTPS:
# git remote add origin https://github.com/<USER>/<REPO>.git

git push -u origin main
```

---

# 2) Upload files to an **existing GitHub repo** you cloned

```bash
# If not cloned yet:
git clone git@github.com:<USER>/<REPO>.git   # or the HTTPS URL
cd <REPO>

# Add/replace files, then:
git add <file-or-folder>     # or: git add -A  (everything changed)
git commit -m "feat: add new files"
git push
```

---

# 3) Upload changes on a **new branch** (then make a Pull Request)

```bash
git checkout -b feat/my-change
# edit files…
git add -A
git commit -m "feat: implement my change"
git push -u origin feat/my-change
# open PR on GitHub (or)
# gh pr create -f   # if you use the GitHub CLI
```

---

# 4) Super common file ops (exact commands)

**Add a single file**

```bash
git add myfile.txt
git commit -m "docs: add myfile"
git push
```

**Rename or move a file**

```bash
git mv old/name.txt docs/new-name.txt
git commit -m "chore: rename file"
git push
```

**Delete a file from the repo**

```bash
git rm path/to/unwanted.txt
git commit -m "chore: remove unwanted file"
git push
```

**Ignore files so they don’t get uploaded**

```bash
echo ".env" >> .gitignore
echo "node_modules/" >> .gitignore
git add .gitignore
git commit -m "chore: add gitignore"
git push
```

---

# 5) Large files (100MB) → use **Git LFS**

GitHub blocks blobs over 100MB unless you use LFS.

```bash
# one-time
git lfs install

# start tracking your large types
git lfs track "*.bin"
git add .gitattributes

# add your large file(s)
git add big-model.bin
git commit -m "chore: track big assets via LFS"
git push
```

---

# 6) Using the **GitHub web UI** to upload quickly (no Git needed)

1. Open your repo on github.com
2. Click **Add file → Upload files**
3. Drag files → **Commit changes** (optionally create a new branch + PR)

Good for tiny edits; prefer Git for real work.

---

# 7) Using the **GitHub CLI** (`gh`) end-to-end

```bash
# install gh (see GitHub docs), then:
gh auth login            # choose GitHub.com, SSH or HTTPS+token

# Create a repo on GitHub and push your local folder in one go:
gh repo create <USER>/<REPO> --public --source=. --remote=origin --push
```

---

# 8) Keep your fork in sync (if you forked a repo)

```bash
# Add upstream (the original repo) once:
git remote add upstream git@github.com:<ORIGINAL_OWNER>/<REPO>.git

# Update your main
git fetch upstream
git checkout main
git rebase upstream/main        # or: git merge upstream/main
git push
```

---

# 9) Typical errors & exact fixes

**“origin already exists”**

```bash
git remote set-url origin git@github.com:<USER>/<REPO>.git
```

**“Updates were rejected because the remote contains work that you do not have”**

```bash
git fetch origin
git rebase origin/main          # keep linear history
# (resolve conflicts if any, then:)
git push --force-with-lease
```

**Accidentally committed to the wrong branch**

```bash
git checkout -b feat/fixup      # create a new branch from current state
git checkout main
git reset --hard origin/main    # reset main to remote
# work now continues on feat/fixup
```

**Undo the last commit but keep changes staged/unstaged**

```bash
git reset --soft HEAD~1   # keep changes staged
# or
git reset --mixed HEAD~1  # keep changes unstaged
```

**Remove a file that’s already tracked and keep it locally**

```bash
git rm --cached secrets.json
echo "secrets.json" >> .gitignore
git commit -m "chore: stop tracking secrets.json"
git push
```

**Wrong default branch name**

```bash
git branch -M main
git push -u origin main
```

**Credential prompt every push (HTTPS)**
Use a token & cache it:

```bash
git config --global credential.helper store
# Next time you push, paste the PAT; it’ll be saved locally.
```

---

# 10) Mini “decision tree” (what should I run?)

* **New folder → new GitHub repo:** use section **1**.
* **Repo exists on GitHub → you want to add files:** clone then section **2**.
* **Want a PR workflow:** section **3**.
* **Big binaries:** section **5** (Git LFS).
* **No Git installed / small change:** section **6** (web UI).
* **Prefer commands that also create the repo for you:** section **7** (GitHub CLI).

---
