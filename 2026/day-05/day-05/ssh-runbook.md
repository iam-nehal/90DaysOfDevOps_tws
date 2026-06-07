
# � SSH Service – Linux Troubleshooting Runbook

**Date:** 06-06-2026

**Target Service:** `sshd` (OpenSSH Daemon)

---

## 1. Environment Basics

### ` uname -a ` 

uname = Unix name 
-a = It displays all information about the operating system and kernel.

Command Output:
```
ubuntu@ip-172-31-30-222:~$ uname -a
Linux ip-172-31-30-222 7.0.0-1006-aws #6-Ubuntu SMP PREEMPT Tue May 26 12:04:34 UTC 2026 x86_64 GNU/Linux
```

| Field          | Your Output                  | Meaning                              |
| -------------- | ---------------------------- | ------------------------------------ |
| Kernel Name    | Linux                        | Linux operating system kernel        |
| Hostname       | ip-172-31-30-222             | Server name (AWS EC2 hostname)       |
| Kernel Release | 7.0.0-1006-aws               | Running kernel version               |
| Build Version  | `#6-Ubuntu`                  | Ubuntu kernel build number           |
| SMP            | SMP                          | Supports multiple CPU cores          |
| PREEMPT        | PREEMPT                      | Preemptive kernel scheduling enabled |
| Build Date     | Tue May 26 12:04:34 UTC 2026 | Kernel compilation date/time         |
| Architecture   | x86_64                       | 64-bit Intel/AMD architecture        |
| OS Type        | GNU/Linux                    | GNU tools + Linux kernel             |

---

### `lsb_release -a`

Displays Linux distribution information

Command Output:

```
ubuntu@ip-172-31-30-222:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 26.04 LTS
Release:	26.04
Codename:	resolute
```

| **Field**      | **Your Output**  | **Meaning**               |
| -------------- | ---------------- | ------------------------- |
| Distributor ID | Ubuntu           | Linux distribution name   |
| Description    | Ubuntu 26.04 LTS | Full OS name and version  |
| Release        | 26.04            | Ubuntu version number     |
| Codename       | resolute         | Ubuntu release codename   |
| LTS            | LTS              | Long Term Support release |

---

## 2. Filesystem Sanity Check

### Create throwaway folder and copy a file

```
ubuntu@ip-172-31-30-222:~$ mkdir /tmp/runbook-demo
ubuntu@ip-172-31-30-222:~$ cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo/
```

Command Output:
```
total 4
-rw-r--r-- 1 ubuntu ubuntu 221 Jun  6 08:26 hosts-copy
```

> **Note:** `/tmp` is writable, filesystem is healthy. File copy succeeded without errors — no disk I/O issues at this stage.

---

## 3. Snapshot: CPU & Memory

### Find SSH PID and inspect it

```
ubuntu@ip-172-31-30-222:~$ pgrep sshd
651
1037
1171
ubuntu@ip-172-31-30-222:~$ ps -o pid,pcpu,pmem,comm -p 651
```

Command Output:

```
PID %CPU %MEM COMMAND
651  0.0  0.8 sshd
```

> **Note:** `sshd` PID is 651, nearly idle at 0.0% CPU and 0.8% memory. Expected behaviour — SSH daemon just sits and waits for incoming connections, consuming almost no resources.

---

### `free -h`

Command Output:

```
ubuntu@ip-172-31-30-222:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           908Mi       325Mi       188Mi       2.7Mi       517Mi       583Mi
Swap:             0B          0B          0B
```

> **Note:** Swap is completely unused - system is not memory-pressured. ~583 MiB available memory. SSH is not contributing to memory stress.

---

## 4. Snapshot: Disk & IO

### `df -h`

Command Output:

```
ubuntu@ip-172-31-30-222:/tmp$ df -h
Filesystem       Size  Used Avail Use% Mounted on
/dev/root         19G  2.6G   16G  15% /
tmpfs            455M     0  455M   0% /dev/shm
tmpfs            182M  900K  181M   1% /run
efivarfs         128K  3.3K  120K   3% /sys/firmware/efi/efivars
tmpfs            455M     0  455M   0% /tmp
none             1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none             1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/nvme0n1p13  989M  164M  759M  18% /boot
/dev/nvme0n1p15  105M  6.3M   99M   7% /boot/efi
none             1.0M     0  1.0M   0% /run/credentials/systemd-networkd.service
none             1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
none             1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyS0.service
tmpfs             91M  8.0K   91M   1% /run/user/1000

```

