# 🐧 Linux Cheatsheet

> *A curated reference for day-to-day Linux power use.*

---

## 📁 File & Directory Operations

| Command | Usage |
|---------|-------|
| `find / -name "*.conf" 2>/dev/null` | Search for files by name, suppress permission errors |
| `find . -type f -mtime -7` | Find files modified in the last 7 days |
| `locate filename` | Fast filename search using a pre-built index (`updatedb` to refresh) |
| `du -sh *` | Show disk usage of each item in current directory, human-readable |
| `df -h` | Display mounted filesystem disk usage |
| `stat file` | Show detailed file metadata (size, inode, timestamps, permissions) |
| `ln -s /path/to/target linkname` | Create a symbolic (soft) link |
| `rsync -avz src/ dest/` | Sync files/dirs efficiently; `-a` archive, `-v` verbose, `-z` compress |
| `tar -czvf archive.tar.gz dir/` | Create a gzipped tarball of a directory |
| `tar -xzvf archive.tar.gz` | Extract a gzipped tarball |
| `zip -r archive.zip dir/` | Zip a directory recursively |
| `unzip archive.zip -d /path/` | Unzip into a specific directory |
| `chmod 755 file` | Set permissions: owner rwx, group/others rx |
| `chown user:group file` | Change file owner and group |

---

## 🔍 Text Processing & Search

| Command | Usage |
|---------|-------|
| `grep -rn "pattern" ./` | Recursively search for pattern, show line numbers |
| `grep -i "pattern" file` | Case-insensitive search |
| `grep -v "pattern" file` | Show lines that do NOT match the pattern |
| `awk '{print $1, $3}' file` | Print specific columns (fields 1 and 3) from a file |
| `sed 's/old/new/g' file` | Replace all occurrences of "old" with "new" in output |
| `sed -i 's/old/new/g' file` | In-place replacement (modifies the file directly) |
| `cut -d':' -f1 /etc/passwd` | Cut field 1 using `:` as delimiter (e.g., list all usernames) |
| `sort -k2 -n file` | Sort file numerically by the 2nd column |
| `uniq -c` | Count and collapse duplicate adjacent lines (pipe after `sort`) |
| `wc -l file` | Count lines in a file |
| `diff file1 file2` | Show line-by-line differences between two files |
| `cat file \| tr 'a-z' 'A-Z'` | Translate lowercase to uppercase |

---

## ⚙️ Process Management

| Command | Usage |
|---------|-------|
| `ps aux` | List all running processes with CPU/memory usage |
| `ps aux \| grep process_name` | Find a specific process |
| `top` / `htop` | Interactive real-time process viewer (`htop` is more readable) |
| `kill -9 PID` | Force-kill a process by its PID |
| `pkill process_name` | Kill process(es) by name |
| `killall process_name` | Kill all processes with a given name |
| `nice -n 10 command` | Run a command with lower scheduling priority (nice: -20 to 19) |
| `renice 5 -p PID` | Change priority of a running process |
| `nohup command &` | Run command immune to hangups; keep it running after logout |
| `jobs` | List background jobs in the current shell session |
| `fg %1` / `bg %1` | Bring job 1 to foreground / resume job 1 in background |
| `Ctrl+Z` → `bg` | Suspend current process, then push it to background |

---

## 👤 Users & Permissions

| Command | Usage |
|---------|-------|
| `whoami` | Print the current logged-in username |
| `id username` | Show UID, GID, and group memberships of a user |
| `sudo -i` | Switch to root shell (interactive login shell) |
| `su - username` | Switch to another user with their environment |
| `useradd -m username` | Create a new user with a home directory |
| `passwd username` | Set or change a user's password |
| `usermod -aG groupname username` | Add user to a group without removing them from others |
| `groups username` | List all groups a user belongs to |
| `visudo` | Safely edit the sudoers file (use this, not direct edit) |
| `last` | Show recent login history |
| `who` | Show who is currently logged in |

---

## 🌐 Networking

| Command | Usage |
|---------|-------|
| `ip addr` | Show all network interfaces and their IP addresses |
| `ip link set eth0 up/down` | Bring a network interface up or down |
| `ip route show` | Display the kernel routing table |
| `ping -c 4 google.com` | Send 4 ICMP echo requests to check connectivity |
| `traceroute google.com` | Trace the network path to a host, hop by hop |
| `dig google.com` | Full DNS lookup with detailed response (records, TTL, etc.) |
| `dig +short google.com` | Quick DNS lookup — just the IP(s) |
| `nslookup domain.com` | Simple DNS query tool (alternative to dig) |
| `curl -I https://example.com` | Fetch HTTP headers only (check status codes, redirects) |
| `curl -O https://example.com/file.zip` | Download a file, keeping the remote filename |
| `curl -u user:pass https://api.example.com` | Make an authenticated HTTP request |
| `wget -q --show-progress URL` | Download a file with a progress bar, suppress noise |
| `ss -tulnp` | Show listening ports and the processes using them |
| `netstat -tulnp` | Similar to `ss`; may need `net-tools` package |
| `nmap -sV hostname` | Scan host for open ports and service versions |
| `iptables -L -n -v` | List all current firewall rules |
| `ufw status verbose` | Check firewall status (Ubuntu/Debian UFW) |
| `ufw allow 22/tcp` | Allow SSH through the UFW firewall |
| `scp file.txt user@host:/remote/path/` | Securely copy a file to a remote host |
| `ssh -i ~/.ssh/key.pem user@host` | Connect to SSH using a specific private key |
| `ssh -L 8080:localhost:80 user@host` | SSH local port forwarding (tunnel) |

