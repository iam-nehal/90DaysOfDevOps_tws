## Part 1: Linux File System Hierarchy

### 1. `/` (Root Directory)

**Purpose:**

- This is the top-level directory in Linux.
- Everything in Linux starts from `/`.

**Check:**

```
ls -l /
```

**Common folders you'll see:**

```
lrwxrwxrwx   1 root root     7 Apr 20 08:46 bin -> usr/bin
drwxr-xr-x   5 root root  4096 Jun  4 06:23 boot
drwxr-xr-x  16 root root  3420 Jun  8 05:53 dev
drwxr-xr-x 108 root root  4096 Jun  7 13:39 etc
drwxr-xr-x   3 root root  4096 Jun  2 06:06 home
lrwxrwxrwx   1 root root     7 Apr 20 08:46 lib -> usr/lib
lrwxrwxrwx   1 root root     9 Apr 20 08:46 lib64 -> usr/lib64
drwx------   2 root root 16384 Apr 21 05:21 lost+found
drwxr-xr-x   2 root root  4096 Apr 21 05:18 media
drwxr-xr-x   2 root root  4096 Apr 21 05:18 mnt
drwxr-xr-x   2 root root  4096 Apr 21 05:18 opt
dr-xr-xr-x 177 root root     0 Jun  8 05:53 proc
drwx------   4 root root  4096 Jun  2 06:06 root
drwxr-xr-x  32 root root   960 Jun  8 06:58 run
lrwxrwxrwx   1 root root     8 Apr 20 08:46 sbin -> usr/sbin
drwxr-xr-x   6 root root  4096 Apr 21 05:25 snap
drwxr-xr-x   2 root root  4096 Apr 21 05:18 srv
dr-xr-xr-x  13 root root     0 Jun  8 05:53 sys
drwxrwxrwt  13 root root   260 Jun  8 09:59 tmp
drwxr-xr-x  12 root root  4096 Apr 21 05:18 usr
drwxr-xr-x  13 root root  4096 Jun  2 06:06 var

```

**I would use this when:**

> I need to understand the overall Linux filesystem structure or navigate to major system directories.

---

## 2. `/home`

**Purpose:**

- Contains home directories of normal users.
- Each user gets a personal workspace.

**Check:**

```
ls -l /home
```

**Example:**

```
drwxr-x--- 5 ubuntu ubuntu 4096 Jun  8 07:11 ubuntu
```

**I would use this when:**

> I need to access user files, scripts, SSH keys, or personal configurations.

---

## 3. `/root`

**Purpose:**

- Home directory of the root user.
- Similar to `/home/user` but specifically for root.

**Check:**

```
sudo ls -la /root
```

**Common files:**

```
drwx------  4 root root 4096 Jun  2 06:06 .
drwxr-xr-x 19 root root 4096 Jun  8 05:53 ..
-rw-r--r--  1 root root 3106 Apr 20 08:46 .bashrc
-rw-r--r--  1 root root  132 Apr 20 08:46 .profile
drwx------  2 root root 4096 Jun  2 06:06 .ssh
drwx------  3 root root 4096 Jun  2 06:06 snap
```

**I would use this when:**

> I am logged in as root and need to access root-specific files.

---

## 4. `/etc`

**Purpose:**

- Stores system-wide configuration files.
- Most Linux service configurations are located here.

**Check:**

```
ls /etc
```

**Common files/directories:**

