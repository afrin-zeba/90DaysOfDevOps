**Core Directories (Must Know):**

| Directory  | What it is used for                                                   |
| ---------- | --------------------------------------------------------------------- |
| `/`        | Root directory - the top-level starting point of the Linux filesystem |
| `/home`    | Stores home directories and personal files of normal users            |
| `/root`    | Home directory of the root/admin user -> Files and configs specifically for the root user|
| `/etc`     | Stores system and application config files                            |
| `/var/log` | Stores system, application, and service log files/ caches etc         |
| `/tmp`     | Stores temporary files created by users or applications during runtime|
| `/bin`     | Essential commands needed for booting and basic system recovery like ls,cp|
| `/usr/bin` | Most regular user applications and non-critical binaries              |
| `/opt`     | Stores third-party or manually installed applications                 |
## bin is often a symlink to /usr/bin

- `/` (root) - The starting point of everything
# what does it contain 
bin,home,mnt,usr,var 

- `/home` - User home directories
# what does it contain 
all user directories eg:- afrinz, ubuntu

- `/root` - Root user's home directory
# what does it contain 
.profile, .ssh, .config

- `/etc` - Configuration files
# what does it contain 
hosts file, logrotate.d, cron.d, localtime 

- `/var/log` - Log files (very important for DevOps!)
# what does it contain 
system logs - which are fetched by journalctl 

- `/tmp` - Temporary files
# what does it contain 
empty - temp files can get created as apps run 

**Additional Directories (Good to Know):**
- `/bin` - Essential command binaries
# what does it contain 
contains all binaries - mkdir, cp, mv 

- `/usr/bin` - User command binaries
# what does it contain 
contains all binaries - mkdir, cp, mv 

- `/opt` - Optional/third-party applications
tomcat, kafka, mongodb

-------------------------------------------------------------------------

## **Hands-on task:**

root@ubuntu-host /opt ➜  du -sh /var/log/* 2>/dev/null | sort -h | tail -5
8.0M    /var/log/journal


root@ubuntu-host /opt ➜  ls -la ~
total 44
drwx------ 1 root root 4096 May 21 01:40 .
dr-xr-xr-x 1 root root 4096 May 21 01:52 ..
-rw-r--r-- 1 root root  561 May 21 01:37 .bash_profile
-rw-r--r-- 1 root root 3135 Nov 22  2024 .bashrc
drwx------ 3 root root 4096 May 21 01:40 .cache
drwxr-xr-x 1 root root 4096 Nov 22  2024 .config
-rw-r--r-- 1 root root  161 Apr 22  2024 .profile
drwx------ 1 root root 4096 May 21 01:39 .ssh
drwx------ 2 root root 4096 May 21 01:40 .terminal_logs
drwxr-xr-x 2 root root 4096 May 21 01:40 test-codes

root@ubuntu-host /opt ➜  cat /etc/hostname 
ubuntu-host

## Scenario based task

## 1. Check if a service is running (eg-nginx)

1. `systemctl status nginx` — Check if service is active, failed, or stopped.
2. `systemctl list-units --type=service` — List all available services if nginx is not found.
3. `systemctl is-enabled nginx` — Check if service starts automatically on boot.

---

## 2. Service not starting after reboot

1. `systemctl status myapp` — Check whether service is failed or inactive.
2. `journalctl -u myapp -n 50` — View recent logs for error messages.
3. `systemctl is-enabled myapp` — Verify whether service is configured to start on boot.
4. `systemctl restart myapp` — Try restarting the service manually after checking logs.

---

## 3. High CPU usage on server

1. `top` — Monitor live CPU and process usage.
2. `ps aux --sort=-%cpu | head -10` — Show top CPU-consuming processes.
3. `pgrep <process-name>` — Find PID of suspicious process if needed.
4. `systemctl status <service-name>` — Inspect the service associated with the high CPU process.

---

## 4. Finding logs for a systemd service

1. `systemctl status docker` — Check service state and recent logs.
2. `journalctl -u docker -n 50` — View last 50 log lines.
3. `journalctl -u docker -f` — Follow logs live in real time.
4. `journalctl -u docker --since "1 hour ago"` — View logs from a specific time range.

---

## 5. Fixing "Permission denied" on script

1. `ls -l /home/user/backup.sh` — Check current file permissions.
2. `chmod +x /home/user/backup.sh` — Add execute permission.
3. `ls -l /home/user/backup.sh` — Verify execute permission was added.
4. `./backup.sh` — Run the script again.