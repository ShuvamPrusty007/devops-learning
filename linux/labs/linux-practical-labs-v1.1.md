# Linux Practical Labs – Cloud & DevOps Learning

**Version:** 1.1
**Environment:** Ubuntu 24.04 LTS on WSL 2
**Level:** Beginner to Intermediate
**Purpose:** Reproducible, hands-on Linux exercises for learners, Cloud/DevOps preparation and production-support practice.

---

## How to Use These Labs

These exercises are designed to be reproducible by anyone who clones this GitHub repository.

Each lab contains:

1. Objective – What you will learn.
2. Commands – What to execute.
3. Explanation – Why the command is used.
4. Expected result – What you should observe.
5. Practice challenge – A task to complete independently.

All log entries and sample data are synthetic. They do not represent real company or production information.

### Before starting

Open Ubuntu and navigate to your learning repository:

```bash
cd ~/devops-learning
```

Go to the Linux practice directory:

```bash
cd linux
```

Check your location:

```bash
pwd
```

The exercises use the following practice structure:

```text
linux/
├── backup2/
├── lab1/
│   ├── archive-practice/
│   ├── backup/
│   ├── application.log
│   ├── error.log
│   ├── access.log
│   ├── duplicate.log
│   └── awk-practice.log
├── labs/
└── notes/
```

Some files may already exist from previous exercises. Where a lab creates or replaces sample data, read the command carefully before executing it.

---

# Lab 1 – Create a Practice Directory Structure

### Objective

Learn to create directories and navigate between them.

### Step 1: Navigate to Linux

```bash
cd ~/devops-learning/linux
```

### Step 2: Create the lab directory

```bash
mkdir -p lab1/backup
mkdir -p lab1/archive-practice
mkdir -p backup2
```

### Step 3: Verify the structure

```bash
tree -L 3
```

Expected structure includes:

```text
linux/
├── backup2/
├── lab1/
│   ├── archive-practice/
│   └── backup/
└── notes/
```

### Step 4: Practise navigation

```bash
cd lab1
pwd
cd backup
pwd
cd ..
pwd
```

### What you learned

- `mkdir` creates directories.
- `mkdir -p` creates nested directories.
- `tree` displays the directory structure.
- `pwd` confirms your current location.
- `cd ..` moves to the parent directory.

### Practice challenge

Create the following structure without using a graphical interface:

```text
lab1/
├── application/
├── logs/
└── reports/
```

---

# Lab 2 – Create Realistic Application Logs

### Objective

Create sample application logs that can be used in later exercises.

### Step 1: Navigate to lab1

```bash
cd ~/devops-learning/linux/lab1
```

### Step 2: Create application.log

```bash
cat > application.log <<EOF
2026-11-21 09:00:00 INFO Application started successfully
2026-11-21 09:02:15 INFO User authentication completed
2026-11-21 09:05:30 INFO Database connection established
2026-11-21 09:10:45 WARN High response time detected
2026-11-21 09:12:10 ERROR Database connection timeout
2026-11-21 09:15:20 INFO Retry attempt started
2026-11-21 09:16:30 INFO Database connection restored
2026-11-21 09:20:00 ERROR Payment service unavailable
2026-11-21 09:25:00 INFO Payment service recovered
2026-11-21 09:30:00 ERROR API request failed
2026-11-21 09:35:00 INFO Application health check successful
2026-11-21 09:40:00 WARN Memory usage above threshold
2026-11-21 09:45:00 INFO Scheduled job completed
2026-11-21 09:50:00 INFO Application running normally
EOF
```

### Step 3: Create error.log

```bash
cat > error.log <<EOF
2026-11-21 09:12:10 ERROR Database connection timeout
2026-11-21 09:20:00 ERROR Payment service unavailable
2026-11-21 09:30:00 ERROR API request failed
2026-11-21 10:05:00 ERROR Authentication service timeout
2026-11-21 10:15:00 ERROR Payment gateway connection refused
EOF
```

