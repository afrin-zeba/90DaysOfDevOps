### Task 1: DNS – How Names Become IPs
1. Explain in 3–4 lines: what happens when you type `google.com` in a browser?
DNS translates google.com into Google's IP address --> application layer 
Your computer uses IP to route packets to Google's servers over the internet --> network layer 
TCP establishes a reliable connection --> transport layer 
HTTPS securely sends a request for the webpage --> application layer + encryption via presentation layer 
Google's server responds with the webpage data, and your browser renders it on the screen --> application layer

2. What are these record types? Write one line each:
   - `A`, `AAAA`, `CNAME`, `MX`, `NS`
A — Maps a domain to an IPv4 address (e.g. example.com → 93.184.216.34)
AAAA — Maps a domain to an IPv6 address (e.g. example.com → 2606:2800::1)
CNAME — Aliases one domain to another (e.g. www.example.com → example.com)
MX — Specifies mail servers responsible for receiving email for a domain (e.g. example.com → mail.google.com)
NS — Delegates a domain to authoritative name servers that hold its DNS records (e.g. example.com → ns1.cloudflare.com)

3. Run: `dig google.com` — identify the A record and TTL from the output
ubuntu@ip-172-31-46-159:~$ dig google.com
; <<>> DiG 9.20.18-1ubuntu2.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8299
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		241	IN	A	142.250.134.100
google.com.		241	IN	A	142.250.134.101
google.com.		241	IN	A	142.250.134.113
google.com.		241	IN	A	142.250.134.102
google.com.		241	IN	A	142.250.134.138
google.com.		241	IN	A	142.250.134.139

;; Query time: 2 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Mon Jun 01 18:10:31 UTC 2026
;; MSG SIZE  rcvd: 135

---

### Task 2: IP Addressing
1. What is an IPv4 address? How is it structured? (e.g., `192.168.1.10`)
 An IPv4 address is a unique address used to identify a device on a network. It consists of 4 numbers (octets) separated by dots, with each number ranging from 0 to 255.

2. Difference between **public** and **private** IPs — give one example of each
public - Visible on the internet and globally unique.
private - Used inside private networks (home, office, VPC) and not directly reachable from the internet.

3. What are the private IP ranges?
   - `10.x.x.x`, `172.16.x.x – 172.31.x.x`, `192.168.x.x`

4. Run: `ip addr show` — identify which of your IPs are private
ubuntu@ip-172-31-46-159:~$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: ens5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 02:da:73:d8:7c:7f brd ff:ff:ff:ff:ff:ff
    altname enp0s5
    altname enx02da73d87c7f
    inet 172.31.46.159/20 metric 100 brd 172.31.47.255 scope global dynamic ens5
       valid_lft 2964sec preferred_lft 2964sec
    inet6 fe80::da:73ff:fed8:7c7f/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever

172.31.46.159/20 --> since it starts with 172 its private 
---

### Task 3: CIDR & Subnetting
1. What does `/24` mean in `192.168.1.0/24`? 2power8 possible ip addresses so 192.168.1.1 - 192.168.1.254
2. How many usable hosts in a `/24`? A `/16`? A `/28`?
/24 -> 2power8 but remove 2 for broadcast and network address 
/16 -> 2power16 but remove 2 for broadcast and network address 
/28 -> 2power4 but remove 2 for broadcast and network address 

3. Explain in your own words: why do we subnet?
We subnet to divide a large network into smaller, organized networks.

Benefits:

Better organization - Separate web servers, databases, and applications.
Improved security - Control which subnets can talk to each other.
Efficient IP usage - Don't waste thousands of IP addresses.
Reduced network traffic - Devices only communicate within smaller groups when possible.

4. Quick exercise — fill in:

| CIDR | Subnet Mask | Total IPs | Usable Hosts |
|------|-------------|-----------|--------------|
| /24  | ?           | 256       | 254          | x.x.x.1 - x.x.x.254
| /16  | ?           | 65,536    | 65,534       | #not sure 
| /28  | ?           | 16        | 14           | x.x.x.1 - x.x.x.14

---

### Task 4: Ports – The Doors to Services
1. What is a port? Why do we need them?
A port is a numbered doorway on a computer that tells incoming network traffic which application should receive it. A single computer can run many services at the same time:

Laptop
├─ SSH Server
├─ Web Server
├─ MySQL
└─ Redis
If network traffic arrives, how does the computer know whether it is for SSH, MySQL, or the web server?That's what ports are for.

Example
192.168.1.10:22    → SSH
192.168.1.10:80    → HTTP
192.168.1.10:3306  → MySQL

Same IP address, different ports, different applications.

2. Document these common ports:

| Port | Service |
|------|---------|
| 22   | SSH     |
| 80   | HTTP    |
| 443  | HTTPS   |
| 53   | DNS     |
| 3306 | MySQL   |
| 6379 | Redis   |
| 27017| MongoDB |

3. Run `ss -tulpn` — match at least 2 listening ports to their services
tcp                 LISTEN               0                     4096        0.0.0.0:22 --> SSH
tcp                 LISTEN               0                     4096        0.0.0.0:80 --> HTTP

---

### Task 5: Putting It Together
Answer in 2–3 lines each:
- You run `curl http://myapp.com:8080` — what networking concepts from today are involved?
CURL means:
"Connect to the web server at myapp.com on port 8080 and send an HTTP request."
What happens:

1. DNS finds the IP address of myapp.com.
2. Your computer connects to that IP on port 8080.
3. It sends an HTTP request like:

GET / HTTP/1.1
Host: myapp.com

The application listening on port 8080 responds and curl prints the response to your terminal -
<html>
  <body>Hello World</body>
</html>

- Your app can't reach a database at `10.0.1.50:3306` — what would you check first?
ill telnet to check if the app is available - telnet 10.0.1.50 3306
then Security Groups - Is port 3306 allowed between the app and DB?
---
