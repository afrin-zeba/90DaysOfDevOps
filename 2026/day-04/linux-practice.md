1. Process commands 
### top -> to monitor running processes and system resource usage in real time.
top - 14:09:11 up 5 days,  3:26,  0 users,  load average: 0.06, 0.06, 0.01
Tasks:   4 total,   1 running,   3 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.8 us,  0.0 sy,  0.0 ni, 99.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  32096.1 total,  29159.0 free,    839.6 used,   2097.5 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.  30841.4 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                
      1 sandbox   20   0    4628   3720   3264 S   0.0   0.0   0:00.02 bash                                                                   
     13 sandbox   20   0    4628   3648   3192 S   0.0   0.0   0:00.00 bash                                                                   
     25 sandbox   20   0    4628   3872   3264 S   0.0   0.0   0:00.02 bash                                                                   
     43 sandbox   20   0    7344   2936   2604 R   0.0   0.0   0:00.00 top                                                                    

### ps - aux -> lists all running processes 
root@ubuntu-host ~ ✖ ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  21152 12756 ?        Ss   00:18   0:00 /sbin/init --log-level=err
root         322  0.0  0.0  34208 12536 ?        Ss   00:18   0:00 /usr/lib/systemd/systemd-journald
message+     864  0.0  0.0   8128  4640 ?        Ss   00:18   0:00 @dbus-daemon --system --address=system
root         888  0.0  0.0  17512  8084 ?        Ss   00:18   0:00 /usr/lib/systemd/systemd-logind

### pgrep -> used to fetch pid via process name 
root@ubuntu-host ~ ➜  pgrep systemd-logind
888

### ps -aux | grep sandbox - greps for a process with specific name
sandbox@playground:~$ ps -aux | grep sandbox 
sandbox        1  0.0  0.0   4628  3720 pts/0    Ss+  14:06   0:00 /bin/bash
sandbox       13  0.0  0.0   4628  3648 pts/1    Ss+  14:06   0:00 /bin/bash
sandbox       25  0.0  0.0   4628  3872 pts/2    Ss   14:06   0:00 /bin/bash
sandbox       45  0.0  0.0   7064  1612 pts/2    R+   14:11   0:00 ps -aux
sandbox       46  0.0  0.0   3472  1688 pts/2    S+   14:11   0:00 grep --color=auto sandbox

### kill -9 PID - kills a specific pid
sandbox@playground:~$ kill -9 13
sandbox@playground:~$ ps -aux | grep sandbox 
sandbox        1  0.0  0.0   4628  3720 pts/0    Ss+  14:06   0:00 /bin/bash
sandbox       25  0.0  0.0   4628  3872 pts/2    Ss   14:06   0:00 /bin/bash
sandbox       47  0.0  0.0   7064  1584 pts/2    R+   14:11   0:00 ps -aux
sandbox       48  0.0  0.0   3472  1592 pts/2    S+   14:11   0:00 grep --color=auto sandbox

-------------------------------------------------------------------------------------------------------------------------

2. SERVICE COMMANDS 

### check running services 
root@ubuntu-host ~ ➜  systemctl list-units --type=service
  UNIT                                     LOAD   ACTIVE SUB     DESCRIPTION                                  
  dbus.service                             loaded active running D-Bus System Message Bus
● kmod-static-nodes.service                loaded failed failed  Create List of Static Device Nodes
  ldconfig.service                         loaded active exited  Rebuild Dynamic Linker Cache
  ssh.service                              loaded active running OpenBSD Secure Shell server
  systemd-journal-catalog-update.service   loaded active exited  Rebuild Journal Catalog
  systemd-journal-flush.service            loaded active exited  Flush Journal to Persistent Storage
  systemd-journald.service                 loaded active running Journal Service

### start a service and check the status of a service 
root@ubuntu-host ~ ➜  systemctl start ssh

root@ubuntu-host ~ ➜  systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Thu 2026-05-21 00:18:09 EDT; 22min ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 881 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 921 (sshd)
      Tasks: 1 (limit: 77149)
     Memory: 1.2M (peak: 18.5M)
        CPU: 161ms

### stop a service
root@ubuntu-host ~ ➜  systemctl stop sshd
Stopping 'sshd.service', but its triggering units are still active:
ssh.socket

root@ubuntu-host ~ ➜  systemctl status sshd
○ ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: inactive (dead) since Thu 2026-05-21 00:54:20 EDT; 11s ago

-------------------------------------------------------------------------------------------------------------------------
3. LOG COMMANDS 

### checking logs via journalctl
root@ubuntu-host ~ ➜  journalctl -u ssh -f
May 21 00:31:30 ubuntu-host sshd[1729]: Accepted publickey for root from 192.168.80.153 port 57436 ssh2: RSA SHA256:6RjdLRihoxjjxsPr9CJN0pfWDZgIERsdgpOmSf91hOM
May 21 00:31:30 ubuntu-host sshd[1729]: pam_unix(sshd:session): session opened for user root(uid=0) by root(uid=0)
May 21 00:31:30 ubuntu-host sshd[1729]: Received disconnect from 192.168.80.153 port 57436:11: disconnected by user
May 21 00:31:30 ubuntu-host sshd[1729]: Disconnected from user root 192.168.80.153 port 57436

### checking logs via tail
root@ubuntu-host log/journal/05ee70c460a9dd015c31410c674044a5 ➜  tail -50f system.journal 


-------------------------------------------------------------------------------------------------------------------------
4. INSPECTING A SERVICE 
### checking few basic details 
sandbox@playground:~$ ps -o pid,ppid,user,%cpu,%mem,stat,command -p 1
    PID    PPID USER     %CPU %MEM STAT COMMAND
    1       0   sandbox   0.0  0.0 Ss+  /bin/bash

### systemctl output
root@ubuntu-host ~ ➜  systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Thu 2026-05-21 00:55:07 EDT; 1min 27s ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 2652 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 2660 (sshd)
      Tasks: 1 (limit: 77149)
     Memory: 1.2M (peak: 1.5M)
        CPU: 18ms
     CGroup: /system.slice/ssh.service
             └─2660 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 21 00:55:07 ubuntu-host sshd[2660]: Server listening on :: port 22.

### reading logs live
root@ubuntu-host ~ ➜  journalctl -fu ssh --> the f is for live logs
May 21 00:31:30 ubuntu-host sshd[1700]: Connection closed by authenticating user root 192.168.80.153 port 57402 [preauth]
May 21 00:31:30 ubuntu-host sshd[1741]: pam_unix(sshd:session): session opened for user root(uid=0) by root(uid=0)
May 21 00:31:30 ubuntu-host sshd[1741]: Received disconnect from 192.168.80.153 port 57446:11: disconnected by user
May 21 00:31:30 ubuntu-host sshd[1741]: Disconnected from user root 192.168.80.153 port 57446
May 21 00:31:30 ubuntu-host sshd[1741]: pam_unix(sshd:session): session closed for user root
May 21 00:54:20 ubuntu-host sshd[921]: Received signal 15; terminating.
May 21 00:55:07 ubuntu-host sshd[2660]: Server listening on :: port 22.

### fetching pid via name 
root@ubuntu-host ~ ➜  pgrep ssh
2660

### fetching listener port 
root@ubuntu-host ~ ➜  ss -tulnp | grep ssh
tcp   LISTEN 0      4096               *:22              *:*    users:(("sshd",pid=2660,fd=3),("systemd",pid=1,fd=51))