### Step 4: Create access.log

```bash
cat > access.log <<EOF
192.168.1.10 GET /login 200
192.168.1.11 POST /login 200
192.168.1.12 GET /dashboard 200
192.168.1.13 GET /payment 500
192.168.1.14 POST /payment 200
192.168.1.15 GET /orders 404
192.168.1.16 POST /orders 201
192.168.1.17 GET /health 200
192.168.1.18 GET /payment 503
EOF
```

### Step 5: Verify the files

```bash
ls -l
cat application.log
cat error.log
cat access.log
```

### What you learned

- `cat > filename` creates or overwrites a file.
- `<<EOF` allows multiple lines to be entered.
- `EOF` marks the end of the input.
- Application logs contain timestamps, severity levels and messages.

### Practice challenge

Create a `database.log` file containing five entries with INFO, WARN and ERROR levels.

---

# Lab 3 – Read and Inspect Log Files

### Objective

Practise reading logs and inspecting file metadata.

Navigate to:

```bash
cd ~/devops-learning/linux/lab1
```

### Step 1: Display the complete file

```bash
cat application.log
```

### Step 2: Display the first five lines

```bash
head -n 5 application.log
```

### Step 3: Display the last five lines

```bash
tail -n 5 application.log
```

### Step 4: Read the file page by page

```bash
less application.log
```

Inside `less`:

- Enter `/ERROR` to search.
- Press `n` for the next match.
- Press `q` to quit.

### Step 5: Identify the file type

```bash
file application.log
file access.log
```

### Step 6: Inspect file metadata

```bash
stat application.log
```

### Step 7: Display the directory tree

```bash
tree -L 2
```

### Step 8: Monitor new log entries

Open two Ubuntu terminal tabs.

In Terminal 1:

```bash
cd ~/devops-learning/linux/lab1
tail -f application.log
```

In Terminal 2:

```bash
cd ~/devops-learning/linux/lab1
echo "2026-11-21 10:30:00 INFO New application event received" >> application.log
```

You should see the new entry appear in Terminal 1.

Press `Ctrl + C` in Terminal 1 to stop monitoring.

### Practice challenge

Use `head`, `tail`, `less`, `file` and `stat` on `error.log`.

---

# Lab 4 – Search Logs Using grep

### Objective

Search for application errors, warnings and specific messages.

Navigate to:

```bash
cd ~/devops-learning/linux/lab1
```

### Step 1: Search for errors

```bash
grep "ERROR" application.log
```

### Step 2: Search without case sensitivity

```bash
grep -i "error" application.log
```

### Step 3: Display line numbers

```bash
grep -in "error" application.log
```

### Step 4: Count matching lines

```bash
grep -c "ERROR" application.log
```

Before the later labs append more entries, the result should be:

```text
3
```

### Step 5: Search for warnings

```bash
grep "WARN" application.log
```

### Step 6: Exclude INFO entries

```bash
grep -v "INFO" application.log
```

### Step 7: Search multiple files

```bash
grep -i "ERROR" *.log
```

### Step 8: Search for errors in real time

```bash
tail -f application.log | grep --line-buffered -i "error"
```

In another terminal, append an error:

```bash
echo "2026-11-21 10:45:00 ERROR Payment transaction failed" >> application.log
```

The filtered terminal should display the new matching entry.

### What you learned

- `grep` searches text.
- `-i` ignores case.
- `-n` displays line numbers.
- `-c` counts matching lines.
- `-v` excludes matching lines.
- `-r` searches recursively.
- Pipes can combine grep with other commands.

### Practice challenge

Find all lines containing `payment` in `application.log`, ignoring case, and display their line numbers.

---

# Lab 5 – Find Files and Directories

### Objective

Find files based on name, type, size and modification time.

Navigate to:

```bash
cd ~/devops-learning/linux
```

### Step 1: Find application.log

```bash
find . -name "application.log"
```

### Step 2: Find all log files

