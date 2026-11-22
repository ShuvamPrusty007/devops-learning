# Linux Command Reference – Cloud & DevOps Learning

**Version:** 1.1
**Environment:** Ubuntu 24.04 LTS on WSL 2
**Purpose:** Linux learning, command revision, practical troubleshooting and Cloud/DevOps interview preparation.

---

## Table of Contents

1. Linux Fundamentals
2. Navigation and Directory Structure
3. Creating Files and Directories
4. Copying, Moving and Deleting
5. Viewing and Inspecting Files
6. Searching Text with grep
7. Finding Files with find
8. Pipes and Output Redirection
9. Counting, Sorting and Removing Duplicates
10. Linux Permissions
11. Users and Groups
12. sudo and Root User
13. Process Management
14. Disk, Memory and System Resources
15. Services and systemd
16. System Logs with journalctl
17. Production Support Troubleshooting
18. wget and curl
19. Archives and Compression
20. Text Processing with AWK
21. Text Editing with SED
22. Background Jobs and Process Control
23. Interactive Monitoring with htop
24. Quick Command Revision
25. Topics Reserved for Version 2.0

---

# 1. Linux Fundamentals

### What is Linux?

Linux is an open-source, Unix-like operating system widely used in servers, cloud infrastructure, containers and production environments.

Linux is important for Cloud and DevOps engineers because many application servers, cloud virtual machines and container environments run on Linux.

### Linux kernel

The kernel is the core of the operating system. It manages system resources and provides the interface between software and hardware.

Its responsibilities include:

- Process and CPU scheduling.
- Memory management.
- Device and hardware management.
- Filesystem and storage management.

Check the kernel version:

```bash
uname -r
```

### Shell and Bash

A shell is a command interpreter. It accepts commands, interprets them and requests the operating system to perform operations.

Bash (Bourne Again Shell) is a commonly used shell. It supports interactive commands and shell scripts.

Check the current shell:

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

### Terminal

A terminal is the application or interface in which you type commands and view their output.

Remember:

- Terminal – where you enter commands.
- Bash – the shell that interprets commands.
- Kernel – the core that manages system operations and resources.

### Linux distributions

A Linux distribution is an operating system built around the Linux kernel and bundled with system utilities, package management and other software.

Examples:

- Ubuntu
- Debian
- Red Hat Enterprise Linux (RHEL)
- Fedora
- Kali Linux

Check distribution information:

```bash
cat /etc/os-release
```

### Linux is case-sensitive

Linux commands, filenames and paths are case-sensitive.

For example:

```bash
application.log
Application.log
APPLICATION.LOG
```

These can be three different filenames.

---

# 2. Navigation and Directory Structure

### Understand your current location

```bash
pwd
```

Displays the present working directory.

```bash
whoami
```

Displays the current username.

### List files and directories

```bash
ls
ls -l
ls -a
ls -la
```

| Command | Purpose |
|---|---|
| `ls` | Lists files and directories |
| `ls -l` | Displays detailed information |
| `ls -a` | Includes hidden files |
| `ls -la` | Detailed listing including hidden files |

In `ls -l`, you can see permissions, owner, group, file size and modification date.

### Navigate between directories

```bash
cd ~
cd /
cd ..
cd linux
cd ~/devops-learning/linux
```

| Command | Meaning |
|---|---|
| `cd ~` | Go to the current user's home directory |
| `cd /` | Go to the root directory |
| `cd ..` | Go to the parent directory |
| `cd directory` | Enter a directory |
| `cd path` | Navigate to a specified path |

### Important path symbols

| Symbol | Meaning |
|---|---|
| `/` | Root directory or path separator |
| `~` | Current user's home directory |
| `.` | Current directory |
| `..` | Parent directory |

Example:

If your current directory is:

```text
/home/user/devops-learning/linux/lab1
```

Then:

```bash
cd ..
```

takes you to:

