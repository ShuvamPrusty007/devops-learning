# Cloud & DevOps Learning – Setup & Command Reference

**Environment:** Windows 11 Pro + WSL 2 + Ubuntu 24.04 LTS + VS Code
**Workspace:** `~/devops-learning`
**Purpose:** Reference notes for installation, configuration and daily practice.

---

# 1. Windows PowerShell – WSL Commands

These commands are executed in **Windows PowerShell**, not Ubuntu.

| Command | Purpose |
|---|---|
| `wsl --status` | Check WSL configuration |
| `wsl --list --verbose` | List installed Linux distributions and WSL versions |
| `wsl -d Ubuntu-24.04` | Launch Ubuntu 24.04 |
| `wsl --set-default Ubuntu-24.04` | Set Ubuntu 24.04 as default |
| `wsl` | Launch the default Linux distribution |
| `wsl --shutdown` | Stop all running WSL distributions |

### Important concepts

- WSL = Windows Subsystem for Linux.
- WSL 2 provides a Linux environment using a real Linux kernel.
- Ubuntu is a Linux distribution.
- `-d` means distribution, not directory.
- Setting Ubuntu as default avoids specifying it every time.

---

# 2. Ubuntu – Initial Setup Commands

These commands are executed inside the **Ubuntu terminal**.

## Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

| Command | Purpose |
|---|---|
| `sudo` | Execute commands with administrator privileges |
| `apt` | Ubuntu package management tool |
| `update` | Refresh package information |
| `upgrade` | Install available package updates |
| `-y` | Automatically confirm prompts |

## Install essential tools

```bash
sudo apt install -y git curl wget unzip zip tree jq vim nano build-essential python3 python3-pip python3-venv
```

| Package | Purpose |
|---|---|
| `git` | Version control |
| `curl` | Transfer data and test APIs |
| `wget` | Download files |
| `unzip` | Extract ZIP files |
| `zip` | Create ZIP archives |
| `tree` | Display directory structure |
| `jq` | Process JSON data |
| `vim` | Terminal text editor |
| `nano` | Beginner-friendly terminal editor |
| `build-essential` | Compiler and build tools |
| `python3` | Python interpreter |
| `python3-pip` | Python package installer |
| `python3-venv` | Python virtual environment support |

---

# 3. Linux Basic Navigation Commands

| Command | Meaning | Example |
|---|---|---|
| `pwd` | Print working directory | `pwd` |
| `whoami` | Display current username | `whoami` |
| `ls` | List files and folders | `ls` |
| `ls -l` | Detailed file listing | `ls -l` |
| `ls -a` | Show hidden files | `ls -a` |
| `cd` | Change directory | `cd python` |
| `cd ..` | Go to parent directory | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd /` | Go to root directory | `cd /` |
| `clear` | Clear terminal screen | `clear` |

## Important Linux directories

| Path | Meaning |
|---|---|
| `/` | Root directory |
| `/home` | Home directories of regular users |
| `/root` | Root user's home directory |
| `/etc` | System configuration files |
| `/var/log` | System and application logs |
| `/tmp` | Temporary files |
| `~` | Current user's home directory |
| `.` | Current directory |
| `..` | Parent directory |

**Remember:** Linux is case-sensitive. `pwd` and `PWD` are not the same.

---

# 4. Creating Your DevOps Workspace

Workspace location:

```text
~/devops-learning
```

## Create workspace

```bash
mkdir devops-learning
cd devops-learning
```

## Create multiple directories

```bash
mkdir -p ~/devops-learning/{linux,networking,git,python,aws,docker,kubernetes,terraform,projects}
```

## Workspace structure

```text
devops-learning/
│
├── linux/
├── networking/
├── git/
├── python/
├── aws/
├── docker/
├── kubernetes/
├── terraform/
└── projects/
```

## Create a file

```bash
touch practice.txt
```

## Check files and folders

```bash
ls -l
```

---

# 5. Linux File and Directory Operations

| Command | Purpose | Example |
|---|---|---|
| `mkdir` | Create directory | `mkdir backup` |
| `touch` | Create empty file | `touch app.log` |
| `cp` | Copy file | `cp app.log backup/` |
| `mv` | Move or rename file | `mv notes.txt linux-notes.txt` |
| `rm` | Delete file | `rm app.log` |
| `cat` | Display file contents | `cat app.log` |
| `less` | Read file page by page | `less app.log` |
| `head` | Display first 10 lines | `head app.log` |
| `tail` | Display last 10 lines | `tail app.log` |
| `file` | Identify file type | `file app.log` |
| `stat` | Display file information | `stat app.log` |
| `tree` | Display directory structure | `tree` |

**Warning:** `rm` normally permanently removes files. Check the filename before executing it.

---

# 6. VS Code + Ubuntu (WSL) Setup

VS Code is installed on Windows, but the WSL extension allows it to work directly with Ubuntu files and commands.

## Open workspace in VS Code

From Ubuntu:

```bash
cd ~/devops-learning
code .
```

### What does `code .` mean?

- `code` = Launch Visual Studio Code.
- `.` = Open the current directory.

## Useful VS Code shortcuts

| Shortcut | Purpose |
|---|---|
| `Ctrl + `` | Open or close integrated terminal |
| `Ctrl + Shift + P` | Open Command Palette |
| `Ctrl + S` | Save file |

## Open Ubuntu folder in Windows File Explorer

```bash
explorer.exe .
```

You can also browse using this Windows Explorer path:

```text
\\wsl.localhost\Ubuntu-24.04\home\<linux-username>\devops-learning
```

---

# 7. Git Installation and Configuration

## Check Git version

```bash
git --version
```