```bash
find . -type f -name "*.log"
```

### Step 3: Find directories

```bash
find . -type d
```

### Step 4: Find files larger than 1 KB

Create a test file:

```bash
cd lab1
dd if=/dev/zero of=large-file.log bs=1024 count=2
```

This creates a test file containing approximately 2 KB of zero bytes.

Now find files larger than 1 KB:

```bash
find . -type f -size +1k
```

### Step 5: Find files modified within the last seven 24-hour periods

```bash
find . -type f -name "*.log" -mtime -7
```

### Step 6: Display matching file sizes

```bash
find . -type f -name "*.log" -exec ls -lh {} \;
```

### Step 7: Find large files under a system directory

```bash
find /var/log -type f -size +1M -exec ls -lh {} \;
```

This is a safe listing operation. It does not delete anything.

### Practice challenge

Find all `.log` files under `linux` and display their sizes.

---

# Lab 6 – Pipes and Output Redirection

### Objective

Combine commands and save their output.

Navigate to:

```bash
cd ~/devops-learning/linux/lab1
```

### Step 1: Redirect output

```bash
echo "Application started" > output.txt
```

Check the file:

```bash
cat output.txt
```

### Step 2: Append output

```bash
echo "Database connected" >> output.txt
```

Check again:

```bash
cat output.txt
```

Both entries should be present.

### Step 3: Use a pipe

```bash
ps -ef | grep '[s]leep'
```

This filters process output.

### Step 4: Count error lines

```bash
grep -i "ERROR" application.log | wc -l
```

### Step 5: Redirect errors

```bash
find /root -type f 2> find-errors.txt
```

You may see permission-related errors saved in the file.

### Step 6: Redirect output and errors together

```bash
find /root -type f > find-output.txt 2>&1
```

### Practice challenge

Find all ERROR lines in `application.log` and save them into `errors-only.txt`.

---

# Lab 7 – Count, Sort and Remove Duplicates

### Objective

Practise `wc`, `sort` and `uniq`.

Navigate to:

```bash
cd ~/devops-learning/linux/lab1
```

### Step 1: Count lines

```bash
wc -l application.log
```

### Step 2: Count words

```bash
wc -w application.log
```

### Step 3: Count bytes

```bash
wc -c application.log
```

### Step 4: Create duplicate.log

```bash
cat > duplicate.log <<EOF
ERROR Database timeout
INFO Application started
ERROR Database timeout
WARN High memory usage
INFO Application started
ERROR Payment failed
EOF
```

### Step 5: Display duplicate entries

```bash
cat duplicate.log
```

### Step 6: Remove adjacent duplicates

```bash
uniq duplicate.log
```

Notice that repeated entries may remain because they are not adjacent.

### Step 7: Sort and remove duplicates

```bash
sort duplicate.log | uniq
```

### Step 8: Count duplicate entries

```bash
sort duplicate.log | uniq -c
```

### Step 9: Save sorted output

```bash
sort duplicate.log > desc_sorted.log
```

The filename is retained for continuity with the existing practice directory; the default `sort` order is ascending, not descending.

For descending order:

```bash
sort -r duplicate.log > desc_sorted.log
```

### Practice challenge

Count how many times each message appears in `duplicate.log`.

---

# Lab 8 – File Permissions

### Objective

Understand and change Linux file permissions.

Navigate to:

```bash
cd ~/devops-learning/linux/lab1
```

### Step 1: Create a file

```bash
touch permission-test.txt
```

### Step 2: Check permissions

```bash
ls -l permission-test.txt
```

### Step 3: Set permission 644

```bash
chmod 644 permission-test.txt
```

Verify:

```bash
ls -l permission-test.txt
```

Expected permission representation:

```text
-rw-r--r--
```

### Step 4: Create a script

```bash
echo '#!/bin/bash' > script.sh
echo 'echo "Linux script executed successfully"' >> script.sh
```

### Step 5: Give execute permission