```text
/home/user/devops-learning/linux
```

### Absolute and relative paths

**Absolute path:** Starts from the root directory.

```bash
/home/user/devops-learning/linux/lab1/application.log
```

**Relative path:** Starts from the current directory.

```bash
lab1/application.log
```

---

# 3. Creating Files and Directories

### Create a directory

```bash
mkdir test
```

Create multiple directories:

```bash
mkdir test1 test2 test3
```

Create nested directories:

```bash
mkdir -p parent/child/backup
```

The `-p` option creates parent directories when required.

### Create files

```bash
touch application.log
touch error.log
touch practice.txt
```

`touch` creates an empty file if it does not exist. If the file already exists, it updates its timestamps.

### Create a file with content

```bash
echo "Application started successfully" > application.log
```

The `>` operator writes the output into the file, replacing its previous content.

Append content:

```bash
echo "Database connection successful" >> application.log
```

The `>>` operator appends content without replacing existing content.

### Create a file using a heredoc

```bash
cat > application.log <<EOF
2026-11-21 09:00:00 INFO Application started
2026-11-21 09:05:00 INFO Database connected
2026-11-21 09:10:00 ERROR Database connection timeout
EOF
```

This creates the file with multiple lines.

---

# 4. Copying, Moving and Deleting Files

### Copy files

```bash
cp source.txt destination.txt
cp application.log backup/
cp *.log backup/
cp -r folder1 backup/
```

| Command | Purpose |
|---|---|
| `cp` | Copies files |
| `cp -r` | Copies directories recursively |
| `*.log` | Matches files ending in `.log` |

Example:

```bash
cp lab1/application.log backup/
```

Copies `application.log` into the `backup` directory.

### Move or rename files

```bash
mv oldname.txt newname.txt
mv application.log backup/
```

Difference:

- `cp` creates a copy.
- `mv` moves the original file or renames it.

### Remove files

```bash
rm filename.txt
rm -i temp.log
```

`-i` asks for confirmation before deleting.

### Remove directories

```bash
rmdir old-logs/
rm -r foldername/
```

- `rmdir` removes an empty directory.
- `rm -r` removes a directory and its contents recursively.

**Safety:** Be careful with recursive deletion. Never use destructive commands on production paths without verification and approval. Practise only with disposable test files. Avoid casually using `rm -rf`.

---

# 5. Viewing and Inspecting Files

### cat

Displays file contents:

```bash
cat application.log
```

Useful for small files.

### less

Displays large files page by page:

```bash
less application.log
```

Useful keys inside `less`:

| Key | Purpose |
|---|---|
| `/ERROR` | Search for ERROR |
| `n` | Next match |
| `N` | Previous match |
| `Space` | Next page |
| `q` | Quit |

Note: `/ERROR` is entered inside the `less` interface, not directly as a shell command.

### head

Displays the first lines:

```bash
head application.log
head -n 5 application.log
```

### tail

Displays the last lines:

```bash
tail application.log
tail -n 20 application.log
```

Follow new log entries in real time:

```bash
tail -f application.log
```

Display the last 10 lines and continue following:

```bash
tail -n 10 -f application.log
```

Press `Ctrl + C` to stop following.

### file

Identifies the type of a file:

```bash
file application.log
file index.html
file script.sh
```

### stat

Displays detailed file metadata:

```bash
stat application.log
```

Information includes file size, permissions, inode, access time and modification time.

### tree

Displays directories in a tree format:

```bash
tree
tree -L 2
```

`-L 2` limits the displayed directory depth.

---

# 6. Searching Text with grep

`grep` searches for matching text or patterns inside files.

### Basic search

```bash
grep "ERROR" application.log
```

Displays lines containing `ERROR`.

### Case-insensitive search

```bash
grep -i "error" application.log
```

Matches ERROR, Error, error and other case variations.

### Display line numbers

```bash
grep -n "ERROR" application.log
```

### Combine options

