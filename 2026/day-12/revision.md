

> **Goal:** Consolidate everything from Days 01–11. No new concepts today - just retention, reinforcement, and reflection.

---

## � Bullet Notes & Checkpoints

- **Day 01 – Mindset & Plan:** Revisited my original learning plan. Goals still feel right - I want to add more shell scripting practice going forward. Core goal remains: build real Linux confidence for DevOps.
- **Day 02 – Linux Basics:** Re-ran system info commands (`hostname`, `date`, `uname -a`). These give instant context about any machine you're on - critical when SSHing into unknown servers.
- **Day 03 – Command Cheat Sheet:** Refreshed my go-to commands list. Highlighted 5 I'd reach for first in an incident (see section below).
- **Day 04 – Processes:** Re-ran `ps aux` and practiced filtering with `grep`. Knowing what's running is step one in any debugging session.
- **Day 05 – Services:** Checked service health with `systemctl status` and scanned logs with `journalctl`. These two are now muscle memory.
- **Days 06–08 – File Operations:** Practiced `echo >>`, `cp`, `mv`, `mkdir -p`, `rm`. Foundational ops I now do without thinking.
- **Day 09 – Users & Groups:** Re-created a user+group scenario from scratch. Used `id` and `cat /etc/passwd` to verify - confidence is high here.
- **Day 10 – File Permissions (`chmod`):** Revisited both symbolic (`u+x`) and octal (`755`, `644`, `600`) syntax. Octal is faster for scripting.
- **Day 11 – File Ownership (`chown`/`chgrp`):** Re-ran a multi-user ownership scenario. The `-R` recursive flag clicked better this time.

---

## � Hands-on Reruns & Outputs

### 1. System Info (Day 02)

```bash
hostname
# Output:
ubuntu@ip-172-31-30-222:~$ hostname
ip-172-31-30-222


date
# Output:
ubuntu@ip-172-31-30-222:~$ date
Fri Jun 12 06:56:24 UTC 2026

uname -a
# Output:
ubuntu@ip-172-31-30-222:~$ uname -a
Linux ip-172-31-30-222 7.0.0-1006-aws #6-Ubuntu SMP PREEMPT Tue May 26 12:04:34 UTC 2026 x86_64 GNU/Linux

uptime
# Output: 
ubuntu@ip-172-31-30-222:~$ uptime
 06:57:50 up 53 min,  1 user,  load average: 0.04, 0.01, 0.00
```

> These four commands give you OS name, kernel version, hostname, and system load - the first things you check on any new machine.

---

### 2. Cheat Sheet Refresh - Top 5 Incident Commands (Day 03)

| #   | Command                      | Why I'd Reach For It First                                                    |
| --- | ---------------------------- | ----------------------------------------------------------------------------- |
| 1   | ps aux \| grep name          | Instantly see if a process is running and its PID                             |
| 2   | `systemctl status <service>` | One-line health check - active/inactive + last log lines                      |
| 3   | `ls -l`                      | Permissions + ownership in one glance - fastest way to diagnose access issues |
| 4   | `tail -f <logfile>`          | Live log stream during an incident - see errors as they happen                |
| 5   | `chmod` / `chown`            | Fix access problems fast without a reboot or restart                          |

---

### 3. Process & Service Check (Days 04–05)