```
ModemManager            cron.monthly    gnutls              ld.so.conf      modules                 plymouth      selinux            tmpfiles.d
PackageKit              cron.weekly     gprofng.rc          ld.so.conf.d    modules-load.d          pm            sensors.d          tpm2-tss
X11                     cron.yearly     groff               ldap            mtab                    polkit-1      sensors3.conf      ubuntu-advantage
acpi                    crontab         group               legal           multipath               pollinate     services           ucf.conf
adduser.conf            crypttab        group-              libaudit.conf   multipath.conf          ppp           sgml               udev
alternatives            dbus-1          grub.d              libblockdev     nanorc                  profile       shadow             udisks2
apparmor                debconf.conf    gshadow             libibverbs.d    needrestart             profile.d     shadow-            ufw
apparmor.d              debian_version  gshadow-            libnl-3         netconfig               protocols     shells             update-manager
apport                  debuginfod      gss                 locale.conf     netplan                 python3       skel               update-motd.d
apt                     default         hdparm.conf         locale.gen      network                 python3.14    sos                update-notifier
bash.bashrc             deluser.conf    hibinit-config.cfg  localtime       networkd-dispatcher     rc0.d         ssh                updatedb.conf
bash_completion         depmod.d        host.conf           logcheck        networks                rc1.d         ssl                usb_modeswitch.conf
bash_completion.d       dhcp            hostname            login.defs      newt                    rc2.d         subgid             usb_modeswitch.d
bindresvport.blacklist  dhcpcd.conf     hosts               logrotate.conf  nftables.conf           rc3.d         subgid-            vconsole.conf
binfmt.d                dpkg            hosts.allow         logrotate.d     nsswitch.conf           rc4.d         subuid             vim
ca-certificates         dracut.conf     hosts.deny          lsb-release     opt                     rc5.d         subuid-            vmware-tools
ca-certificates.conf    dracut.conf.d   init.d              lvm             os-release              rc6.d         sudo.conf          vtrgb
chrony                  e2scrub.conf    initramfs-tools     machine-id      overlayroot.conf        rcS.d         sudo_logsrvd.conf  wgetrc
cloud                   ec2_version     inputrc             magic           overlayroot.local.conf  resolv.conf   sudoers            xattr.conf
console-setup           environment     iscsi               magic.mime      pam.conf                rmt           sudoers.d          xdg
credstore               ethertypes      issue               manpath.config  pam.d                   rpc           supercat           xml
credstore.encrypted     fstab           issue.net           mdadm           passwd                  rsyslog.conf  sysctl.d           zsh_command_not_found
cron.d                  fuse.conf       kernel              mime.types      passwd-                 rsyslog.d     sysstat
cron.daily              fwupd           landscape           mke2fs.conf     perl                    screenrc      systemd
cron.hourly             gai.conf        ld.so.cache         modprobe.d      pki                     security      terminfo
```

**I would use this when:**

> I need to modify service configurations such as SSH, networking, DNS, or system settings.

---

## 5. `/var/log`

**Purpose:**

- Contains system and application log files.
- One of the most important directories for troubleshooting.

**Check:**

```
ls -l /var/log
```

**Common files:**

```
README            apport.log  auth.log.1  cloud-init-output.log  dmesg       dmesg.2.gz  dpkg.log  kern.log.1  private   sysstat
alternatives.log  apt         btmp        cloud-init.log         dmesg.0     dmesg.3.gz  journal   landscape   syslog    unattended-upgrades
amazon            auth.log    chrony      dist-upgrade           dmesg.1.gz  dmesg.4.gz  kern.log  lastlog     syslog.1  wtmp

```

**I would use this when:**

> I need to investigate login issues, service failures, application errors, or system events.

---

## 6. `/tmp`

**Purpose:**

- Stores temporary files.
- Usually cleared automatically after reboot.

**Check:**

```
ls -la /tmp
```

**Common items:**

```
systemd-private-*tmp.*
```

**I would use this when:**

> I need temporary storage for scripts, downloads, testing, or troubleshooting files.

---

# Additional Directories

---

## 7. `/bin`

**Purpose:**

- Contains essential Linux commands required for booting and basic operation.

**Check:**

```
ls -l /bin
```

**Examples:**

```
ls
cp
mv
cat
```

**I would use this when:**

> I need core Linux commands required for system operation.

---

## 8. `/usr/bin`

**Purpose:**

- Contains most user-level commands and applications.

**Check:**

```
ls -l /usr/bin
```

**Examples:**

```
grep
awk
sed
vim
curl
```

**I would use this when:**

> I need to locate executable binaries or verify installed software.

---

## 9. `/opt`

**Purpose:**

- Used for optional or third-party applications.

**Check:**

```
ls -l /opt
```

**Examples:**

```
google
docker
custom-app
```

**I would use this when:**

> I need to manage software that is not installed through the default package manager.

----
# Linux Filesystem Hierarchy (FHS)

Quick revision guide for Linux filesystem hierarchy, with **examples**, **key points**, and **memory tips** .

---

## Root Directory `/`
- **Description:** Top-level directory; all files start here.
- **Exam Tip:** Always start your path from `/`.
- **Example:** `/etc/passwd`

---

## Essential Commands

| Directory | Purpose | Example | Tip |
|-----------|---------|---------|----------|
| `/bin`   | Basic user commands | `/bin/ls`, `/bin/cp` | Commands needed to boot & run system |
| `/sbin`  | Admin/system commands | `/sbin/reboot`, `/sbin/fsck` | Only root/system admin uses these |

---

## Configuration Files