```bash
chmod 755 script.sh
```

### Step 6: Execute the script

```bash
./script.sh
```

Expected output:

```text
Linux script executed successfully
```

### Step 7: Practise symbolic permissions

```bash
chmod u+x script.sh
chmod g-w permission-test.txt
```

### Practice challenge

Set a file to permission `750` and explain what owner, group and others can do.

---

# Lab 9 – Users and Groups

### Objective

Practise creating users, groups and assigning supplementary group membership.

**Important:** These commands change system accounts. Run them only in your own learning environment, not on a production machine without approval.

### Step 1: Create a user

```bash
sudo adduser john
```

Follow the prompts.

### Step 2: Create a group

```bash
sudo groupadd developers
```

If the group already exists, you do not need to create it again.

### Step 3: Add the user to the group

```bash
sudo usermod -aG developers john
```

### Step 4: Verify membership

```bash
groups john
id john
```

### Step 5: Look up the user

```bash
getent passwd john
```

### Step 6: Look up the group

```bash
getent group developers
```

### Step 7: Change ownership of a practice file

```bash
sudo chown john:developers permission-test.txt
```

Verify:

```bash
ls -l permission-test.txt
```

### Important learning point

Use `usermod -aG` to append a supplementary group. Avoid using `usermod -G` alone unless you intentionally want to replace the user's supplementary group list.

### Practice challenge

Explain the difference between `chmod`, `chown`, `adduser`, `groupadd` and `usermod`.

---

# Lab 10 – Process Management

### Objective

Start, inspect and terminate Linux processes.

### Step 1: Start a background process

```bash
sleep 300 &
```

The command runs in the background.

### Step 2: List current shell jobs

```bash
jobs
```

### Step 3: Find the sleep process

```bash
ps -ef | grep '[s]leep'
```

### Step 4: Identify its PID

Look at the PID column in the output.

### Step 5: Inspect the process

Replace `PID` with the actual process ID:

```bash
ps -fp PID
```

### Step 6: Terminate the process

```bash
kill PID
```

This sends SIGTERM by default.

Check:

```bash
jobs
```

### Step 7: Practise foreground job control

Start:

```bash
sleep 300
```

Press `Ctrl + Z` to suspend it.

Then:

```bash
jobs
bg
jobs
fg
```

Press `Ctrl + C` to interrupt the foreground command.

### What you learned

- `&` starts a command in the background.
- `jobs` lists shell-managed jobs.
- `ps` displays processes.
- `kill` sends a signal.
- `Ctrl + Z` suspends a foreground job.
- `bg` resumes a suspended job in the background.
- `fg` brings a job to the foreground.
- `Ctrl + C` interrupts a foreground command.

---

# Lab 11 – Disk, Memory and System Resources

### Objective

Check disk usage, directory sizes, memory and system uptime.

### Step 1: Check disk space

```bash
df -h
```

### Step 2: Check Linux directory size

```bash
du -sh ~/devops-learning/linux
```

### Step 3: Check individual directory sizes

```bash
du -sh *
```

Run this from the Linux directory.

### Step 4: Identify the largest files and directories

```bash
du -ah | sort -rh | head
```

### Step 5: Check memory

```bash
free -h
```

### Step 6: Check uptime and load average

```bash
uptime
```

### Practice challenge

Explain the difference between `df -h` and `du -sh`.

---

# Lab 12 – Services and System Logs

### Objective

Check service health and inspect systemd journal logs.

### Step 1: Check cron service status

```bash
systemctl status cron
```

### Step 2: Check whether the service is active

```bash
systemctl is-active cron
```

### Step 3: Check whether it is enabled

```bash
systemctl is-enabled cron
```

### Step 4: List services

```bash
systemctl list-units --type=service --all
```

### Step 5: View cron logs

```bash
journalctl -u cron
```

### Step 6: Display the latest 20 entries

```bash
journalctl -u cron -n 20
```

### Step 7: Follow logs

```bash
journalctl -u cron -f
```