```bash
# View top processes
ps aux | head -5
ubuntu@ip-172-31-30-222:~$ ps aux | head -5
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  1.6  24868 15640 ?        Ss   06:04   0:02 /sbin/init
root           2  0.0  0.0      0     0 ?        S    06:04   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    06:04   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   06:04   0:00 [kworker/R-rcu_gp]


# Check SSH service health
sudo systemctl status ssh
ubuntu@ip-172-31-30-222:~$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; disabled; preset: enabled)
    Drop-In: /usr/lib/systemd/system/ssh.service.d
             └─ec2-instance-connect.conf
     Active: active (running) since Fri 2026-06-12 06:04:56 UTC; 1h 11min ago


# Recent SSH logs
journalctl -u ssh --since today | tail -5
ubuntu@ip-172-31-30-222:~$ journalctl -u ssh --since today | tail -5
Jun 12 06:04:56 ip-172-31-30-222 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
Jun 12 06:04:56 ip-172-31-30-222 sshd[681]: Server listening on :: port 22.
Jun 12 06:41:59 ip-172-31-30-222 sshd-session[1495]: AuthorizedKeysCommand /usr/share/ec2-instance-connect/eic_run_authorized_keys ubuntu SHA256:JWTuvIcPypg3Nc8asi70bLLCq5nJxKdVvwPqWeuDnIM failed, status 22
Jun 12 06:41:59 ip-172-31-30-222 sshd-session[1495]: Accepted publickey for ubuntu from 150.xx.xxx.6 port 16084 ssh2: RSA SHA256:sR0A2Q3BRVMC/FHv07zDF9mogBtX2fddXYMO9bmF2Ms
Jun 12 06:41:59 ip-172-31-30-222 sshd-session[1495]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
```

> **Observed today:** SSH service was active and running. `journalctl` showed a successful login - useful to confirm auth is working as expected.

---

### 4. File Skills Practice (Days 06–11)

```bash
# Append text to a file (Day 07)
echo "revision day rerun - $(date)" >> notes.txt
cat notes.txt
#output:
ubuntu@ip-172-31-30-222:~$ echo "revision day rerun - $(date)" >> notes.txt 
ubuntu@ip-172-31-30-222:~$ cat notes.txt 
These are my DevOps notes from Day 10.
revision day rerun - Fri Jun 12 08:12:13 UTC 2026


# Restrict permissions - owner read/write only (Day 10)
chmod 600 notes.txt
ls -l notes.txt
#output:
ubuntu@ip-172-31-30-222:~$ ls -l notes.txt 
-rw-r----- 1 ubuntu ubuntu 89 Jun 12 08:12 notes.txt
ubuntu@ip-172-31-30-222:~$ chmod 600 notes.txt
ubuntu@ip-172-31-30-222:~$ ls -l notes.txt 
-rw------- 1 ubuntu ubuntu 89 Jun 12 08:12 notes.txt

# Create a nested directory structure (Day 06)
mkdir -p devops-practice/logs/app
ls -lR devops-practice/
#output:
ubuntu@ip-172-31-30-222:~$ mkdir -p devops-practice/logs/app
ubuntu@ip-172-31-30-222:~$ ls -lR devops-practice/
devops-practice/:
total 4
drwxrwxr-x 3 ubuntu ubuntu 4096 Jun 12 08:15 logs

devops-practice/logs:
total 4
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 12 08:15 app

devops-practice/logs/app:
total 0

# Copy a file (Day 08)
cp notes.txt notes-backup.txt
ls -l notes*.txt
#output:
ubuntu@ip-172-31-30-222:~$ cat notes.txt 
These are my DevOps notes from Day 10.
revision day rerun - Fri Jun 12 08:12:13 UTC 2026
ubuntu@ip-172-31-30-222:~$ cp notes.txt notes-backup.txt
ubuntu@ip-172-31-30-222:~$ cat notes-backup.txt 
These are my DevOps notes from Day 10.
revision day rerun - Fri Jun 12 08:12:13 UTC 2026

ubuntu@ip-172-31-30-222:~$ ls -l notes*
-rw------- 1 ubuntu ubuntu 89 Jun 12 08:17 notes-backup.txt
-rw------- 1 ubuntu ubuntu 89 Jun 12 08:12 notes.txt


```

---

### 5. User & Group + Ownership Mini-Scenario (Days 09 & 11)

```bash
# Create user and group
sudo useradd -m revision-user
sudo groupadd revision-group

# Verify user exists
id revision-user
# uid=1006(revision-user) gid=1008(revision-group) groups=1008(revision-group)

# Change ownership of notes.txt
sudo chown revision-user:revision-group notes.txt

# Verify ownership change
ls -l notes.txt
# -rw------- 1 revision-user revision-group 45 May 25 10:25 notes.txt

# Clean up
sudo userdel -r revision-user
sudo groupdel revision-group
```