| Directory | Purpose | Example | Tip |
|-----------|---------|---------|----------|
| `/etc`   | System-wide configuration | `/etc/ssh/sshd_config` | Think: "E" for **Edit config** |
| `/root`  | Root user home | `/root/.bashrc` | Root’s personal files |

---

## User Data & Applications

| Directory | Purpose | Example | Tip |
|-----------|---------|---------|----------|
| `/home`  | User personal files | `/home/alex/document.txt` | Each user has their own folder |
| `/usr`   | User applications | `/usr/bin/python3` | Non-essential system programs |
| `/lib`   | Essential libraries | `/lib/libc.so.6` | Needed by binaries in `/bin` & `/sbin` |

---

## Logs, Temporary & Variable Data

| Directory | Purpose | Example | Tip |
|-----------|---------|---------|----------|
| `/var`   | Variable data/logs | `/var/log/syslog` | Think: "Var = Variable" |
| `/tmp`   | Temporary files | `/tmp/test.tmp` | Files cleared on reboot |

---

## Boot & Devices

| Directory | Purpose | Example | Tip |
|-----------|---------|---------|----------|
| `/boot`  | Bootloader/kernel | `/boot/vmlinuz` | Kernel files live here |
| `/dev`   | Device files | `/dev/sda` | Devices are files |
| `/proc`  | Process info (virtual) | `/proc/cpuinfo` | Think: "Proc = Process" |
| `/sys`   | Kernel & hardware info | `/sys/class/net` | System info interface |

---

## Mount Points

| Directory | Purpose | Example | Tip |
|-----------|---------|---------|----------|
| `/mnt`   | Temporary mounts | `/mnt/backup` | Manual mounts go here |
| `/media` | Removable media | `/media/usb` | USB/CD drives appear here |

---

## Quick Memory Tricks 

- Short phrase:  
  **“Configs in `/etc`, Logs in `/var`, Users in `/home`, Commands in `/bin`”**

---

## Tips

1. **Root `/` is the starting point** – paths always start here.
2. **Remember `/bin` vs `/sbin`** – essential vs admin commands.
3. **Configuration lives in `/etc`**, user files in `/home`.
4. **Logs in `/var/log`**, temporary files in `/tmp`.
5. **Kernel & boot in `/boot`, devices in `/dev`**.
6. **Virtual dirs `/proc` & `/sys`** – they do not occupy disk space.

---

## �️ Part 2: Scenario-Based Practice

> **Key mindset:** Don't memorize commands - understand the **troubleshooting flow**. Always ask: *What do I check first? What does the output tell me? What's my next step?*

---

### ✅ Solved Example: Check if a Service is Running

**Question:** How do you check if `nginx` is running?

```bash
# Step 1: Check service status
systemctl status nginx
# Why: Shows if the service is active, failed, or stopped - your first clue.

# Step 2: If not found, list all services
systemctl list-units --type=service
# Why: See what services actually exist on this system.

# Step 3: Check if it's enabled on boot
systemctl is-enabled nginx
# Why: Know if it will auto-start after a reboot.
```

> **What I learned:** Always check `status` first. The output tells you what to investigate next.

---

### � Scenario 1: Service Not Starting

**Situation:** A web application service called `myapp` failed to start after a server reboot.

**Troubleshooting Flow:**

```bash
# Step 1: Check if the service is running or failed or stopped
systemctl status myapp
# Why: This gives you the current state (active/failed/inactive) and the last few log lines - often the error is right here.

# Step 2: View the last 50 lines of service logs
journalctl -u myapp -n 50
# Why: Detailed logs show exactly what error caused the failure (port conflict, missing file, config error, etc.)

# Step 3: Check if the service is enabled to start on boot
systemctl is-enabled myapp
# Why: If it says "disabled", the service won't start automatically after reboot - this is likely the root cause.

# Step 4: Enable it and try starting it manually
systemctl enable myapp
systemctl start myapp
# Why: Enable makes it start on future reboots; start fires it up right now.
```

> **What I learned:** The most common causes are: service disabled on boot, a config error, or a port already in use. `journalctl` almost always tells you exactly what went wrong.

---

### � Scenario 2: High CPU Usage

**Situation:** Your manager says the server is slow. You SSH in and need to find what's hogging the CPU.

**Troubleshooting Flow:**