Verified version during setup: `2.43.0`

## Configure Git identity

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

## Check configuration

```bash
git config --global --list
```

| Command | Purpose |
|---|---|
| `git --version` | Check installed Git version |
| `git config` | Configure Git settings |
| `--global` | Apply settings to your Linux user |
| `user.name` | Set commit author name |
| `user.email` | Set commit author email |

**Status:** Git is installed and configured.

---

# 8. GitHub SSH Authentication

SSH authentication allows Ubuntu to connect securely to GitHub without entering your GitHub password for every Git operation.

## What is SSH?

SSH stands for Secure Shell. It is a network protocol used for secure communication and authentication between systems.

## SSH key pair

SSH authentication uses two keys:

| Key | Purpose |
|---|---|
| Private key | Used to prove your identity. Must remain secret. |
| Public key | Shared with services such as GitHub for authentication. |

### How SSH authentication works

1. Generate an SSH key pair.
2. Add the public key to your GitHub account.
3. Keep the private key securely on your machine.
4. SSH uses the key pair to authenticate your connection.

## Test GitHub connection

```bash
ssh -T git@github.com
```

On the first connection, SSH may ask whether you trust GitHub's host key. Verify the host fingerprint against GitHub's published fingerprint before accepting it.

Successful authentication produces a message similar to:

```text
Hi YOUR_GITHUB_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

### Important security rules

- Never share or upload your private key.
- Do not include actual key contents in public notes.
- Add appropriate SSH credential files and folders to `.gitignore`.
- The public key is registered with GitHub for authentication.

**Status:** GitHub SSH authentication successfully tested.

---

# 9. Python Installation and Configuration

Python was installed inside Ubuntu.

Verified versions during setup:

```text
Python 3.12.3
pip 24.0
```

## Check Python version

```bash
python3 --version
```

## Check pip version

```bash
pip3 --version
```

## Navigate to Python workspace

```bash
cd ~/devops-learning/python
```

## Create virtual environment

```bash
python3 -m venv .venv
```

### Meaning

- `python3` = Python interpreter.
- `-m` = Run a Python module.
- `venv` = Virtual environment module.
- `.venv` = Name of the environment folder.

## Activate virtual environment

```bash
source .venv/bin/activate
```

When activated, your terminal displays:

```text
(.venv)
```

## Verify Python interpreter

```bash
python --version
which python
```

Expected interpreter location:

```text
~/devops-learning/python/.venv/bin/python
```

## Deactivate environment

```bash
deactivate
```

### Why use a virtual environment?

It keeps project-specific Python packages separate from the system Python installation and other projects.

Whenever you open a new terminal to work on this Python project:

```bash
cd ~/devops-learning/python
source .venv/bin/activate
```

---

# 10. Git Ignore – Protecting Sensitive Files

A `.gitignore` file tells Git which files and folders should not be tracked.

Create `.gitignore` in the root of the `devops-learning` directory.

## Example `.gitignore`

```gitignore
# SSH credentials
.ssh/
*.pem
*.key

# Python virtual environment
.venv/
venv/

# Environment variables and secrets
.env
.env.*

# Python cache
__pycache__/
*.pyc
```

### Important

- `.gitignore` does not delete files.
- It prevents matching untracked files from being added to Git.
- It does not automatically remove files already committed.
- Always review files before pushing to a public repository.
- Never publish passwords, access tokens, private keys or confidential information.

---

# 11. Important Command Differences

| Command | Environment | Purpose |
|---|---|---|
| `wsl` | PowerShell | Launch default Linux distribution |
| `wsl -d Ubuntu-24.04` | PowerShell | Launch specified distribution |
| `pwd` | Ubuntu | Show current directory |
| `cd` | Ubuntu | Change directory |
| `sudo apt update` | Ubuntu | Refresh package information |
| `code .` | Ubuntu | Open current folder in VS Code |
| `git --version` | Ubuntu | Check Git version |
| `python3 --version` | Ubuntu | Check Python version |
| `ssh -T git@github.com` | Ubuntu | Test GitHub SSH authentication |

---

# 12. Initial Setup Completion Checklist

| Component | Status |
|---|---|
| WSL 2 enabled | Completed |
| Ubuntu 24.04 installed | Completed |
| Ubuntu packages updated | Completed |
| Essential Linux tools installed | Completed |
| Git installed | Completed |
| Git identity configured | Completed |
| GitHub SSH authentication | Completed |
| Python 3 installed | Completed |
| pip installed | Completed |
| Python virtual environment | Created and activated |
| VS Code installed | Completed |
| VS Code WSL integration | Completed |
| DevOps workspace created | Completed |
| First GitHub repository push | Pending |
| First Python script practice | Pending |

---

# 13. What We Will Learn Next

Our roadmap continues in this order:

1. Linux file operations – `mkdir`, `cp`, `mv`, `rm`, `cat`, `head`, `tail`, `less`.
2. Searching files and text – `grep`, `find`, `wc`, `sort`, `uniq`.
3. Linux permissions – `chmod`, `chown`, `rwx`.
4. Processes and resources – `ps`, `top`, `kill`, `df`, `du`, `free`.
5. Services and logs – `systemctl`, `journalctl`.
6. Networking – IP, DNS, TCP/UDP, HTTP/HTTPS and ports.
7. Remote access – SSH and SFTP.
8. Git fundamentals – repository, staging, commits, branches, push and pull.

**My learning rule:** Understand each command, type it yourself in Ubuntu, observe the output and practise a real troubleshooting example. Do not just memorise commands.

---

*This document records my initial Cloud & DevOps learning environment setup. I will maintain separate topic-wise notes as I progress through the roadmap.*