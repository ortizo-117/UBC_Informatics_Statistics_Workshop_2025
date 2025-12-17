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
ssh-keygen -t ed25519 -C "oscar.ortiz.angulo@gmail.com"
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
