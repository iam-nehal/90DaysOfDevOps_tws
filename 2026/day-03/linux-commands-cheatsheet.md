Build a practical Linux command cheat sheet that I can use daily as a DevOps Engineer for troubleshooting, administration, and system monitoring.

# Linux Command Cheat Sheet

## ⚙️ Process Management

**Manage system resources, view running tasks, and control process execution.**

| Command              | Usage Example     | Description                                                                |
| :------------------- | :---------------- | :------------------------------------------------------------------------- |
| **`ps`**             | `ps aux`          | Displays a detailed snapshot of all currently running processes.           |
| **`top`**            | `top`             | Shows real-time system statistics and a dynamic list of processes.         |
| **`htop`**           | `htop`            | An interactive, colorful process viewer.(if installed)                     |
| **`kill`**           | `kill 1234`       | Terminates the process with PID `1234`.                                    |
| **`killall`**        | `killall firefox` | Kills all processes named "firefox".                                       |
| **`pkill`**          | `pkill -u user`   | Signals processes based on name or other attributes (e.g., specific user). |
| **`bg`**             | `bg %1`           | Move job to background                                                     |
| **`fg`**             | `fg %1`           | Brings a background job to the foreground                                  |
| **`ps -ef`**         | `ps -ef`          | View all running processes                                                 |
| `pgrep service`      | `pgrep nginx`     | Find PID of a process                                                      |
| `pidof service`      | `pidof sshd`      | Get PID by process name                                                    |
| `kill -9 PID`        | `kill -9 1234`    | Force kill a process with PID 1234                                         |
| `pkill process name` | `pkill nginx`     | Kill process by name                                                       |
| `nice -n 10 command` |                   | Start process with lower priority                                          |
| `renice 5 PID`       |                   | Change running process priority                                            |
| `jobs`               |                   | show background                                                            |



## 📂 File System


**Navigate directories, manipulate files, and manage permissions.**

| Command            | Usage Example           | Description                                                          |
| :----------------- | :---------------------- | :------------------------------------------------------------------- |
| **`ls`**           | `ls -lah`               | Lists directory contents with details, including hidden files.       |
| **`cd directory`** | `cd /var/log`           | Changes the current working directory.                               |
| **`pwd`**          | `pwd`                   | Prints the full path of the current working directory.               |
| **`cp`**           | `cp -r source dest`     | Copies files or directories recursively.                             |
| **`mv`**           | `mv file.txt new.txt`   | Moves or renames files and directories.                              |
| **`rm`**           | `rm -rf dir_name`       | Removes files or directories (use caution with `-rf`).               |
| **`mkdir`**        | `mkdir -p /a/b/c`       | Creates a new directory with parent directories                      |
| **`chmod`**        | `chmod 755 script.sh`   | Changes file access permissions (read, write, execute).              |
| **`chown`**        | `chown user:group file` | Changes file owner and group.                                        |
| **`df`**           | `df -h`                 | Displays disk space usage for file systems in human-readable format. |
| **`du`**           | `du -sh folder`         | Estimates file space usage for a specific directory.                 |
| `cd ..`            | `cd ..`                 | Move one level up from the current directory                         |
| `cd or cd ~`       | `cd or cd ~`            | Go to home directory                                                 |
| `cd -`             | `cd -`                  | Go to previous directory                                             |
| `touch`            | `touch file.txt`        | Create file                                                          |
| `mkdir`            | `mkdir test`            | Create test name directory                                           |
| `cat`              | `cat file.txt`          | Display file content                                                 |
| `less `            | `less file.txt`         | View large file                                                      |
| `head`             | `head file.txt`         | show first 10 lines                                                  |
| `tail`             | `tail file.txt`         | show last 10 lines                                                   |
| `tail -f`          | `tail -f logfile.log`   | live log monitoring                                                  |
| `find`             | `find / -name file.txt` | Find file having name file in / directory                            |
| `find`             | `find . -type -f`       | Find all files in current directory                                  |
| `locate`           | 'locate nginx.conf'     | Fast file search (need to install)                                   |
| `which`            | `which bash`            | Locate executable means where bash command located                   |
| `whereis`          | `whereis ssh`           | Locate binary and man pages                                          |
| `df -h`            | `df -h`                 | Disk space usage                                                     |
| `du -sh`           | `du -sh`                | Directory size                                                       |
| `lsblk`            | `lsblk`                 | block devices                                                        |
| `mount`            | `mount`                 | Mounted filesystems                                                  |



## 🌐 Networking Troubleshooting

**Diagnose connectivity issues, check configurations, and transfer data.**

| Command          | Usage Example                 | Description                                                                          |
| :--------------- | :---------------------------- | :----------------------------------------------------------------------------------- |
| **`ip addr`**    | `ip addr show`                | Displays IP addresses and network interface properties.                              |
| **`ping`**       | `ping -c 4 google.com`        | Checks connectivity to a host by sending ICMP echo requests. -c 4 sends four packets |
| **`curl`**       | `curl -I https://example.com` | Transfers data from or to a server; `-I` fetches headers only.                       |
| **`dig`**        | `dig google.com`              | DNS lookup utility to query DNS name servers, DNS detailed info                      |
| **`traceroute`** | `traceroute google.com`       | Traces the route packets take to a network host.                                     |
| `ip a`           | `ip a`                        | short form of ip addr                                                                |
| `hostname -I`    | `hostname -I `                | Display system IP                                                                    |
| `ip route`       | `ip route`                    | View routing table                                                                   |
| `tracepath`      | `tracepath google.com`        | Route diagnostics                                                                    |
| `nslookup`       | `nslookup google.com`         | DNS lookup                                                                           |
| `host`           | `host google.com`             | DNS query                                                                            |