```bash
grep -in "error" application.log
```

Displays matching lines with line numbers, ignoring case.

### Search in multiple files

```bash
grep -i "ERROR" *.log
```

### Exclude matching lines

```bash
grep -v "INFO" application.log
```

Displays lines that do not contain `INFO`.

### Count matching lines

```bash
grep -c "ERROR" application.log
```

Important: `grep -c` counts matching lines, not the total number of occurrences.

### Count occurrences

```bash
grep -io "ERROR" application.log | wc -l
```

This counts individual occurrences of ERROR.

### Search recursively

```bash
grep -r "ERROR" .
```

Searches files under the current directory.

### Real-time error monitoring

```bash
tail -f application.log | grep --line-buffered -i "error"
```

Displays new lines containing ERROR as the file grows.

---

# 7. Finding Files with find

`find` searches for files and directories based on name, type, size, modification time and other conditions.

### Find by filename

```bash
find . -name "application.log"
```

### Case-insensitive filename search

```bash
find . -iname "application.log"
```

### Find all log files

```bash
find . -type f -name "*.log"
```

### Find directories

```bash
find . -type d
```

### Find files larger than 1 GB

```bash
find /var/log -type f -size +1G
```

### Find log files larger than 500 MB

```bash
find /var/log -type f -name "*.log" -size +500M
```

### Find files modified within the last seven 24-hour periods

```bash
find /var/log -type f -name "*.log" -mtime -7
```

### Find files between 5 GB and 10 GB

```bash
find /path/to/directory -type f -size +5G -size -10G
```

### Display file sizes

```bash
find /var/log -type f -size +1G -exec ls -lh {} \;
```

Explanation:

- `-type f` – Search for regular files.
- `-size +1G` – Files larger than 1 GB.
- `-exec` – Execute a command for each matching result.
- `{}` – Represents the matching file.
- `\;` – Ends the `-exec` command.

### Safely identify large files before deletion

First list matching files:

```bash
find /var/log -type f -size +1G -exec ls -lh {} \;
```

Only after confirming the exact files, appropriate ownership and retention requirements should deletion be considered.

Do not automatically delete production logs simply because they are large. They may be required for troubleshooting, audits or compliance.

---

# 8. Pipes and Output Redirection

### Pipe operator `|`

A pipe sends the output of one command as input to another command.

Example:

```bash
ps -ef | grep java
```

Execution:

1. `ps -ef` displays processes.
2. `|` passes the output to the next command.
3. `grep java` filters lines containing java.

Another example:

```bash
grep -i "ERROR" application.log | wc -l
```

Counts the number of lines containing ERROR.

### Output redirection

| Operator | Meaning |
|---|---|
| `>` | Redirect output and overwrite the destination |
| `>>` | Append output to the destination |
| `2>` | Redirect standard error |
| `2>>` | Append standard error |
| `2>&1` | Redirect standard error to standard output |

Examples:

```bash
echo "First entry" > output.txt
echo "Second entry" >> output.txt
```

Redirect errors:

```bash
find /root -type f 2> errors.txt
```

Redirect standard output and errors together:

```bash
command > output.txt 2>&1
```

---

# 9. Counting, Sorting and Removing Duplicates

### wc – Word Count

| Command | Purpose |
|---|---|
| `wc -l file` | Count lines |
| `wc -w file` | Count words |
| `wc -c file` | Count bytes |
| `wc file` | Show lines, words and bytes |

Examples:

```bash
wc -l application.log
wc -w application.log
wc -c application.log
```

### sort

Sorts lines alphabetically by default:

```bash
sort application.log
```

Reverse order:

```bash
sort -r application.log
```

Save sorted output:

```bash
sort application.log > sorted.log
```

Numeric sorting:

```bash
sort -n numbers.txt
```

Sort does not modify the original file unless you explicitly redirect or overwrite it.

### uniq

Removes adjacent duplicate lines:

```bash
uniq duplicate.log
```

To remove duplicates that are not adjacent:

```bash
sort duplicate.log | uniq
```

Count duplicates:

```bash
sort duplicate.log | uniq -c
```

Important: `uniq` only compares adjacent lines. Sorting first groups identical lines together.

---

# 10. Linux Permissions

Linux permissions control who can read, write or execute files and directories.

### Understand ls -l output

Example:

```text
-rw-r--r-- 1 user user 1200 Nov 21 application.log
```

The permission section is:

```text
-rw-r--r--
```

| Part | Meaning |
|---|---|
| `-` | Regular file |
| `rw-` | Owner permissions |
| `r--` | Group permissions |
| `r--` | Others permissions |

### Permission types

| Permission | Symbol | Numeric value |
|---|---|---:|
| Read | `r` | 4 |
| Write | `w` | 2 |
| Execute | `x` | 1 |

### Common permissions

| Permission | Meaning |
|---|---|
| `644` | Owner can read/write; group and others can read |
| `755` | Owner can read/write/execute; group and others can read/execute |
| `750` | Owner has full access; group can read/execute; others have no access |

### Change permissions using chmod

```bash
chmod 644 application.log
chmod 755 script.sh
chmod 750 foldername
```

### Symbolic permissions

```bash
chmod u+x script.sh
chmod g-w application.log
chmod o-r application.log
```

- `u` – Owner/user.
- `g` – Group.
- `o` – Others.
- `a` – All.

### Change ownership using chown

```bash
sudo chown john application.log
sudo chown john:developers application.log
```

Changes the owner or owner and group.

---

# 11. Users and Groups

Linux supports multiple users and groups. Permissions can be assigned to control access to files and directories.

### Check current user

```bash
whoami
id
groups
```

### Important user and group files

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/group` | Group information |
| `/etc/shadow` | Protected password-related information |

Do not expose or share `/etc/shadow` contents.

### Create a user

```bash
sudo adduser john
```

Ubuntu's `adduser` command creates the user and guides you through setting up the account.

### Create a group

```bash
sudo groupadd developers
```

### Add a user to a supplementary group

```bash
sudo usermod -aG developers john
```

Important:

- `-a` means append.
- `-G` specifies supplementary groups.
- Using `-G` without `-a` can replace the existing supplementary group list.

### Verify group membership

```bash
groups john
id john
```

### Look up users and groups

```bash
getent passwd john
getent group developers
```

### Change file ownership

```bash
sudo chown john:developers application.log
```

---

# 12. sudo and Root User

### sudo

`sudo` allows an authorised user to execute a command with elevated privileges.

Examples:

```bash
sudo apt update
sudo systemctl status cron
sudo whoami
```

The last command displays:

```text
root
```

This does not permanently convert your account into root. Only that command runs with elevated privileges.

### Root user

- Root is the Linux superuser.
- Root has UID 0.
- Root has full administrative privileges.

On a default Ubuntu installation, members of the `sudo` group are authorised to use sudo, subject to the system's sudo configuration.

Check group membership:

```bash
groups
```

Use elevated privileges carefully. Do not run unfamiliar commands with sudo without understanding their effect.

---

# 13. Process Management

A process is a running instance of a program. Linux assigns each process a Process ID (PID).

### View processes

```bash
ps
ps -ef
```

- `ps` – Displays processes associated with the current terminal session.
- `ps -ef` – Displays a detailed process list across the system.

Important `ps -ef` columns:

| Column | Meaning |
|---|---|
| UID | User running the process |
| PID | Process ID |
| PPID | Parent Process ID |
| C | CPU utilisation indicator |
| STIME | Start time |
| TTY | Associated terminal |
| TIME | CPU time consumed |
| CMD | Command |

### Search for a process

```bash
ps -ef | grep ssh
```

To avoid matching the grep command itself:

```bash
ps -ef | grep '[s]sh'
```

### Inspect a process by PID

```bash
ps -fp PID
```

Replace `PID` with the actual process ID.

### Live process monitoring

```bash
top
```

Useful keys inside `top`:

- `P` – Sort by CPU usage.
- `M` – Sort by memory usage.
- `q` – Quit.

### Find processes using the most CPU

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head
```