> **Note:** Root filesystem (`/dev/root`) is at 15% of 19GB - healthy but worth monitoring. NVMe drive confirms this is an SSD-based VM. `/boot/efi` at 7% is fine. No volumes near full that could block SSH log writes.

---

### `du -sh /var/log`

Command Output:

```
19M	/var/log/
```

> **Note:** Log directory is 19MB . Approaching the 20MB mark where log rotation should be reviewed. Run `sudo journalctl --vacuum-size=100M` to clean old journal logs if needed.`journalctl --vacuum-size=100M` is used to rotate and clean old systemd journal logs. It removes archived logs until the total journal size is reduced to 100 MB, helping manage disk space on Linux servers.

---

### `vmstat 1 5`

Command Output:

```
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 0  0      0 215084  35560 473916    0    0   200    41   88    0  0  0 99  0  0  0
 0  0      0 215084  35560 473916    0    0     0     0  320  184  0  0 100  0  0  0
 0  0      0 215084  35560 473916    0    0     0     0  252  134  0  0 100  0  0  0
 0  0      0 215084  35560 473916    0    0     0     0   95   61  0  0 100  0  0  0
 0  0      0 215084  35560 473916    0    0     0     0   61   45  0  0 100  0  0  0
```

> **Note:** used `vmstat 1 5` to monitor the system performance in real time. The output showed that the CPU was almost 100% idle (`id=100`), swap usage was zero (`si=0`, `so=0`), and there were no blocked processes (`r=0`, `b=0`). This indicates that the server was healthy, with no CPU, memory, or I/O bottlenecks.

---

## 5. Snapshot: Network

### `sudo ss -tulpn | grep ssh`

Command Output:

```
tcp   LISTEN 0      4096              0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=674,fd=3),("systemd",pid=1,fd=101))
tcp   LISTEN 0      4096                 [::]:22           [::]:*    users:(("sshd",pid=674,fd=4),("systemd",pid=1,fd=102))

```

> **Note:** `sshd` is correctly listening on port 22, both IPv4 and IPv6. PID matches what we found earlier. No unexpected ports open.

---

### `ping -c 3 localhost`

Command Output:

```
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.024 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.028 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.033 ms

--- localhost ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2080ms
rtt min/avg/max/mdev = 0.024/0.028/0.033/0.003 ms
```

> **Note:** Localhost is reachable with <1ms latency. 0% packet loss. Average response time is 0.028ms - network stack is perfectly healthy. SSH can reach local interfaces without issues.

---

### `ssh localhost`

### Observation:

> "`ssh localhost` is used to test whether the SSH service is running correctly on the local machine. It creates an SSH connection from the system to itself and helps verify SSH configuration, authentication, and connectivity. Tested the local SSH service using `ssh localhost`. The connection to the SSH daemon was successful, which confirmed that the SSH service was running and listening on port 22. However, authentication failed with `Permission denied (publickey)`, indicating that the required public/private key authentication was not configured for localhost login."

---

## 6. Logs Reviewed

### `journalctl -u ssh -n 10`

Command Output:

```
Jun 07 13:35:06 ip-172-31-30-222 systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Jun 07 13:35:07 ip-172-31-30-222 sshd[674]: Server listening on 0.0.0.0 port 22.
Jun 07 13:35:07 ip-172-31-30-222 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
Jun 07 13:35:07 ip-172-31-30-222 sshd[674]: Server listening on :: port 22.
Jun 07 13:36:11 ip-172-31-30-222 sshd-session[984]: AuthorizedKeysCommand /usr/share/ec2-instance-connect/eic_run_authorized_keys ubuntu SHA256:JWTuvIcPypg3N>
Jun 07 13:36:11 ip-172-31-30-222 sshd-session[984]: Accepted publickey for ubuntu from 103.85.8.1 port 10574 ssh2: RSA SHA256:sR0A2Q3BRVMC/FHv07zDF9mogBtX2fd>
Jun 07 13:36:11 ip-172-31-30-222 sshd-session[984]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
Jun 07 16:27:36 ip-172-31-30-222 sshd-session[2032]: Connection closed by 3.142.219.55 port 36262
Jun 07 16:51:51 ip-172-31-30-222 sshd-session[2054]: Connection closed by 44.201.161.147 port 19012 [preauth]
Jun 07 17:00:59 ip-172-31-30-222 sshd-session[2135]: Connection closed by authenticating user ubuntu 127.0.0.1 port 60924 [preauth]

```

