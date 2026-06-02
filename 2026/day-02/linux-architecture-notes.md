Welcome to the Linux System Fundamentals guide. This document covers the core architecture, boot process, detailed file system hierarchy, and essential commands for daily operations.

---

## 1. Linux Architecture

The Linux operating system is built in layers, facilitating communication between the user and the physical hardware.

### The Layers

1. **Hardware:** The physical computer components (CPU, RAM, Disk).
2. **Kernel:** The core of Linux. It is the program that the machine actually understands and acts as the bridge to the hardware.

    **Responsibilities:**

- CPU management
- Memory management
- Process management
- Device driver management
- File system management
- Network communication

**Example:**  
When an application wants to read a file, it sends a request to the kernel, which interacts with the storage device and returns the data.
    
3. **Shell:** An interactive program that takes commands from the user and sends them to the kernel.
4. **Applications / User:** The software programs (like web browsers) and the human users interacting with the system.

Examples:
Bash shell
VS Code
Firefox/Chrome
Docker CLI
System utilities (ls, cp, grep)

> **Key Concept:** High-level code (like C Programs) is converted into binary (`1010`) so the machine can execute it.


## 2. The Boot Process

The sequence of events that occurs when a Linux system is powered on.

1. **Power On:** The system receives power.
2. **BIOS:** The motherboard initializes and performs a hardware check.
3. **GNU GRUB:** The bootloader that loads the Linux Kernel into memory.
4. **OS Loading:** The operating system begins to load (e.g., "Ubuntu Loading...").
5. **Init Process / Systemd:** The very first process started by the kernel (Process ID 1), which then starts all other services.

## 3.Init/Systemd

When Linux boots, the kernel starts the first process called **PID 1**.

On modern Linux distributions, PID 1 is usually **systemd**.

**Responsibilities:**

- Starts system services
- Manages background processes
- Handles service dependencies
- Maintains system state during runtime

Hardware
   ↓
Kernel
   ↓
systemd (PID 1)
   ↓
Services (nginx, sshd, docker)
   ↓
User Applications (bash, VS Code, browsers)


## 4. File System Hierarchy

Linux uses a tree-like directory structure starting from the root (`/`). All other directories and files branch off from this single point.

| Directory | Description                                                                                 |
| :-------- | :------------------------------------------------------------------------------------------ |
| **`/`**   | **The Root.** The starting point of the filesystem hierarchy.                               |
| `/home`   | Home directory for standard users (e.g., `/home/john`). Stores personal documents.          |
| `/root`   | The home directory specifically for the **root** (admin) user.                              |
| `/boot`   | Contains static bootloader and kernel executables.                                          |
| `/etc`    | Contains system-wide **configuration** files and shell scripts.                             |
| `/bin`    | **User Binaries.** Essential commands used by all users (e.g., `ls`, `cp`, `ping`).         |
| `/sbin`   | **System Binaries.** Utilities used by the root user for administration (e.g., `shutdown`). |
| `/usr`    | **User Programs.** The majority of user utilities and applications.                         |
| `/var`    | **Variable Files.** Files that change size over time, such as system **logs** (`/var/log`). |
| `/dev`    | **Device Files.** Entry points for hardware (e.g., `/dev/sda` for hard drives).             |
| `/opt`    | **Optional.** Used for installing add-on application software packages.                     |
| `/tmp`    | **Temporary.** Stores temporary files; usually deleted upon reboot.                         |
| `/lib`    | **Libraries.** Shared library files needed by binaries in `/bin` and `/sbin`.               |
| `/media`  | **Removable Media.** Mount point for USB drives, CD-ROMs, etc.                              |
| `/proc`   | **Process Info.** A virtual filesystem providing info about running processes and kernel.   |

## 5. Essential Commands & Tasks

Common commands for managing files, network, and system resources.

### Navigation & File Management


- **Change Directory:** `cd <path>`
- **Create a Folder:** `mkdir <folder_name>`
- **Create an Empty File:** `touch <file_name>`
- **Copy a File:** `cp <source> <destination>`
- **Move or Rename:** `mv <old_name> <new_name>`
    - _Usage:_ `mv file.txt /home/user/docs` (Move)
    - _Usage:_ `mv file.txt newname.txt` (Rename)
- **Remove (Delete):** `rm <file_name>`
    - To delete a folder, use `rm -r <folder_name>`.
    - Warning: be cautious while using, `rm -f`

### Network Operations

- **Find IP Address:** `ip addr`
- **Check Connectivity:** `ping <website>`
    - _Example:_ `ping google.com`

### System Monitoring

- **Check Storage Usage:** `df -h`
    - _(-h displays sizes in human-readable GB/MB)_
- **Check RAM Usage:** `free -h`

### 6.Process States

|State|Meaning|
|---|---|
|Running (R)|Currently executing|
|Sleeping (S)|Waiting for an event|
|Stopped (T)|Paused|
|Zombie (Z)|Finished but not cleaned up|
|Dead|Completely terminated|

| Command                         | Use                           |
| ------------------------------- | ----------------------------- |
| `ps aux`                        | Check process status          |
| `top`                           | Realtime check process status |
| `ps -ef`                        | Process Management Commands   |
| `pgrep <service>` `pgrep nginx` | Find process:                 |
| `kill <PID>`                    | Terminate process             |
| `kill -9 <PID>`                 | Force terminate               |

### Service Management

| Command                        | Use             |
| ------------------------------ | --------------- |
| `sudo systemctl start nginx`   | Start service   |
| `sudo systemctl stop nginx`    | Stop service    |
| `sudo systemctl restart nginx` | Restart service |
| `systemctl status nginx`       | Check status    |

#### Boot Management

| Command                        | Use             |
| ------------------------------ | --------------- |
| `sudo systemctl enable nginx`  | Enable service  |
| `sudo systemctl disable nginx` | Disable service |

#### Logging with journalctl

| Command               | Use               |
| --------------------- | ----------------- |
| `journalctl`          | View logs         |
| `journalctl -u nginx` | View service logs |
| `journalctl -xe`      | View latest logs  |