### Find processes using the most memory

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%mem | head
```

---

# 14. Disk, Memory and System Resources

### df – Disk filesystem usage

```bash
df -h
```

Displays filesystem capacity and available space in human-readable units.

### du – Directory and file usage

```bash
du -sh ~/devops-learning
du -sh /var/log
du -sh *
```

| Option | Meaning |
|---|---|
| `-s` | Summary |
| `-h` | Human-readable units |
| `-a` | Include files as well as directories |

Find large files and directories:

```bash
du -ah | sort -rh | head
```

### free – Memory usage

```bash
free -h
free -m
```

Displays memory and swap information.

The `available` memory figure is generally more useful than the `free` figure when assessing how much memory can be used by applications without swapping.

### uptime

```bash
uptime
```

Displays:

- Current system time.
- How long the system has been running.
- Number of logged-in users.
- Load averages for 1, 5 and 15 minutes.

Load average is not CPU percentage. It represents the average system load, including runnable and certain uninterruptible tasks. Interpret it in relation to the number of CPU cores.

---

# 15. Linux Services and systemd

A service is a background process that performs a task, such as a database, web server, SSH server or scheduled job.

`systemd` is a system and service manager commonly used by Linux distributions.

### Check service status

```bash
systemctl status cron
systemctl status ssh
systemctl status nginx
```

The SSH and Nginx service checks in our Ubuntu WSL exercise returned a unit-not-found message because those service units were not installed. The cron service was available and running.

### Common service states

| Status | Meaning |
|---|---|
| `active (running)` | Service is running |
| `inactive (dead)` | Service is stopped |
| `failed` | Service failed |
| `activating` | Service is starting |
| `deactivating` | Service is stopping |

### Service management commands

```bash
sudo systemctl start service-name
sudo systemctl stop service-name
sudo systemctl restart service-name
sudo systemctl enable service-name
sudo systemctl disable service-name
```

| Command | Purpose |
|---|---|
| `start` | Starts service immediately |
| `stop` | Stops service |
| `restart` | Restarts service |
| `enable` | Configures automatic startup at boot |
| `disable` | Disables automatic startup at boot |

Important distinction:

- `start` does not necessarily enable automatic startup.
- `enable` does not necessarily start the service immediately.

### Check service state

```bash
systemctl is-active cron
systemctl is-enabled cron
systemctl list-units --type=service --all
systemctl is-system-running
```

### Reload systemd configuration

```bash
sudo systemctl daemon-reload
```

Reloads systemd unit configuration files. It does not restart every service.

### Cron service

```bash
systemctl status cron
```

`cron` is a scheduling service that runs configured tasks automatically at specified times.

Examples include scheduled backups, cleanup scripts and recurring maintenance tasks.

---

# 16. Linux Logs – journalctl

`journalctl` reads logs stored in the systemd journal.

### View logs for a service

```bash
journalctl -u cron
```

### View the latest 50 entries

```bash
journalctl -u service-name -n 50
```

### View the latest 20 entries for cron

```bash
journalctl -u cron -n 20
```

### Follow service logs in real time

```bash
journalctl -u cron -f
```

### Follow the complete system journal

```bash
journalctl -f
```

Press `Ctrl + C` to stop following logs.

### tail versus journalctl

```bash
tail -f application.log
journalctl -u service-name -f
```

| Command | Purpose |
|---|---|
| `tail -f application.log` | Follows a specific log file |
| `journalctl -u service-name -f` | Follows systemd journal logs for a service |
| `journalctl -f` | Follows the system journal |

---

# 17. Production Support Troubleshooting Examples

Linux commands are especially useful when troubleshooting production incidents.

### Scenario 1: Application is not responding

Check service status:

```bash
systemctl status application
```

Check recent logs:

```bash
journalctl -u application -n 50
```

Follow logs:

```bash
journalctl -u application -f
```

Check whether the process is running:

```bash
ps -ef | grep '[a]pplication'
```

### Scenario 2: High CPU utilisation

```bash
top
```

Or:

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head
```

