# UBC_Informatics_Statistics_Workshop_2025
A repository for the UBC Bioinformatics &amp; Statistics Workshop – Session 6 (Dec 17th) 

# Overview
---

# 1. Prerequisites

You need the following **on each computer** (Windows and/or macOS):

1. **R installed**
   - https://cran.r-project.org
2. **RStudio Desktop installed**
   - https://posit.co/download/rstudio-desktop/
3. **A GitHub account**
   - https://github.com
4. **Git installed and visible to RStudio** (critical)
5. **Terminal access** (RStudio has a built-in Terminal)
6. **Internet access** (for GitHub authentication)

---

# 2. Install Git and Make It Visible to RStudio

## 2.1 Windows

### Install Git

1. Download Git for Windows:
   - https://git-scm.com/download/win
   - I will be using the x64 Setup option
2. Run the installer.
3. Important installer options:
   - ✅ *Use Git from the command line and also from 3rd-party software*
   - ✅ Default branch name: `main`
   - ✅ HTTPS backend: OpenSSL
   - ✅ Line endings: Checkout Windows-style, commit Unix-style

### Restart RStudio

RStudio must be restarted to detect Git.

### Verify Git is visible to RStudio
You should now have a "Git" tab in your environment. 

In the **RStudio Console**:

```{r}
Sys.which("git")
```

Expected output (example):

```
"C:/Program Files/Git/bin/git.exe"
```

### If Git is installed but NOT detected

1. RStudio → **Tools → Global Options → Git/SVN**
2. Set **Git executable** to:

```
C:/Program Files/Git/bin/git.exe
```

3. Restart RStudio.

---

## 2.2 macOS

### Option A: Xcode Command Line Tools

Open Terminal:

```{bash}
xcode-select --install
```

### Option B: Git installer (I recomend this - seems a bit easier)

- https://git-scm.com/download/mac

### Restart RStudio

### Verify Git is visible to RStudio

In the **RStudio Console**:

```{r}
Sys.which("git")
```

Expected output (one of):

```
/usr/bin/git
/usr/local/bin/git
/opt/homebrew/bin/git
```

### If Git is NOT detected

1. RStudio → **Preferences → Git/SVN**
2. Set **Git executable** manually (try one of the paths above)
3. Restart RStudio

---

# 3. Generate an SSH Key

SSH keys allow secure, password-free authentication with GitHub.

## 3.1 Open a Terminal

- In RStudio: **Tools → Terminal → New Terminal**

## 3.2 Create the SSH key

Replace the email with your GitHub email address:

```{bash}
ssh-keygen -t ed25519 -C "youremail@gmail.com"
```

- Press **Enter** to accept the default location
- Optional but recommended: set a passphrase

This creates:

- Private key: `~/.ssh/id_ed25519` (keep secret)
- Public key: `~/.ssh/id_ed25519.pub`

---

# 4. Start SSH Agent and Add Key

## macOS

```{bash}
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## Windows (RStudio Terminal or Git Bash)

```{bash}
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
#Note - You need to run these commands on a bash terminal. In windows, the prompt should look like " username@computer ~" or have the MINGw64 tag. If you are getting errors like "eval" is not recognized...etc, it likely is that you are not on a bash terminal. You can go to Tools → Global Options → Terminal and set Shell to GitBash and then open a new terminal by going to  Tools → Terminal → New Terminal.

The SSH agent securely manages your key in memory.
---

# 5. Add the SSH Key to GitHub

## Copy the public key

```{bash}
cat ~/.ssh/id_ed25519.pub
```

## Add it on GitHub

1. GitHub → **Settings → SSH and GPG keys**
2. Click **New SSH key**
3. Paste the key
4. Give it a name (e.g., *RStudio Windows* or *RStudio Mac*)
5. Save

---

# 6. Test SSH Connection

```{bash}
ssh -T git@github.com
```

Expected message:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

If you see this, SSH is working correctly.

---

# 7. Use SSH with RStudio Projects

## Option A: Clone a Repository via SSH (recommended)

1. On GitHub, open your repository
2. Click **Code → SSH**
3. Copy the URL:

```
git@github.com:username/repository.git
```

4. In RStudio:
   - **File → New Project → Version Control → Git**
   - Paste the SSH URL
   - Create Project

---

## Option B: Convert an Existing Project from HTTPS to SSH

In the project terminal:

### 1) Check current remote

```{bash}
git remote -v
```

### 2) Change HTTPS to SSH

```{bash}
git remote set-url origin git@github.com:username/repository.git
```

### 3) Test pull/push

```{bash}
git pull
git push
```

---

# 8. Daily Workflow in RStudio

Use the **Git** tab:

1. Stage changes
2. Commit with a message
3. Push to GitHub

No terminal required for daily use.

---

# 9. Common Problems and Fixes

## Git tab does not appear

- You must be inside an **RStudio Project**
- Tools → Project Options → Git/SVN → Enable Git
- Restart RStudio

## `Sys.which("git")` returns empty

- Git not installed correctly
- RStudio not restarted
- Manually set Git executable path (see Sections 2.1 / 2.2)

## SSH error: `Permission denied (publickey)`

Try adding the key again:

```{bash}
ssh-add ~/.ssh/id_ed25519
```

Then retest:

```{bash}
ssh -T git@github.com
```

## Using multiple computers (Windows + Mac)

- Create **one SSH key per computer**
- Add **each public key** to the same GitHub account
- Do **not** copy `.ssh` folders between machines

---

# 10. Best Practices

- Always clone using **SSH URLs**, not HTTPS
- Use the same email in Git config as GitHub
- Commit often with clear messages
- Never share private SSH keys

