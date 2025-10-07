# Git Tutorial Basics

Git is a **distributed version control system**. Instead of relying on a single central server, each developer **clones** a full copy of the repository (including history) to their machine. The original can live on a self-hosted server or on a service like **GitHub**.

---

## Step 0: Create a GitHub Account
Sign up at: <https://github.com/>

---

## Step 1: Make sure Git is installed
**macOS / Linux / Windows (Git Bash / PowerShell):**
```bash
git --version
```

---

## Step 2: Tell Git who you are
Every commit records your name and email.
```bash
git config --global user.name  "Your_UserName"
git config --global user.email "abc@gmail.com"
git config --global init.defaultBranch main
git config --global --list   # verify settings
```

---

## Step 3: Create a repository (GitHub → New)
Follow GitHub’s guide:  
<https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository>

**Tip:** Create it **empty** (no README/license) to avoid an immediate merge conflict on first push.

---

## Step 4: Initialize a local repo and add files
Navigate to the folder you want under Git, then initialize:
```bash
cd ~/Desktop/your_directory
git init                     # initialize empty Git repo
touch README.md              # create a README file
echo "My first repo" >> README.md
```

Stage and commit:
```bash
git add .
git commit -m "First commit"
```

---

## Step 5: Connect the local repo to GitHub (remote)

### Option 1 — HTTPS with a Personal Access Token (PAT)
1) Generate a token: **GitHub** → Settings → Developer settings → **Personal access tokens (classic)** → *Generate new token* → choose scopes (usually `repo`).  
2) **Set the remote** (replace placeholders with your info):
```bash
git remote add origin https://github.com/<your-username>/<repo-name>.git
```
If you want to bake credentials into the URL (quick demo only; not recommended for shared machines):
```bash
git remote set-url origin https://<your-username>:<your-token>@github.com/<your-username>/<repo-name>.git
```
Then push:
```bash
git branch -M main
git push -u origin main
```

### Option 2 — SSH (recommended once set up)
Generate a key, add it to GitHub, and test (docs: <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/>):
```bash
ssh-keygen -t ed25519 -C "you@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
# copy the public key to GitHub → Settings → SSH and GPG keys
# macOS:   pbcopy < ~/.ssh/id_ed25519.pub
# Windows: clip < ~/.ssh/id_ed25519.pub
# Linux:   xclip -sel clip < ~/.ssh/id_ed25519.pub

ssh -T git@github.com   # should say "Hi <username>!"
git remote add origin git@github.com:<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```

---

## Change the remote URL later (if needed)
```bash
git remote -v                     # verify current
git remote set-url origin <new-url>
```
Examples:
```bash
# switch to HTTPS
git remote set-url origin https://github.com/<your-username>/<repo-name>.git

# switch to SSH
git remote set-url origin git@github.com:<your-username>/<repo-name>.git
```

---

## Clone an existing GitHub repo to your computer
1) On GitHub: click **Code** → copy the URL (HTTPS or SSH).  
2) In your terminal:
```bash
cd /path/to/your/directory
git clone https://github.com/<username>/<repository-name>.git
cd <repository-name>
```

---

## Everyday workflow (edit → stage → commit → push)
```bash
# edit files with your editor

git status                      # see changes
git add -A                      # stage all
git commit -m "Describe what changed"
git push                        # upload commits to GitHub
```

---

## Collaboration Basics
Invite collaborators, use branches, and merge.

**Invite a collaborator (on GitHub):**
- Repo → **Settings** → **Collaborators** → **Add people**.

**Create a branch and work:**
```bash
git checkout -b feature/some-change
# edit files
git add -A
git commit -m "Implement some change"
git push -u origin feature/some-change
```
Open a **Pull Request** on GitHub to merge into `main`.

**Merge (maintainer):**
- Review PR → **Merge pull request** → **Confirm**.

---

## Helpful tips
If you created a README on GitHub **and** locally before your first push:
```bash
git pull --rebase origin main
git push
```
Line endings (cross-platform teams):
```bash
# Windows
git config --global core.autocrlf true

# macOS/Linux
git config --global core.autocrlf input
```

---

### (Handling merge conflict markers)
If you ever see lines like these after a merge:
```
<<<<<<< HEAD
...
=======
...
>>>>>>> 507cc1aa75ab694bc2aa9eabf074e17a2f58124d
```
they indicate a **merge conflict**. Edit the file to keep the correct content, then:
```bash
git add <file>
git commit -m "Resolve merge conflict"
```