Identify the process consuming high CPU and investigate its logs, workload and recent changes.

### Scenario 3: High memory utilisation

```bash
free -h
```

Identify processes using memory:

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%mem | head
```

### Scenario 4: Disk space is full

```bash
df -h
```

Identify large directories:

```bash
du -sh /var/log
du -ah /var/log | sort -rh | head
```

Find large files:

```bash
find /var/log -type f -size +500M -exec ls -lh {} \;
```

Do not delete production logs without checking retention requirements and obtaining the required approval.

### Scenario 5: Application errors in logs

```bash
grep -in "ERROR" application.log
```

Count error lines:

```bash
grep -ic "ERROR" application.log
```

Monitor new errors:

```bash
tail -f application.log | grep --line-buffered -i "error"
```

### Recommended production troubleshooting sequence

1. Understand the incident and its impact.
2. Check application or service status.
3. Review recent application and system logs.
4. Check CPU, memory and disk usage.
5. Identify recent deployments or configuration changes.
6. Check dependencies and connectivity using approved procedures.
7. Escalate if required.
8. Restart or change a service only with appropriate authorisation.
9. Verify service recovery and monitor logs.
10. Update the incident ticket, document the RCA and improve the runbook.

---

# 18. wget and curl

Both utilities can transfer data using URLs, but they are commonly used for different purposes.

### wget

`wget` is commonly used to download files.

```bash
wget https://example.com
```

Download using a specific filename:

```bash
wget -O sample.html https://example.com
```

### curl

`curl` transfers data to or from a URL. It is widely used for API testing and troubleshooting HTTP requests.

```bash
curl https://example.com
```

Display response headers:

```bash
curl -I https://example.com
```

Display detailed request and connection information:

```bash
curl -v https://example.com
```

Display response headers and body:

```bash
curl -i https://example.com
```

### Common curl options

| Option | Purpose |
|---|---|
| `-I` | Fetch response headers |
| `-i` | Include response headers in output |
| `-v` | Verbose connection and request details |
| `-o file` | Save response body to a file |
| `-O` | Save using the remote filename |
| `-L` | Follow redirects |
| `-X` | Specify HTTP method |
| `-H` | Add request headers |
| `-d` | Send request data |

Example API request:

```bash
curl -X GET https://example.com
```

Add a header:

```bash
curl -H "Accept: application/json" https://example.com
```

Note: HTTP and networking concepts will be covered in more detail in the networking module.

---

# 19. Archives and Compression

Linux provides utilities to combine files and compress data.

### tar

Create an archive:

```bash
tar -cvf logs.tar application.log error.log
```

Extract an archive:

```bash
tar -xvf logs.tar
```

List archive contents:

```bash
tar -tvf logs.tar
```

### gzip

Compress a file:

```bash
gzip application.log
```

This normally creates `application.log.gz` and removes the uncompressed source file.

Decompress:

```bash
gunzip application.log.gz
```

### Create a compressed TAR archive

```bash
tar -czvf logs.tar.gz application.log error.log
```

Extract:

```bash
tar -xzvf logs.tar.gz
```

### ZIP

Create a ZIP archive:

```bash
zip logs.zip application.log error.log
```

Compress a directory:

```bash
zip -r backup.zip backup/
```

Extract:

```bash
unzip logs.zip
```

Extract into a specific directory:

```bash
unzip logs.zip -d zip-extracted/
```

### TAR versus ZIP

| Feature | TAR | ZIP |
|---|---|---|
| Combines multiple files | Yes | Yes |
| Compresses by default | No | Yes |
| Common extension | `.tar` | `.zip` |
| Common commands | `tar -cvf`, `tar -xvf` | `zip`, `unzip` |
| GZIP-compressed TAR | `.tar.gz` | Not applicable |

Useful extensions:

- `.tar` – Archive; not necessarily compressed.
- `.gz` – GZIP-compressed data.
- `.tar.gz` or `.tgz` – TAR archive compressed with GZIP.
- `.zip` – ZIP archive, normally compressed.

---

# 20. Text Processing with AWK

`awk` is a text-processing utility that can extract fields, filter records and perform calculations on text files.

By default, AWK separates fields using whitespace.

### AWK field references

| Expression | Meaning |
|---|---|
| `$0` | Entire line |
| `$1` | First field |
| `$2` | Second field |
| `$3` | Third field |
| `$NF` | Last field |
| `NF` | Number of fields in the current line |

Example practice file:

```text
2026-11-21 INFO Application started
2026-11-21 ERROR Database connection failed
2026-11-21 WARN High memory usage
2026-11-21 ERROR Payment service timeout
```

Print the second field (log level):

```bash
awk '{print $2}' awk-practice.log
```

Print the first field (date):

```bash
awk '{print $1}' awk-practice.log
```

Print complete lines where the second field is ERROR:

```bash
awk '$2 == "ERROR" {print $0}' awk-practice.log
```

Count records where the second field is ERROR:

```bash
awk '$2 == "ERROR" {count++} END {print count}' awk-practice.log
```

In this example, the result is `2`.

Key ideas:

- A condition such as `$2 == "ERROR"` selects matching records.
- `count++` increments a counter.
- `END` runs after all input records have been processed.

---

# 21. Text Editing with SED

`sed` stands for Stream Editor. It can search, replace and delete text in a stream or file.

### Replace text in displayed output

```bash
sed 's/ERROR/CRITICAL/' awk-practice.log
```

This replaces the first matching occurrence on each line in the displayed output. It does not modify the original file.

### Replace all matching occurrences on each line

```bash
sed 's/ERROR/CRITICAL/g' awk-practice.log
```

The `g` flag means global replacement on each line.

### Delete matching lines from displayed output

```bash
sed '/CRITICAL/d' awk-practice.log
```

The `d` command deletes matching lines from the output. It does not modify the original file unless in-place editing is used.

### Modify the original file

```bash
sed -i 's/ERROR/CRITICAL/g' awk-practice.log
```

The `-i` option edits the file in place.

Important:

- Without `-i`, SED displays the changed output and leaves the source file unchanged.
- With `-i`, SED modifies the original file.
- Practise in a test directory and keep a backup before editing important files.

---

# 22. Process Control and Background Jobs

A process is a running instance of a program. Linux assigns each process a PID.

### Start a process in the background

```bash
sleep 300 &
```

- `sleep 300` waits for 300 seconds.
- `&` starts the command in the background.
- The shell displays a job number and PID.

### List jobs in the current shell

```bash
jobs
```

`jobs` shows background and suspended jobs managed by the current shell. It is not a list of every process on the system.

### Find a process

```bash
ps -ef | grep '[s]leep'
```

The pattern helps avoid matching the grep command itself.

### Terminate a process

```bash
kill PID
```

`kill` sends SIGTERM by default, requesting graceful termination.

If a process does not respond to normal termination, SIGKILL can be sent as a last resort:

```bash
kill -9 PID
```

Use process termination carefully, especially on production systems.

### Foreground and background job control

Start a foreground command:

```bash
sleep 300
```

Then use:

| Action | Purpose |
|---|---|
| `Ctrl + Z` | Suspend the foreground job |
| `jobs` | List shell jobs |
| `bg` | Resume a suspended job in the background |
| `fg` | Bring a job to the foreground |
| `Ctrl + C` | Interrupt the foreground command |
| `command &` | Start a command in the background |

Example practice sequence:

```bash
sleep 300
```

Press `Ctrl + Z`, then:

```bash
jobs
bg
jobs
fg
```

Press `Ctrl + C` to stop the foreground command.

Important distinction:

- `kill PID` sends a signal to a process.
- `Ctrl + C` interrupts the foreground command.
- `Ctrl + Z` suspends the foreground job; it does not terminate it.
- `bg` resumes a suspended job in the background.
- `fg` brings a job into the foreground.

---

# 23. Interactive Process Monitoring with htop

`htop` is an interactive process-monitoring utility that provides a visual view of processes and system resource usage.

Run:

```bash
htop
```

If it is not installed:

```bash
sudo apt install htop
```

Useful features:

- View CPU and memory utilisation.
- Identify resource-intensive processes.
- Sort and filter processes.
- Inspect process IDs and owners.

Quit using `F10` or `q`, depending on the terminal configuration.

---

# 24. Quick Linux Command Revision

| Command | Purpose |
|---|---|
| `pwd` | Current directory |
| `whoami` | Current username |
| `ls -l` | Detailed file listing |
| `ls -la` | Detailed listing including hidden files |
| `cd ..` | Parent directory |
| `mkdir` | Create directory |
| `touch` | Create empty file/update timestamp |
| `cp` | Copy |
| `mv` | Move/rename |
| `rm -i` | Delete with confirmation |
| `rmdir` | Remove empty directory |
| `rm -r` | Remove directory recursively |
| `cat` | Display file |
| `less` | Read file page by page |
| `head` | First lines |
| `tail` | Last lines |
| `tail -f` | Follow log file |
| `file` | Identify file type |
| `stat` | File metadata |
| `tree` | Directory tree |
| `grep` | Search text |
| `find` | Search files/directories |
| `wc -l` | Count lines |
| `wc -w` | Count words |
| `wc -c` | Count bytes |
| `sort` | Sort lines |
| `uniq` | Remove adjacent duplicates |
| `chmod` | Change permissions |
| `chown` | Change ownership |
| `id` | UID/GID information |
| `groups` | User groups |
| `adduser` | Create user |
| `groupadd` | Create group |
| `usermod -aG` | Add supplementary group |
| `sudo` | Execute with elevated privileges |
| `ps` | Process listing |
| `ps -ef` | Detailed process listing |
| `top` | Live process monitoring |
| `df -h` | Filesystem usage |
| `du -sh` | File/directory size |
| `free -h` | RAM usage |
| `uptime` | Uptime and load average |
| `systemctl` | Manage services |
| `journalctl` | Read systemd journal logs |
| `wget` | Download files |
| `curl` | Transfer data using URLs |
| `tar` | Create/extract archives |
| `gzip` | Compress files |
| `gunzip` | Decompress GZIP files |
| `awk` | Extract and process text fields |
| `sed` | Search, replace and edit text |
| `jobs` | List shell jobs |
| `bg` | Resume a job in background |
| `fg` | Bring a job to foreground |
| `kill` | Send a signal to a process |
| `htop` | Interactive process monitoring |

---

# 25. Topics Intentionally Pending

These topics remain planned for later study and are not covered by this version of the notes.

### Networking fundamentals

- IP addresses.
- Public vs private IP.
- Subnets and subnet masks.
- Default gateway.
- DNS.
- TCP vs UDP.
- Ports.
- HTTP vs HTTPS.
- `ping`, `curl`, `nslookup`, `dig`, `ss`, `ip`, `traceroute`.

### Remote access

- SSH.
- SSH key authentication.
- SFTP.
- FTP vs SFTP.

**Future Linux notes version:** Version 2.0 will include networking and remote-access topics after they have been studied and practised.

---

**End of Linux Command Reference – Version 1.1**