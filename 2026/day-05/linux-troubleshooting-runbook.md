1. Environment basics

### uname -a 
root@ubuntu-host log/journal/05ee70c460a9dd015c31410c674044a5 ➜  uname -a
Linux ubuntu-host 6.8.0-1047-gcp #50~22.04.2-Ubuntu SMP Wed Jan 28 01:43:28 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux

### cat /etc/os-release
root@ubuntu-host log/journal/05ee70c460a9dd015c31410c674044a5 ➜  cat /etc/os-release 
PRETTY_NAME="Ubuntu 24.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.1 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
-------------------------------------------------------------------------------------------------------------------------------

2. CPU/ Memory
## free -h
root@ubuntu-host log/journal/05ee70c460a9dd015c31410c674044a5 ➜  free -h
               total        used        free      shared  buff/cache   available
Mem:            62Gi        13Gi       4.0Gi       164Mi        46Gi        49Gi
Swap:             0B          0B          0B

## top
top - 01:10:05 up 51 min,  0 user,  load average: 0.54, 0.77, 0.80
Tasks:  13 total,   1 running,  12 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.1 us,  1.2 sy,  0.0 ni, 96.6 id,  0.0 wa,  0.0 hi,  0.2 si,  0.0 st 
MiB Mem :  64298.3 total,   4073.5 free,  13424.5 used,  47681.8 buff/cache     
MiB Swap:      0.0 total,      0.0 free,      0.0 used.  50873.8 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                           
      1 root      20   0   21348  12904   9800 S   0.0   0.0   0:00.25 systemd                           
    322 root      20   0   34208  12536  11404 S   0.0   0.0   0:00.05 systemd-journal                   
    864 message+  20   0    8128   4640   4140 S   0.0   0.0   0:00.01 dbus-daemon                       
    888 root      20   0   17512   8092   7132 S   0.0   0.0   0:00.16 systemd-logind                    
    914 root      20   0    8064   3544   3280 S   0.0   0.0   0:00.00 start-ttyd.sh                     
    924 root      20   0    9736   1136    972 S   0.0   0.0   0:00.30 ttyd        

 ## ps -o pid,pcpu,pmem,comm -p <pid>
 root@ubuntu-host ~ ➜  ps -o pid,pcpu,pmem,comm -p 323
    PID %CPU %MEM COMMAND
    323  0.0  0.0 systemd-journal
-------------------------------------------------------------------------------------------------------------------------------
3. Disk / IO
## df -h (disk free human readable) --> to check available and used disk space on mounted filesystems
root@ubuntu-host ~ ➜  df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay         737G  153G  547G  22% /
tmpfs            64M     0   64M   0% /dev
tmpfs            32G   44K   32G   1% /run
tmpfs            32G     0   32G   0% /run/lock
tmpfs            32G  8.0M   32G   1% /var/log/journal
shm              64M     0   64M   0% /dev/shm
/dev/root        20G  6.4G   13G  34% /etc/hosts
tmpfs            13G   12M   13G   1% /etc/hostname
tmpfs           3.8G   12K  3.8G   1% /run/secrets/kubernetes.io/serviceaccount
/dev/md127      737G  153G  547G  22% /var/lib/k0s
tmpfs            32G     0   32G   0% /proc/acpi
tmpfs            32G     0   32G   0% /proc/scsi
tmpfs            32G     0   32G   0% /sys/firmware

## du - sh -> to find total disk space used by a specific file or directory 
root@ubuntu-host ~ ➜  du -sh /var/log
8.1M    /var/log

## root@ubuntu-host ~ ✖ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
11  0      0 1288416 9058568 40823064    0    0 9346121 2280442 18605319   48  8  5 87  0  0  0

-------------------------------------------------------------------------------------------------------------------------------
4. Network commands 

## ss -tulpn --> Show listening ports and the processes using them
root@ubuntu-host ~ ➜  ss -tulpn
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process                          
tcp    LISTEN  0       128            0.0.0.0:8080        0.0.0.0:*      users:(("ttyd",pid=925,fd=12))  
tcp    LISTEN  0       4096                 *:22                *:*      users:(("sshd",pid=920,fd=3),("systemd",pid=1,fd=36))

## netstat -tulpn 
root@ubuntu-host ~ ➜  netstat -tulpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      925/ttyd            
tcp6       0      0 :::22      

| Option | Meaning                                 |
| ------ | --------------------------------------- |
| `-t`   | Show TCP connections                    |
| `-u`   | Show UDP connections                    |
| `-l`   | Show only listening ports               |
| `-p`   | Show process using the port             |
| `-n`   | Show numeric IPs/ports instead of names |

## curl -I <service-endpoint>
root@ubuntu-host ~ ➜  curl -I https://google.com
HTTP/2 301 
location: https://www.google.com/
content-type: text/html; charset=UTF-8
content-security-policy-report-only: object-src 'none';base-uri 'self';script-src 'nonce-5vDdb0ZekwQON0UQmj2vwg' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
date: Fri, 22 May 2026 12:28:07 GMT
expires: Sun, 21 Jun 2026 12:28:07 GMT
cache-control: public, max-age=2592000
server: gws
content-length: 220
x-xss-protection: 0
x-frame-options: SAMEORIGIN

## ping   
root@ubuntu-host ~ ➜  ping google.com
PING google.com (192.178.210.102) 56(84) bytes of data.
64 bytes from yucbfiv-in-f102.1e100.net (192.178.210.102): icmp_seq=1 ttl=114 time=1.50 ms
64 bytes from yucbfiv-in-f102.1e100.net (192.178.210.102): icmp_seq=2 ttl=114 time=1.22 ms
64 bytes from yucbfiv-in-f102.1e100.net (192.178.210.102): icmp_seq=3 ttl=114 time=1.18 ms
64 bytes from yucbfiv-in-f102.1e100.net (192.178.210.102): icmp_seq=4 ttl=114 time=1.25 ms
64 bytes from yucbfiv-in-f102.1e100.net (192.178.210.102): icmp_seq=5 ttl=114 time=1.22 ms

| Command   | Use it when you want to check...                          |
| --------- | --------------------------------------------------------- |
| `ping`    | if another machine/server is reachable                    |
| `curl`    | if an application/API/website is responding               |
| `netstat` | older tool to inspect ports and network connections       |
| `ss`      | modern replacement for `netstat` to inspect sockets/ports |

-------------------------------------------------------------------------------------------------------------------------------
5. Log commands
### checking logs via journalctl
root@ubuntu-host ~ ➜  journalctl -u ssh -f
May 21 00:31:30 ubuntu-host sshd[1729]: Accepted publickey for root from 192.168.80.153 port 57436 ssh2: RSA SHA256:6RjdLRihoxjjxsPr9CJN0pfWDZgIERsdgpOmSf91hOM
May 21 00:31:30 ubuntu-host sshd[1729]: pam_unix(sshd:session): session opened for user root(uid=0) by root(uid=0)
May 21 00:31:30 ubuntu-host sshd[1729]: Received disconnect from 192.168.80.153 port 57436:11: disconnected by user
May 21 00:31:30 ubuntu-host sshd[1729]: Disconnected from user root 192.168.80.153 port 57436

### checking logs via tail
root@ubuntu-host log/journal/05ee70c460a9dd015c31410c674044a5 ➜  tail -50f system.journal 
-------------------------------------------------------------------------------------------------------------------------------