Press `Ctrl + C` to stop following.

### Important

Some services may not exist in a fresh Ubuntu installation. A unit-not-found message does not automatically mean the entire Linux system is unhealthy.

Do not stop or restart important services just for practice. Use a disposable learning environment.

---

# Lab 13 – AWK Log Analysis

### Objective

Extract log fields and count ERROR records using AWK.

Navigate to:

```bash
cd ~/devops-learning/linux/lab1
```

### Step 1: Create the practice file

```bash
cat > awk-practice.log <<EOF
2026-11-21 INFO Application started
2026-11-21 ERROR Database connection failed
2026-11-21 WARN High memory usage
2026-11-21 ERROR Payment service timeout
2026-11-21 INFO User login successful
EOF
```

### Step 2: Print the first field

```bash
awk '{print $1}' awk-practice.log
```

### Step 3: Print the second field

```bash
awk '{print $2}' awk-practice.log
```

### Step 4: Display ERROR entries

```bash
awk '$2 == "ERROR" {print $0}' awk-practice.log
```

### Step 5: Count ERROR entries

```bash
awk '$2 == "ERROR" {count++} END {print count}' awk-practice.log
```

Expected output:

```text
2
```

### What you learned

- `$1` accesses the first field.
- `$2` accesses the second field.
- `$0` represents the complete line.
- `count++` increments a counter.
- `END` runs after all input records have been processed.

### Practice challenge

Count how many INFO and WARN records are present.

---

# Lab 14 – SED Text Replacement

### Objective

Search, replace and delete text using SED.

Use the file created in Lab 13.

### Step 1: Replace ERROR in displayed output

```bash
sed 's/ERROR/CRITICAL/' awk-practice.log
```

The original file remains unchanged.

### Step 2: Replace all occurrences on each line

```bash
sed 's/ERROR/CRITICAL/g' awk-practice.log
```

### Step 3: Display output without CRITICAL lines

```bash
sed '/CRITICAL/d' awk-practice.log
```

### Step 4: Create a backup before editing

```bash
cp awk-practice.log awk-practice-backup.log
```

### Step 5: Modify the original practice file

```bash
sed -i 's/ERROR/CRITICAL/g' awk-practice.log
```

Verify:

```bash
cat awk-practice.log
```

### Important

- Without `-i`, SED prints modified output and leaves the source unchanged.
- With `-i`, SED modifies the original file.
- Keep a backup before editing important files.

### Practice challenge

Replace all WARN entries with WARNING in a copy of the file.

---

# Lab 15 – Downloading and Archiving Files

### Objective

Practise downloading files and creating archives.

### Step 1: Navigate to archive-practice

```bash
cd ~/devops-learning/linux/lab1/archive-practice
```

### Step 2: Create sample files

```bash
echo "Application log sample" > application.log
echo "Error log sample" > error.log
echo "Backup report sample" > report.txt
```

### Step 3: Download a sample webpage using wget

```bash
wget https://example.com
```

The exact downloaded filename can depend on the server response and redirects.

### Step 4: Use curl

```bash
curl -I https://example.com
```

Display verbose connection information:

```bash
curl -v https://example.com
```

### Step 5: Create a TAR archive

```bash
tar -cvf logs.tar application.log error.log
```

### Step 6: List archive contents

```bash
tar -tvf logs.tar
```

### Step 7: Extract the archive

```bash
mkdir tar-extracted
tar -xvf logs.tar -C tar-extracted
```

### Step 8: Create a compressed TAR archive

```bash
tar -czvf logs.tar.gz application.log error.log
```

### Step 9: Extract the compressed archive

```bash
mkdir gzip-extracted
tar -xzvf logs.tar.gz -C gzip-extracted
```

### Step 10: Create a ZIP archive

```bash
zip logs.zip application.log error.log report.txt
```

### Step 11: Extract ZIP contents

```bash
mkdir zip-extracted
unzip logs.zip -d zip-extracted
```

