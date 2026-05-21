# Linux Architecture & Troubleshooting Notes

## 1. Linux Architecture Overview

Linux is layered. Each layer talks only to the one directly below it.

```
+----------------------+
|    Applications      |   ← Chrome, Python, nginx
+----------------------+
|  Shell / Utilities   |   ← bash, zsh, ls, grep
+----------------------+
| systemd / Services   |   ← service management, boot
+----------------------+
|    Linux Kernel      |   ← the core of the OS
+----------------------+
|      Hardware        |   ← CPU, RAM, Disk, NIC
+----------------------+
```

**Common issue types at each layer:**

| Issue Type | What It Means |
|---|---|
| CPU | Processes consuming too much power → slowness, high load averages |
| Memory | RAM exhausted or leaking → swapping, OOM kills, crashes |
| Disk | Full disks, slow I/O, filesystem corruption, failing hardware |
| Networking | DNS issues, firewall rules, routing problems, interface failures |
| Process | Hung, crashed, consuming too many resources, or spawning incorrectly |
| Service startup | Fails due to bad config, missing permissions, dependency failures, port conflicts |

---

## The Kernel — What It Does

The kernel is the core of Linux. It sits between hardware and software, managing everything the system needs to function.

### Responsibilities

#### Process Management
The kernel handles process creation, scheduling, CPU allocation, and priorities.

> **Example:** When Chrome and Spotify run together, the kernel decides who gets CPU time, for how long, and in what order.

#### Memory Management
The kernel allocates and frees RAM, manages virtual memory, and handles swapping.

> **Example:** If RAM becomes full, Linux may move inactive memory pages to swap space on disk.

#### Device Management
The kernel communicates with disks, keyboards, network cards, and USB devices through **device drivers**.

> **Example:** Without a network driver, Linux cannot talk to your Ethernet card.

#### File System Management
Handles reading and writing files, enforcing permissions, and mounting disks.

> **Example:** When you run `cat file.txt`, the kernel retrieves the data from disk and hands it to the shell.

#### Networking
Manages TCP/IP, sockets, routing, and firewall interactions.

> **Example:** Every Kubernetes node depends heavily on Linux networking internals for pod-to-pod communication.

---

## The Shell

The shell is the **command-line interface between you and Linux** — it takes what you type and translates it into kernel instructions.

Common shells: `bash`, `zsh`

**What happens when you type `ls`:**

1. Shell reads your command
2. Shell finds the binary (`/bin/ls`)
3. Shell asks the kernel to execute it
4. Output is printed to your terminal

---

## Kernel Space vs User Space

Linux splits memory into two protected regions:

| | Kernel Space | User Space |
|---|---|---|
| Who runs here | Only the kernel | All applications (Chrome, Python, nginx, bash) |
| Privileges | Full hardware access | Restricted — cannot touch hardware directly |
| How apps interact with hardware | — | Via **system calls** (`read`, `write`, `fork`, etc.) |

Applications in user space must **ask** the kernel to do anything involving hardware. This isolation is what makes Linux stable — a crashing application cannot take down the whole system.

---

## 5. How Processes Are Created and Managed

### What is a Process?

A process is a **running instance of a program**. Every process contains:

- Program code
- Memory
- Process ID (PID)
- Open files
- Environment variables
- Current state
- Threads

### Creation: fork() and exec()

Every process in Linux is born from another process using two system calls:

| System Call | What It Does |
|---|---|
| `fork()` | Parent **clones itself** — child is an exact copy |
| `exec()` | Child **replaces itself** with a new program |

**Example — what happens when you type `ls -l`:**

```
bash
 └─ fork() → creates a copy of bash
      └─ exec(ls) → copy transforms into the ls program
           └─ ls runs, prints output, exits
bash resumes, shows prompt
```

### The Process Family Tree

Every process has:
- **PID** — its own unique ID
- **PPID** — its parent's PID

`systemd` (PID 1) is the ancestor of every process on the system. You can visualise this with:

```bash
pstree          # show full process family tree
ps aux          # list all running processes
echo $$         # show your current shell's PID
```

### Managing Processes

```bash
sleep 100 &         # run process in background
jobs                # list background jobs
fg                  # bring background job to foreground
Ctrl+Z              # pause (stop) a running process

kill 1234           # send SIGTERM — politely ask process to stop
kill -9 1234        # send SIGKILL — force kill, no cleanup

nice -n 10 ./app    # start with lower CPU priority
renice 5 -p 1234    # change priority of a running process
```

---

## 6. Process States