> > "From the SSH journal logs, I observed that the SSH service started successfully and was listening on port 22 for both IPv4 and IPv6. A successful login was recorded for the ubuntu user using public-key authentication. I also noticed a few pre-authentication connection closures from external IPs and one failed localhost authentication attempt, which matched my `ssh localhost` test."

---

### `tail -n 10 /var/log/auth.log`

Command Output:

```
2026-06-07T16:56:35.797609+00:00 ip-172-31-30-222 sudo: pam_unix(sudo:session): session closed for user root
2026-06-07T16:56:45.481182+00:00 ip-172-31-30-222 sudo: pam_unix(sudo:session): session opened for user root(uid=0) by ubuntu(uid=1000)
2026-06-07T16:56:45.481375+00:00 ip-172-31-30-222 sudo: ubuntu : TTY=/dev/pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/ss -tulnp
2026-06-07T16:56:45.490192+00:00 ip-172-31-30-222 sudo: pam_unix(sudo:session): session closed for user root
2026-06-07T16:57:09.721098+00:00 ip-172-31-30-222 sudo: pam_unix(sudo:session): session opened for user root(uid=0) by ubuntu(uid=1000)
2026-06-07T16:57:09.721338+00:00 ip-172-31-30-222 sudo: ubuntu : TTY=/dev/pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/ss -tulpn
2026-06-07T16:57:09.730340+00:00 ip-172-31-30-222 sudo: pam_unix(sudo:session): session closed for user root
2026-06-07T17:00:59.604883+00:00 ip-172-31-30-222 sshd-session[2135]: Connection closed by authenticating user ubuntu 127.0.0.1 port 60924 [preauth]
2026-06-07T17:17:01.091958+00:00 ip-172-31-30-222 CRON[2202]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
2026-06-07T17:17:01.095289+00:00 ip-172-31-30-222 CRON[2202]: pam_unix(cron:session): session closed for user root

```

> **Note:** ✅ No brute-force attempts or unauthorized logins found. From the auth.log output, I observed sudo activity where the ubuntu user executed `ss -tulpn` with root privileges. I also noticed a failed localhost SSH authentication attempt (`preauth`) and a successful execution of a scheduled cron job. Overall, authentication, sudo, and cron logging were functioning correctly.

---

## 7. Quick Findings

| Check        | Status     | Observation                                   |
| ------------ | ---------- | --------------------------------------------- |
| CPU Usage    | ✅ Normal   | 0.0% - daemon idle, PID 674                   |
| Memory       | ✅ Normal   | 0.1% - no pressure, 118MB free                |
| Disk         | ⚠️ Monitor | 16% root used, logs at 19MB                   |
| Port Binding | ✅ Normal   | Listening on :22 (IPv4 + IPv6)                |
| Auth Logs    | ✅ Clean    | No brute-force or unauthorized attempts found |
| Swap         | ✅ Normal   | swpd=0 - swap completely unused               |

---

## 8. If This Worsens — Next Steps

**1. Block brute-force IPs with `fail2ban`**

```bash
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban
sudo fail2ban-client status sshd   # verify SSH jail is active
```

> If brute-force attacks increase, check `/etc/fail2ban/jail.local` and tighten `maxretry` and `bantime`.Fail2Ban is a log-monitoring security tool that protects Linux servers from brute-force attacks. It monitors logs such as `/var/log/auth.log`, detects repeated failed login attempts, and automatically blocks the offending IP address using firewall rules.


**2. Increase SSH log verbosity**

Edit `/etc/ssh/sshd_config`:

```
LogLevel VERBOSE
```

Then: `sudo systemctl restart sshd`

> "This captures more detail about key exchange failures, which helps diagnose intermittent auth issues.`/etc/ssh/sshd_config` is the main configuration file for the OpenSSH server. It controls SSH server behavior such as listening port, authentication methods, root login permissions, allowed users, and security settings. Any changes require validation and restarting the SSH service."


**3. Collect connection-level traces with `strace`**

```bash
sudo strace -p $(pgrep -o sshd) -e trace=network,read,write -o /tmp/sshd-trace.txt
```

> "This command attaches `strace` to the oldest SSH daemon process and captures only network, read, and write system calls. The trace output is written to `/tmp/sshd-trace.txt` and can be used to troubleshoot SSH connectivity and communication issues."

---