---

# 11. Final Verification Checklist

- `Sys.which("git")` returns a valid path
- `ssh -T git@github.com` succeeds
- Git tab appears in RStudio
- Push and pull work without passwords

---

You are now fully set up to use **RStudio + GitHub via SSH** on Windows and macOS.


# 12. Git Command Cheat Sheet

This section lists the **most common Git commands** you’ll use in RStudio projects. All commands are run in the **Terminal (Git Bash / Bash)** from inside your project directory.

---

## Repository setup & status

```bash
git status
```
Shows which files are modified, staged, or untracked.

```bash
git log --oneline --graph --decorate
```
Shows a compact commit history.

---

## Add (stage) changes

```bash
git add file_name.R
```
Stages a single file.

```bash
git add .
```
Stages **all** changed files in the project.

---

## Commit changes

```bash
git commit -m "Short, clear commit message"
```
Creates a commit with a message describing *what* changed and *why*.

Best practice:
- Commit often
- One logical change per commit

---

## Sync with GitHub

```bash
git pull
```
Downloads changes from GitHub and merges them into your local branch.

```bash
git push
```
Uploads your local commits to GitHub.

---

## First push (new repository)

```bash
git push -u origin main
```
Pushes your branch and sets the upstream so future `git push` works without arguments.

---

## Branching (basic)

```bash
git branch
```
Lists branches.

```bash
git checkout -b new-branch-name
```
Creates and switches to a new branch.

```bash
git checkout main
```
Switches back to the main branch.

---

## Remote repositories

```bash
git remote -v
```
Shows the GitHub remote URLs.

```bash
git remote set-url origin git@github.com:username/repository.git
```
Switches a repo from HTTPS to SSH.

---

## Undo common mistakes

```bash
git restore file_name.R
```
Discards local changes to a file (not committed).

```bash
git reset HEAD file_name.R
```
Unstages a file (keeps changes).

```bash
git commit --amend
```
Edits the last commit message or adds forgotten files.

---

## SSH troubleshooting

```bash
ssh-add -l
```
Lists SSH keys currently loaded in the agent.

```bash
ssh -T git@github.com
```
Tests GitHub SSH authentication.

---

## RStudio shortcut (recommended for beginners)

Most of these commands can also be done via:

- **Git tab → Stage → Commit → Push**

The command line and Git tab are fully equivalent.

---

# 12. Final Verification Checklist

- `Sys.which("git")` returns a valid path
- `ssh -T git@github.com` succeeds
- Git tab appears in RStudio
- `git pull` and `git push` work without passwords

---

# 13. Hands-On Tutorial: Hello World with GitHub + RStudio

This section is designed as a **guided, end-to-end exercise**. By the end, you will have:

- Created your own GitHub repository
- Cloned it to your computer using SSH
- Made changes locally
- Committed those changes
- Pushed them back to GitHub

This mirrors a typical real-world workflow.

---

## Step 1: Create a Repository on GitHub

1. Log in to your **GitHub account**
2. Click **New repository**
3. Set the following options:
   - **Repository name:** `hello_world`
   - **Visibility:** Public
   - ✅ Check **Add a README file**
4. Click **Create repository**

You now have a GitHub repository with one file: `README.md`.

---

## Step 2: Copy the SSH Clone URL

1. In your `hello_world` repository on GitHub
2. Click **Code → SSH**
3. Copy the URL, which should look like:

```
git@github.com:your-username/hello_world.git
```

---

## Step 3: Clone the Repository in RStudio

1. Open **RStudio**
2. Go to **File → New Project → Version Control → Git**
3. Paste the SSH URL
4. Choose a local folder
5. Click **Create Project**

RStudio will:
- Clone the repository
- Create an RStudio Project
- Open it automatically

---

## Step 4: Explore the Project Structure

In the **Files** pane, you should see:

- `hello_world.Rproj`
- `README.md`
- A hidden `.git/` directory (managed by Git)

Open `README.md` to view its contents.

---

## Step 5: Edit the README File

1. Open `README.md` in RStudio
2. Replace its contents with something like:

```markdown
# Hello World

This is my first GitHub repository connected to RStudio.

This change was made locally and pushed to GitHub.
```

3. Save the file

---

## Step 6: Check Git Status

Open the **Git** tab in RStudio.

You should see:
- `README.md` listed as **modified**

This means Git has detected your change.

---

## Step 7: Commit the Change

Using the **Git tab**:

1. Check the box next to `README.md`
2. Click **Commit**
3. Enter a commit message, for example:

```
Update README with hello world message
```

4. Click **Commit**

A commit is a snapshot of your project at this point in time.

---

## Step 8: Push to GitHub (main branch)

Still in the Git pane:

1. Click **Push**

Because SSH is set up correctly:
- No password is required
- The commit is sent to GitHub

---

## Step 9: Verify on GitHub

1. Return to your repository on GitHub in a browser
2. Refresh the page
3. Confirm that the README now shows your updated text

Congratulations 🎉 — you have completed a full Git workflow.

---

## Key Concepts Reinforced by This Exercise

- Git tracks **local changes**
- Commits are **local** until pushed
- GitHub is a **remote backup and collaboration platform**
- SSH handles authentication silently
- RStudio integrates Git into your daily workflow

---

## Common Issues During the Tutorial

- **Push fails:** check that your remote uses SSH (`git remote -v`)
- **No Git tab:** ensure you are inside an RStudio Project
- **Authentication errors:** re-test with `ssh -T git@github.com`


You are now fully set up to use **RStudio + GitHub via SSH** on Windows and macOS.