A process moves through different states during its lifetime:

| State | Code | Meaning | Example | Why It Matters |
|---|---|---|---|---|
| Running | `R` | Actively executing on CPU | `python app.py` under heavy load | High CPU troubleshooting |
| Interruptible Sleep | `S` | Waiting for an event; can be woken up | Web server waiting for a request | Normal, healthy state |
| Uninterruptible Sleep | `D` | Waiting on I/O; **cannot** be interrupted | Disk read stuck due to storage issue | Often signals disk/NFS problems |
| Stopped | `T` | Paused | `Ctrl+Z` or `kill -STOP <PID>` | Useful for debugging |
| Zombie | `Z` | Finished but parent hasn't collected exit status | Child exited but parent never called `wait()` | Indicates bad process handling |
| Orphan | — | Parent died before the child finished | Parent crashed mid-execution | Automatically adopted by PID 1 |

### Finding Zombie Processes

```bash
ps aux | grep Z
ps -el | grep Z
```

If zombies accumulate, the parent process is likely not calling `wait()`. Fix: restart the parent.

---

## What systemd Does and Why It Matters

`systemd` is PID 1 — the **first process the kernel starts** after boot. Everything on the system flows from it.

### What systemd Manages

- Starting and stopping services
- Defining startup order and dependencies
- Logging (via `journald`)
- Auto-restarting crashed services
- Mounting filesystems
- Handling system shutdown

### Common systemctl Commands

```bash
systemctl start nginx       # start a service
systemctl stop nginx        # stop a service
systemctl restart nginx     # restart a service
systemctl status nginx      # check if it's running and healthy
systemctl enable nginx      # start automatically on boot
systemctl disable nginx     # don't start on boot

journalctl -u nginx         # view logs for a specific service
journalctl -f               # tail all system logs live
```

### Why PID 1 Matters

`systemd` as PID 1 has special responsibilities:

- Starts the entire system
- Adopts **orphan processes** when their parent dies
- Manages all service lifecycles
- Handles clean shutdown and reboot

> **If PID 1 dies, the entire system crashes.** It's the root of everything.

---

## Services vs Processes

| | Process | Service |
|---|---|---|
| Definition | Any running program | A long-running background process managed by systemd |
| Examples | `ls`, `grep`, your script | `nginx`, `docker`, `postgresql` |
| Managed by | Kernel | systemd |
| Has unit file? | No | Yes (`.service` file) |

**The key distinction:**

```
Every service is a process.
But not every process is a service.
```

---

## What Happens When Linux Boots

```
1. BIOS/UEFI starts
        ↓
2. Bootloader (GRUB) loads the kernel into memory
        ↓
3. Kernel initializes hardware (CPU, RAM, devices)
        ↓
4. Kernel starts PID 1 → systemd
        ↓
5. systemd reads unit files, resolves dependencies
        ↓
6. systemd starts services in parallel
        ↓
7. User space becomes available
        ↓
8. Login prompt appears
```

---

## DevOps Troubleshooting

When something breaks, work through the layers systematically — don't guess.

```
Is the process alive?      →  ps aux | grep <name>
Is the service healthy?    →  systemctl status <service>
Are there errors in logs?  →  journalctl -u <service>
Is CPU overloaded?         →  top  or  htop
Is memory exhausted?       →  free -h
Is disk I/O stuck?         →  iostat  or  iotop
```

---

### Scenario 1 — Service Keeps Crashing

```bash
# Step 1: Check service status
systemctl status app

# Step 2: Read the logs
journalctl -u app --since "10 minutes ago"
```

**Common causes to look for:**

- `Out of memory` → memory limit hit, increase or fix leak
- `No such file or directory` → missing config or binary
- `Permission denied` → wrong file ownership or SELinux
- `Address already in use` → port conflict, find with `ss -tlnp`

---

### Scenario 2 — High CPU

```bash
# Step 1: Find the culprit
top        # press P to sort by CPU
htop       # more visual alternative

# Step 2: Inspect the process
ps -fp <PID>

# Step 3: Check what it's doing
strace -p <PID>     # system calls it's making
lsof -p <PID>       # files it has open
```

---

### Scenario 3 — Zombie Processes

```bash
# Step 1: Find zombies
ps aux | grep Z

# Step 2: Find the parent
ps -eljf | grep Z

# Step 3: Fix
# Option A: Kill the parent (systemd will clean up children)
kill <PPID>

# Option B: Restart the parent service
systemctl restart <service>
```

> Zombies themselves consume no CPU or memory — they're just an entry in the process table. 