### What you learned

- `wget` downloads files.
- `curl` transfers data and can inspect HTTP responses.
- `tar` combines files into an archive.
- `gzip` compresses files.
- `zip` and `unzip` create and extract ZIP archives.

---

# Lab 16 – Production Support Troubleshooting Scenario

### Objective

Apply Linux commands to investigate a simulated application incident.

### Scenario

A production application is reporting intermittent failures. Users complain that payment requests are taking longer than usual.

You are the support engineer investigating the issue.

### Step 1: Check the application logs

```bash
cd ~/devops-learning/linux/lab1
tail -n 20 application.log
```

### Step 2: Search for errors

```bash
grep -in "ERROR" application.log
```

### Step 3: Search for payment-related issues

```bash
grep -in "payment" application.log
```

### Step 4: Check disk utilisation

```bash
df -h
```

### Step 5: Check memory utilisation

```bash
free -h
```

### Step 6: Check processes

```bash
ps -ef | head
```

### Step 7: Identify high CPU processes

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head
```

### Step 8: Check service status

```bash
systemctl status cron
```

For an actual application, use its real service name if installed and available.

### Step 9: Record findings

Create a report:

```bash
cat > incident-report.txt <<EOF
Incident: Simulated payment application errors

Observations:
- Application log contains payment-related errors.
- Database timeout messages are present.
- Disk and memory utilisation were checked.
- Process information was reviewed.
- Service status was checked.

Next actions:
- Investigate database connectivity.
- Review recent application changes.
- Escalate to the responsible team if required.
- Monitor logs after recovery.
- Update the incident record and runbook.
EOF
```

Review the report:

```bash
cat incident-report.txt
```

### Production support lesson

Do not immediately restart an application when an error occurs.

First gather evidence, understand the impact, check logs and resources, investigate dependencies, and follow the incident-management and approval process.

---

# Lab 17 – Final Independent Practice Challenge

Complete these tasks without looking at the command reference.

1. Navigate to `linux/lab1`.
2. Create a file named `final-practice.log`.
3. Add at least ten sample log entries.
4. Include INFO, WARN and ERROR entries.
5. Display the last five lines.
6. Search for all ERROR entries.
7. Display matching line numbers.
8. Count the ERROR lines.
9. Save ERROR entries to `final-errors.log`.
10. Find all `.log` files under the Linux directory.
11. Display the sizes of those files.
12. Create a backup directory.
13. Copy the final log files into it.
14. Set `final-practice.log` permissions to `644`.
15. Create a TAR archive containing the practice logs.
16. Extract the archive into a new directory.
17. Use AWK to count ERROR entries.
18. Use SED to replace WARN with WARNING in displayed output.
19. Check disk and memory utilisation.
20. Write a short incident report explaining the findings.

---

# Lab Completion Checklist

- [ ] Created directories and files.
- [ ] Created synthetic application, error and access logs.
- [ ] Used cat, less, head and tail.
- [ ] Used grep to search and count errors.
- [ ] Used find to locate files by name, type and size.
- [ ] Used pipes and output redirection.
- [ ] Practised wc, sort and uniq.
- [ ] Changed permissions with chmod.
- [ ] Practised users and groups.
- [ ] Viewed and managed test processes.
- [ ] Checked disk, memory and uptime.
- [ ] Inspected services using systemctl.
- [ ] Read service logs using journalctl.
- [ ] Practised AWK and SED.
- [ ] Created and extracted archives.
- [ ] Completed the production-support scenario.
- [ ] Completed the independent challenge.

---

## Notes for GitHub Readers

- All examples use synthetic data.
- Commands are intended for a personal learning environment.
- Some commands require `sudo` privileges.
- Service names and available commands may differ across Linux distributions.
- Do not run destructive commands on production systems without verification and authorisation.
- Do not publish private keys, passwords, API tokens, real customer information or confidential company logs.

**End of Linux Practical Labs – Version 1.1**