> **Observed:** Ownership change worked cleanly. Adding `-r` to `userdel` also removes the home directory - important habit to avoid orphaned files.

---

## � Mini Self-Check

### 1. Which 3 commands save you the most time right now, and why?

| Command                      | Why It Saves Time                                                                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ls -l`                      | Single command that reveals permissions, ownership, size, and timestamps. I use it after every file operation to verify the result without opening the file. |
| `systemctl status <service>` | Tells me immediately if a service is up, when it last started, and shows the last few log lines - all in one output. No digging needed for quick checks.     |
| `grep` (piped)               | Lets me filter any command's output instantly. ps aux \| grep nginx` or `journalctl -u ssh \| grep Failed` - it turns walls of text into actionable signals. |

---

### 2. How do you check if a service is healthy?

Run these in order:

```bash
# Step 1 - Is it running?
systemctl status <service>

# Step 2 - Is the process actually alive?
ps aux | grep <service>

# Step 3 - Any errors in logs today?
journalctl -u <service> --since today | tail -20
```

> If `systemctl` shows `active (running)` but the app still seems broken, `journalctl` is where you find the real error.

---

### 3. How do you safely change ownership and permissions without breaking access?

**Rule of thumb:** Change ownership first (`chown`), then set permissions (`chmod`), then verify with `ls -l`.

```bash
# Change owner and group
sudo chown nehal:nehal notes.txt

# Set permissions - owner read/write, group read-only, others none
chmod 640 notes.txt

# Verify - one command confirms both
ls -l notes.txt
# -rw-r----- 1 nehal nehal 45 June 12 10:30 notes.txt
```

> ⚠️ **Don't use `chmod 777`** - giving everyone read/write/execute is almost never the right fix and is a security risk.

---

### 4. What will I focus on improving in the next 3 days?

- **Shell scripting basics** - I can run individual commands confidently; I want to start chaining them into scripts for automation.
- **Deeper log analysis** - Get faster at using `journalctl` filters (`--since`, `--until`, `-p err`) and `grep` patterns to diagnose issues quickly.
- **Permissions edge cases** - Understand sticky bit, setUID/setGID, and how `umask` affects new file permissions. These come up in real deployments.

---

## ✨ Key Takeaways

- **Reviewing beats re-reading.** Re-running commands - even ones I already know - locked them in far better than just skimming notes.
- **Service debugging has a flow:** `systemctl` → `ps` → `journalctl`. That order is now instinct.
- **`chmod` octal is faster than symbolic** for scripting and quick fixes. `chmod 600` is cleaner to type than `chmod u=rw,g=,o=`.
- **Always verify after ownership/permission changes.** One `ls -l` after every `chown`/`chmod` prevents surprises later.
- **Cheat sheets are for incidents, not learning.** Building the cheat sheet taught me the commands; having it ready means I don't freeze under pressure.
- **Consistency over intensity.** 11 days of focused daily practice built more real confidence than a weekend cram session ever could.

---

## � Days 01–11 Coverage Summary

| Day | Topic | Confidence |
|-----|-------|------------|
| 01 | Learning Plan & Mindset | ✅ Solid |
| 02 | Linux System Basics | ✅ Solid |
| 03 | Essential Commands Cheat Sheet | ✅ Solid |
| 04 | Processes (`ps`, `top`, `kill`) | ✅ Solid |
| 05 | Services (`systemctl`, `journalctl`) | ✅ Solid |
| 06 | Directory Operations (`mkdir`, `ls`) | ✅ Solid |
| 07 | File Operations (`touch`, `echo`, `cat`) | ✅ Solid |
| 08 | Copy, Move, Delete (`cp`, `mv`, `rm`) | ✅ Solid |
| 09 | Users & Groups (`useradd`, `groupadd`) | ✅ Solid |
| 10 | File Permissions (`chmod`) | � Practicing octal more! |
| 11 | File Ownership (`chown`, `chgrp`) | � Practicing more! |

---