```bash
# Step 1: Open live process monitor sorted by CPU
top
# Why: Shows real-time CPU usage per process. The top process is your suspect.
# (Press 'q' to quit)

# Step 2: Alternative - cleaner view with htop
htop
# Why: More readable than top, with colour coding. Sort by CPU with F6.

# Step 3: Quick snapshot sorted by CPU %
ps aux --sort=-%cpu | head -10
# Why: Non-interactive - useful in scripts. Shows top 10 CPU-hungry processes instantly.

# Step 4: Note the PID and investigate further
ls -l /proc/<PID>
# Why: Every running process has a folder in /proc - you can inspect it to find what the process is doing.
```

```
What does /proc means???

/proc is a virtual/fake directory - it doesn't actually exist on your hard disk. Linux creates it in memory and fills it with live information about everything running on your system right now.
Think of it like a live dashboard that Linux maintains for you.

For every running process, Linux creates a folder named after its PID (Process ID):
/proc/1234/     ← all info about the process with PID 1234
/proc/5678/     ← all info about process 5678

```

> **What I learned:** `top` and `ps` are your first tools. Once you have the PID, you can kill the process with `kill <PID>` or `kill -9 <PID>` for forceful termination.

---

### � Scenario 3: Finding Service Logs

**Situation:** A developer asks: *"Where are the logs for the `docker` service?"*

> systemd services → logs are in journald
> `journald` is a background service that runs silently and collects logs from everything - system, services, kernel, boot, etc.
> journald stores logs in a binary format (not plain text) at:
 `/run/log/journal/`, Because it's binary, you can't just cat it like a normal file.
> journalctl is simply the tool to read what journald collected.

| | `/var/log` | `journald` |
|---|---|---|
| **Age** | Old (decades) | Modern (systemd era) |
| **Format** | Plain text files | Binary (need journalctl) |
| **Who uses it** | Old apps, some system files | All systemd services |
| **How to read** | `cat`, `tail`, `grep` | `journalctl` |

**Troubleshooting Flow:**

```bash
# Step 1: Check service status (always start here)
systemctl status docker
# Why: Confirms if docker is running AND shows the last few log lines right there.

# Step 2: View the last 50 log lines
journalctl -u docker -n 50
# Why: systemd services log to journald. This is how you read them - replace 'docker' with any service name.

# Step 3: Follow logs in real-time (like tail -f)
journalctl -u docker -f
# Why: Great for watching what happens LIVE - useful when debugging a startup issue or reproducing a bug.

# Step 4: Filter logs by time
journalctl -u docker --since "1 hour ago"
# Why: When you know roughly when the issue occurred, narrow down the logs to that window.

```

> **What I learned:** On systemd-based Linux systems (Ubuntu, CentOS 7+), **all service logs go to journald** - not to `/var/log` files directly. `journalctl -u <service-name>` is your go-to command.

---

### � Scenario 4: File Permissions Issue

**Situation:** A script at `/home/user/backup.sh` gives `Permission denied` when you try to run it.

**What's happening:** The file exists, but it doesn't have **execute (`x`) permission**.

**Troubleshooting Flow:**

```bash
# Step 1: Check current permissions
ls -l /home/user/backup.sh
# Why: See exactly what permissions the file has.
# Output example: -rw-r--r-- (no 'x' = not executable!)
```

**Reading the permission string:**

```
-rw-r--r--
│ ││  │  │
│ │└──┴──┴── group & others permissions
│ └───────── owner permissions (r=read, w=write, NO x=execute)
└─────────── file type (- = regular file)
```

```bash
# Step 2: Add execute permission
chmod +x /home/user/backup.sh
# Why: Grants execute permission to owner, group, and others.

# Step 3: Verify it worked
ls -l /home/user/backup.sh
# Why: Confirm the 'x' now appears.
# Output example: -rwxr-xr-x ✅

# Step 4: Run the script
./backup.sh
# Why: Now it should execute without "Permission denied".
```

> **What I learned:** Linux won't execute a file unless it has the `x` bit set. `chmod +x` is the fix. Always verify with `ls -l` before and after.

---

### Troubleshooting - Key Takeaways

| Scenario | First Command | What to Look For |
|----------|--------------|------------------|
| Service not starting | `systemctl status <service>` | Active/failed state + error hint |
| High CPU | `top` or `ps aux --sort=-%cpu` | Process using most CPU + its PID |
| Service logs | `journalctl -u <service> -n 50` | Error messages, timestamps |
| Permission denied | `ls -l <file>` | Missing `x` in permissions |

### The DevOps Troubleshooting Mindset

1. **Check status first** - don't guess, get facts.
2. **Read the logs** - they almost always tell you what's wrong.
3. **Fix, verify, test** - don't assume the fix worked; confirm it.
4. **Understand the why** - knowing *why* a command works beats memorizing it.

---
