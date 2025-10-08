# Git Tutorial Basics

Git is a **distributed version control system**. Instead of relying on a single central server, each developer **clones** a full copy of the repository (including history) to their local machine.
The original repository can live on a self-hosted server or a hosting service like **GitHub**.

---

## Step 0: Create a GitHub Account

Sign up at: [https://github.com/](https://github.com/)

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
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global --list   # verify settings
```

---

## Step 3: Create a repository (GitHub → New)

Follow GitHub’s guide:
[https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)

**Tip:** Create it **empty** (no README/license) to avoid merge conflicts on your first push.

---

## Step 4: Initialize a local repo and add files

Navigate to the folder you want under Git, then initialize:

```bash
cd ~/Desktop/your_directory
git init
touch README.md
echo "My first repo" >> README.md
```

Stage and commit:

```bash
git add .
git commit -m "First commit"
```

---

## Step 5: Connect your local repo to GitHub

> ⚠️ **Important:**
> GitHub **no longer supports password authentication** for Git operations.
> You must use either a **Personal Access Token (PAT)** or **SSH key**.

---

### Option 1 — HTTPS with a Personal Access Token (PAT)

1. **Generate a Personal Access Token (PAT):**

   * Go to: [https://github.com/settings/tokens](https://github.com/settings/tokens)
   * Choose **Fine-grained token** (recommended) or **Classic token**
   * Scope: allow **repo** (read and write)

2. **Set the remote:**

   ```bash
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   ```

3. **Push your local commits:**

   ```bash
   git branch -M main
   git push -u origin main
   ```

4. When prompted:

   * **Username:** your GitHub username (e.g., `sudipdhakal1`)
   * **Password:** your **PAT** (paste the token here; you won’t see it on screen)

5. (Optional) To store your token securely on macOS:

   ```bash
   git config --global credential.helper osxkeychain
   ```

> 🔒 **Never paste your token directly into commands or share it.**
> If you ever expose your token, **revoke it immediately** in GitHub settings and generate a new one.

---

### Option 2 — SSH (recommended for long-term use)

Generate an SSH key and add it to GitHub:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Copy the public key to GitHub:

```bash
pbcopy < ~/.ssh/id_ed25519.pub  # macOS
# or clip < ~/.ssh/id_ed25519.pub (Windows)
```

Then set your remote and push:

```bash
git remote add origin git@github.com:<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```

Test connection:

```bash
ssh -T git@github.com
```

If successful, you’ll see:

```
Hi <username>! You've successfully authenticated.
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

## Clone an existing GitHub repo

On GitHub, click **Code** → copy the HTTPS or SSH URL, then:

```bash
cd /path/to/your/folder
git clone https://github.com/<username>/<repository-name>.git
cd <repository-name>
```

---

## Everyday workflow (edit → stage → commit → push)

```bash
git status
git add -A
git commit -m "Describe what changed"
git push
```

---

## Collaboration Basics

**Invite a collaborator:**

* Go to your repo → **Settings → Collaborators → Add people**

**Work on a branch:**

```bash
git checkout -b feature/update-readme
git add .
git commit -m "Updated README with secure token instructions"
git push -u origin feature/update-readme
```

Then open a **Pull Request** on GitHub to merge changes into `main`.

---

## Helpful tips

If your local and remote branches differ (e.g., GitHub created a README online):

```bash
git pull --rebase origin main
git push
```

Handle line endings:

```bash
# Windows
git config --global core.autocrlf true

# macOS/Linux
git config --global core.autocrlf input
```

---

## Handling Merge Conflicts

If you see conflict markers like:

```
<<<<<<< HEAD
...
=======
...
>>>>>>> some-commit-hash
```

Edit to keep the correct content, then:

```bash
git add <file>
git commit -m "Resolve merge conflict"
```

