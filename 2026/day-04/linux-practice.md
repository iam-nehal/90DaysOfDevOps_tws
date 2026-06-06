This document captures hands-on Linux practice focused on **processes, services, and logs** using the **SSH service** on an Ubuntu AWS EC2 instance.

Environment:

- OS: Ubuntu 26.04 (AWS EC2)
- Authentication: SSH key-based authentication
- Service inspected: SSH (sshd)

---

## 🔹 Process Commands

### 1. `pgrep -af sshd`

**Explanation:**  
Lists all running `sshd` processes, including the main listener and each active SSH session.

| `pgrep ` | Search running process               |
| -------- | ------------------------------------ |
| `-af`    | show PID + full entire command line  |
| `sshd`   | Search for processes containing sshd |

**Observations from output:**

- 646 ->  main SSH daemon (listener)
- 1077 ->  privileged SSH process
- 1210 -> active user session ( `pts/0`)

Output:
```
Pasted image 20260605140649.png
```

----

## 2. ps aux | grep sshd

**Explanation:**
Shows detailed resource usage and ownership of SSH daemon and session processes.

**Observations from output:**

- SSH daemon runs as root
- User sessions run as ubuntu
- Multiple sessions create multiple sshd processes (normal behavior)

Output:

Pasted image 20260605115710.png

---
## 🔹 Service Commands

## 3. systemctl status ssh

**Explanation:**
Displays the health, uptime, and recent activity of the SSH service managed by systemd.

**Observations from output:**

- SSH is active (running)
- Listening on port 22
- EC2 Instance Connect is providing SSH keys
- Successful public key authentication

Output:
Pasted image 20260605120251.png

---

## 4. systemctl list-units --type=service --state=running

**Explanation:**
Lists all currently running system services, confirming overall system health.

**Observations from output:**

-  ssh.service is running
-  Core services (cron, systemd-journald, networkd) are active
-  Instance is stable

Output:
Pasted image 20260605122444.png

----

## 🔹 Log Commands

## 5. journalctl -u ssh -n 10

**Explanation:**
Shows the latest SSH service logs, including service startup and authentication events.

**Observations from output:**

-  SSH service startup
-  Port 22 listening
-  Accepted public key logins from real IPs
- Session creation events


Output:
Pasted image 20260605123045.png

---

## 6. tail -n 10 /var/log/auth.log

**Explanation:**
Displays the most recent authentication and authorization activity on the system.

**Observations from output:**

-  SSH session opened for user ubuntu
-  sudo command activity logged
-  cron jobs running as root

Output:
Pasted image 20260605123313.png

## ✅ Key Learnings

- SSH creates multiple processes per user session
- AWS EC2 uses key-based SSH authentication
- `systemctl` is used to inspect and manage services
- Logs (`journalctl`, `auth.log`) are essential for troubleshooting and security auditing

-------

## � Quick Reference Table

### Process Commands

| Command | What It Does |
|---|---|
| `ps aux` | List all running processes |
| `ps aux --sort=-%cpu` | Sort by CPU usage |
| `ps aux --sort=-%mem` | Sort by memory usage |
| `top` | Interactive real-time monitor |
| `pgrep -a <name>` | Find process by name |
| `kill <PID>` | Send TERM signal to process |
| `kill -9 <PID>` | Force kill a process |
| `pstree -p` | Show parent-child process tree |

### Service Commands

| Command | What It Does |
|---|---|
| `systemctl status <svc>` | Check service status |
| `systemctl start <svc>` | Start a service |
| `systemctl stop <svc>` | Stop a service |
| `systemctl restart <svc>` | Restart a service |
| `systemctl enable <svc>` | Enable at boot |
| `systemctl disable <svc>` | Disable at boot |
| `systemctl is-active <svc>` | Check if currently running |
| `systemctl list-units --type=service` | List all services |

### Log Commands

| Command | What It Does |
|---|---|
| `journalctl -u <svc>` | Logs for a specific service |
| `journalctl -u <svc> -f` | Follow logs in real-time |
| `journalctl -u <svc> -n 50` | Last 50 log entries |
| `journalctl -p err` | Only error-level messages |
| `journalctl --since "1 hour ago"` | Logs from last 1 hour |
| `tail -f /var/log/syslog` | Follow system log file |
| `tail -f /var/log/auth.log` | Follow authentication log |


---

## � STAT Column Reference

**Primary State (first letter):**

| Code | Meaning |
|---|---|
| `R` | Running - actively using CPU |
| `S` | Sleeping - waiting for an event (interruptible) |
| `D` | Disk sleep - waiting for I/O, cannot be interrupted |
| `Z` | Zombie - finished but parent hasn't cleaned it up |
| `T` | Stopped - paused (e.g. Ctrl+Z) |
| `I` | Idle kernel thread - doing nothing |

**Modifier flags (extra letters after):**

| Code | Meaning |
|---|---|
| `s` | Session leader - started a process group (e.g. main `sshd`) |
| `l` | Multi-threaded - has multiple threads running |
| `+` | Foreground process - running in active terminal |
| `<` | High priority - gets more CPU time |
| `N` | Low priority (nice) - yields CPU to others |
| `L` | Pages locked in RAM - can't be swapped out |

**Common combinations:**

| STAT | Meaning |
|---|---|
| `Ss` | Sleeping + session leader (most daemons) |
| `Ssl` | Sleeping + session leader + multi-threaded |
| `S+` | Sleeping + running in foreground terminal |
| `SN` | Sleeping + low priority |
| `I<` | Idle kernel thread + high priority |
| `R+` | Actively running in foreground |

> � **Simple rule:** First letter = what it's doing right now. Remaining letters = how it's doing it.

---

## � Key Takeaways

1. `ps`, `top`, `pgrep` are your go-to tools for understanding what's running and why
2. `systemctl` is the central control panel for all services in modern Linux (systemd)
3. `journalctl` is more powerful than log files - filter by unit, time range, or severity
4. Always check logs **before** trying to fix a service — they tell you the exact cause
5. Failed SSH logins in `auth.log` are a sign of brute-force attacks - monitor regularly