---

## 🖥️ System Info & Hardware

| Command | Usage |
|---------|-------|
| `uname -a` | Show kernel version, architecture, and hostname |
| `lsb_release -a` | Show Linux distribution details |
| `uptime` | Show how long the system has been running and load averages |
| `free -h` | Display total, used, and free RAM/swap in human-readable form |
| `lscpu` | Display CPU architecture and core details |
| `lsblk` | List block devices (disks, partitions) in a tree view |
| `lsusb` | List connected USB devices |
| `lspci` | List PCI devices (GPU, network cards, etc.) |
| `dmesg \| tail -50` | Show last 50 kernel messages (useful for debugging hardware) |
| `journalctl -xe` | View systemd journal logs with context for recent errors |
| `vmstat 1 5` | System performance snapshot (CPU, mem, I/O) every 1s for 5s |
| `iostat -xz 1` | Disk I/O statistics per device, updated every second |

---

## 🔧 Services & Systemd

| Command | Usage |
|---------|-------|
| `systemctl start service` | Start a service |
| `systemctl stop service` | Stop a service |
| `systemctl restart service` | Restart a service |
| `systemctl enable service` | Enable a service to start on boot |
| `systemctl disable service` | Prevent a service from starting on boot |
| `systemctl status service` | Check current status and recent logs of a service |
| `systemctl list-units --type=service` | List all loaded systemd services |
| `journalctl -u service -f` | Follow live logs for a specific service |
| `journalctl --since "1 hour ago"` | Show journal logs from the last hour |

---

## 📦 Package Management

### Debian / Ubuntu (apt)

| Command | Usage |
|---------|-------|
| `apt update && apt upgrade -y` | Refresh package index and upgrade all packages |
| `apt install pkg` | Install a package |
| `apt remove pkg` | Remove a package (keep config) |
| `apt purge pkg` | Remove package and its config files |
| `apt autoremove` | Remove unused dependency packages |
| `dpkg -l \| grep pkg` | Check if a package is installed |

### RHEL / Fedora / CentOS (dnf / yum)

| Command | Usage |
|---------|-------|
| `dnf install pkg` | Install a package |
| `dnf update` | Update all packages |
| `dnf remove pkg` | Remove a package |
| `rpm -qa \| grep pkg` | Check if an RPM package is installed |

---

## 🛠️ Shell Productivity

| Command | Usage |
|---------|-------|
| `history \| grep cmd` | Search command history for a previous command |
| `Ctrl+R` | Reverse interactive search through history |
| `!!` | Repeat the last command |
| `!$` | Use the last argument of the previous command |
| `alias ll='ls -lah'` | Create a shortcut alias (add to `~/.bashrc` to persist) |
| `export VAR=value` | Set an environment variable for the current session |
| `env` | List all environment variables |
| `xargs -I {} cmd {}` | Build and execute commands from stdin input |
| `tee file.txt` | Write stdin to both a file and stdout simultaneously |
| `watch -n 2 command` | Re-run a command every 2 seconds and display output live |
| `time command` | Measure how long a command takes to execute |
| `strace -p PID` | Trace system calls made by a running process |
| `lsof -p PID` | List open files (and sockets) used by a process |
| `lsof -i :80` | Find what process is using port 80 |
| `screen` / `tmux` | Multiplexer: persist sessions across SSH disconnects |

---

## 🔐 SSH & Security

| Command | Usage |
|---------|-------|
| `ssh-keygen -t ed25519` | Generate a modern, secure SSH key pair |
| `ssh-copy-id user@host` | Copy your public key to a remote host for passwordless login |
| `ssh-agent bash && ssh-add ~/.ssh/key` | Start SSH agent and load a private key into it |
| `gpg --gen-key` | Generate a new GPG key pair |
| `gpg --encrypt -r user@email file` | Encrypt a file for a recipient by email |
| `openssl rand -hex 32` | Generate a cryptographically secure random string |
| `openssl s_client -connect host:443` | Inspect a remote TLS certificate |
| `fail2ban-client status` | Check fail2ban status and banned IPs |
| `auditctl -l` | List active Linux audit rules |
| `chattr +i file` | Make a file immutable (not even root can delete it) |

---

## 🧩 One-Liners & Handy Patterns

```bash
# Watch a log file live
tail -f /var/log/syslog

# Find and delete files older than 30 days
find /tmp -type f -mtime +30 -delete

# Count occurrences of each unique line
sort file.txt | uniq -c | sort -rn

# Show top 10 largest files in current directory (recursive)
du -ah . | sort -rh | head -10

# Monitor network bandwidth per interface
watch -n 1 'cat /proc/net/dev'

# Quickly serve current directory over HTTP (Python)
python3 -m http.server 8080

# Check open ports without nmap
ss -tulnp | grep LISTEN

# Decode a base64 string
echo "SGVsbG8=" | base64 -d

# Get your public IP
curl -s https://ifconfig.me
```

---

*Last updated: 2025 · Linux kernel 6.x era*
