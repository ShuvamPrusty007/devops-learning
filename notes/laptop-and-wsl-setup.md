# Laptop and WSL Setup

## 1. Development Environment

| Component | Configuration |
|---|---|
| Laptop | Dell Latitude 5440 |
| Processor | Intel Core i5-1345U, 13th Gen |
| RAM | 24 GB |
| Operating System | Windows 11 Pro, 64-bit |
| Linux Environment | Ubuntu 24.04 LTS |
| WSL Version | WSL 2 |
| Code Editor | Visual Studio Code |
| Terminal | Ubuntu Bash |

## 2. WSL and Ubuntu

Windows Subsystem for Linux (WSL) allows Linux distributions to run directly on Windows.

Ubuntu 24.04 LTS is installed using WSL 2.

### Verify installation

```powershell
wsl --list --verbose
```

### Launch Ubuntu

```powershell
wsl
```

### Open the Ubuntu terminal

Launch Ubuntu from the Windows Start menu.

## 3. VS Code Integration

VS Code is connected to Ubuntu through the WSL extension.

Open the learning workspace:

```bash
cd ~/devops-learning
code .
```

Verify that VS Code displays:

`devops-learning [WSL: Ubuntu-24.04]`

The integrated terminal should use Ubuntu Bash.

## 4. Essential Linux Packages

```bash
sudo apt update
sudo apt upgrade -y

sudo apt install -y git curl wget unzip zip tree jq vim nano build-essential python3 python3-pip python3-venv
```

## 5. Git Configuration

Git is installed and configured with the user's name and email.

Verify:

```bash
git --version
git config --global user.name
git config --global user.email
```

GitHub SSH authentication has been configured and successfully tested.

```bash
ssh -T git@github.com
```

## 6. Python Environment

Python 3 and pip are installed.

A virtual environment is configured inside the Python learning directory.

```bash
cd ~/devops-learning/python

python3 -m venv .venv
source .venv/bin/activate
```

Verify:

```bash
python --version
which python
```

Deactivate:

```bash
deactivate
```

## 7. Learning Workspace

```text
devops-learning/
├── notes/
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

## 8. Windows File Explorer Access

To open the current Ubuntu directory in Windows File Explorer:

```bash
explorer.exe .
```

The Ubuntu workspace can also be accessed through:

`\\wsl.localhost\Ubuntu-24.04\home\shuvamprusty\devops-learning`

## 9. Security Practices

- Never publish private SSH keys.
- Never commit passwords, tokens or API credentials.
- Use `.gitignore` to exclude virtual environments and sensitive files.
- Keep personal information out of public learning notes.
- Use separate credentials for different environments where appropriate.

---

**Document purpose:** Reference for the local Cloud and DevOps development environment.