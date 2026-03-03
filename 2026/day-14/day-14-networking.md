# Day 14 – Networking Fundamentals & Hands-on Checks

## Quick Concepts (write 1–2 bullets each)
- OSI layers (L1–L7) vs TCP/IP stack (Link, Internet, Transport, Application)
- Where **IP**, **TCP/UDP**, **HTTP/HTTPS**, **DNS** sit in the stack
- One real example: “`curl https://example.com` = App layer over TCP over IP”

 **OSI vs TCP/IP models** : 

**OSI : Open System Interconnection Model**
 `7 Layer Framework`
 `explains how data trvels from one device to another device over network.`

 7.Application Layer => HTTP,DNS
 6.Presentation Layer =>  Data Formatting : Encryption/decryption
 5.Session Layer =>  (Login Session)
 4.Transport Layer => TCP/UDP
 3.Network Layer => Logical Routing and addressing
 2.Data Link Layer => Physical Addressing (MAC Address)
 1.Physical Layer => Ethernet cable, Fiber optics


 **TCP/IP Model : Transmission Control Protocol/Internet Protocol**
 `Communication Model for internet and modern Network`

 4.Application Layer => HTTP/HTTPS
 3.Transport Layer => TCP,UDP
 2.Internet Layer => IP Addresses/Route (IP/ICMP)
 1.Network Access Layer => Cables & MAC Address


 `curl https://example.com` => 

 curl = to find your own IP address
 https = Protocol
 www = Web pages
 example = Domain Name
 .com = ROOT DNS


----------------------------------------------------------------------------------------------------------------

 ## Hands-on Checklist (run these; add 1–2 line observations)
- **Identity:** `hostname -I` (or `ip addr show`) — note your IP.
=> 172.31.2.44    172.17.0.1
hostname : Shows system hostname
hostname -I : all IP addresses assigned to your machine.

- **Reachability:** `ping <target>` — mention latency and packet loss.
=> ping 172.31.2.44

--- 172.31.2.44 ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8210ms
rtt min/avg/max/mdev = 0.025/0.032/0.035/0.002 ms

- **Path:** `traceroute <target>` (or `tracepath`) — note any long hops/timeouts.
=> traceroute 172.31.2.44

traceroute to 172.31.2.44 (172.31.2.44), 30 hops max, 60 byte packets
 1  ip-172-31-2-44.ap-south-1.compute.internal (172.31.2.44)  0.031 ms  0.005 ms  0.005 ms

- **Ports:** `ss -tulpn` (or `netstat -tulpn`) — list one listening service and its port.
=> `shows all listening TCP & UDP ports along with the process using them`


- **Name resolution:** `dig <domain>` or `nslookup <domain>` — record the resolved IP.
=>(DNS Debugging Commands)

Dig : helps to check how a domain name is resolved to an IP address.
lookup : helps to check if DNS is correctly resolving a domain

- **HTTP check:** `curl -I <http/https-url>` — note the HTTP status code.
=> curl -I  http://google.com

curl → Sends HTTP request
-I → Fetch headers only (no body/content)
http://google.com → Target URL

👉 It sends a HEAD request and shows only the HTTP response headers.

- **Connections snapshot:** `netstat -an | head` — count ESTABLISHED vs LISTEN (rough).

netstat → Shows network connections
-a → Show all connections (listening + established)
-n → Show numeric addresses (no DNS resolution)
| head → Show only the first 10 lines

tcp        0    192 172.31.2.44:22          157.49.12.103:55126     ESTABLISHED
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:45381         0.0.0.0:*               LISTEN
tcp6       0      0 :::80                   :::*                    LISTEN
tcp6       0      0 :::22                   :::*                    LISTEN
----------------------------------------------------------------------------------------------------------------

## Mini Task: Port Probe & Interpret
1) `Identify one listening port from `ss -tulpn` (e.g., SSH on 22 or a local web app).` 

=>  53 = DNS (Domain Name System)
    22 = SSH
    80 = HTTP (Web Traffic)
    45381 = local ephemeral (temporary) port

2) From the same machine, test it: `nc -zv localhost <port>` (or `curl -I http://localhost:<port>`).
=>  
nc -zv localhost 45381
Connection to localhost (127.0.0.1) 45381 port [tcp/*] succeeded!
 nc -zv localhost 22
Connection to localhost (127.0.0.1) 22 port [tcp/ssh] succeeded!
nc -zv localhost 80
Connection to localhost (127.0.0.1) 80 port [tcp/http] succeeded!
nc -zv localhost 53
nc: connect to localhost (127.0.0.1) port 53 (tcp) failed: Connection refused


3) `Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).`
## Here 53 is not reachable to TCP so i will : ##

-Verify if DNS service is running
-whether dns service listen on TCP or UDP only
-Run: ss -tulpn | grep :53
-dig localhost

-------------------------------------------------------------------------------------------------------------

## Reflection (add to your markdown)
- Which command gives you the fastest signal when something is broken?

It depends on what is broken:

`Problem Type`
Website down/application-level issue : curl
Service not running : ss -tulpn
Port issue : nc -zv
DNS issue : dig
Network unreachable : ping


- What layer (OSI/TCP-IP) would you inspect next if DNS fails? If HTTP 500 shows up?
=> Transport Layer

- Two follow-up checks you’d run in a real incident.
=> curl
=> ping