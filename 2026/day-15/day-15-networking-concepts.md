# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

You will:
- Understand how **DNS** resolves names to IPs
- Learn **IP addressing** (IPv4, public vs private)
- Break down **CIDR notation** and **subnetting** basics
- Know common **ports** and why they matter

## Challenge Tasks

### Task 1: DNS – How Names Become IPs
1. Explain in 3–4 lines: what happens when you type `google.com` in a browser?

1️⃣ DNS Resolution (Application Layer – L7) = google.com → IP address
     if DNS Failed = Server Not Found.

2️⃣ TCP Connection (Transport Layer – L4) = Browser opens a TCP connection to: Port: 443 (HTTPS default)
3️⃣ TLS Handshake (Because It’s HTTPS)
4️⃣ HTTP Request Sent
5️⃣ Load Balancing & Global Routing
6️⃣ Server Processing
7️⃣ Response Sent Back

`so its => DNS → TCP → TLS → HTTP → Rendering`

2. What are these record types? Write one line each:
   - `A`, `AAAA`, `CNAME`, `MX`, `NS`

They are core DNS Record type.

1️⃣ A Record
Maps a domain name to an IPv4 address.
👉 example.com → 192.168.HAM.1

2️⃣ AAAA Record
Maps a domain name to an IPv6 address.
👉 example.com → 2404:6800:4009:80b::200e

3️⃣ CNAME (Canonical Name)
Points one domain to another domain name (alias).
👉 www.example.com → example.com

4️⃣ MX (Mail Exchange)
Specifies the mail server responsible for receiving emails for a domain (with priority).
👉 example.com → mail.example.com (priority 10)

5️⃣ NS (Name Server)
Defines the authoritative DNS servers for a domain.
👉 example.com → ns1.provider.com

💡 Quick Memory Trick:
A → Address (IPv4)
AAAA → Address (IPv6)
CNAME → Copy Name
MX → Mail eXchange
NS → Name Server

3. Run: `dig google.com` — identify the A record and TTL from the output

`dig google.com`

;; ANSWER SECTION:
google.com.     142     IN      A       142.250.183.14

A → Record type (IPv4 address)
142.250.183.14 → The IPv4 address for google.com

TTL(Time To Live) = 142 seconds

That means the DNS response can be cached for 142 seconds before it must be refreshed.

### Task 2: IP Addressing
1. What is an IPv4 address? How is it structured? (e.g., `192.168.1.10`)

IPV4 = Internet Protocol Version 4
A 32 bit logical address which is used to uniquely identify a device on a network.
Allows the device to send and receive data over IP Network.

An IPV4 adddress is :
-32 bit address
-divided into 4 octets
-each octate = 8 bits
-Each octet ranges from 0-255

An IPv4 address has:
1.Network Portion
2.Host Portion

Example : `192.168.1.10/24`

/24 → first 24 bits = network
last 8 bits = host

So:
Network → 192.168.1.0
Host → .10

`When debugging networking :`
IP wrong? → No connectivity
Subnet mismatch? → Can't reach gateway
Private IP without NAT? → No internet
Wrong CIDR? → Routing failure

`Understanding IPv4 structure is foundational for :`
VPC design
Subnetting
Security groups
Kubernetes networking
Load balancers

2. Difference between **public** and **private** IPs — give one example of each

🔹 `Public IP`
Globally unique and routable on the internet
Assigned by an ISP
Used to communicate across the internet
example = 8.8.8.8 = goggle's public DNS Server

🔹 `Private IP`
Used inside local networks (LAN / VPC)
Not directly accessible from the internet
Requires NAT to access the internet
Assigned by Router / DHCP
example = 192.168.1.10

3. What are the private IP ranges?
   - `10.x.x.x`, `172.16.x.x – 172.31.x.x`, `192.168.x.x`

`three official ranges :`
A. 10.0.0.0 – 10.255.255.255
b. 172.16.0.0 – 172.31.255.255
C. 192.168.0.0 – 192.168.255.255


4. Run: `ip addr show` — identify which of your IPs are private

### Task 3: CIDR & Subnetting
1. What does `/24` mean in `192.168.1.0/24`?

The /24 is called CIDR notation.
Means The first 24 bits are the network portion of the IP address.


2. How many usable hosts in a `/24`? A `/16`? A `/28`?

`Usable Hosts = 2^(host bits) - 2`

👉 We subtract 2 for:

1 Network address
1 Broadcast address

/24 = Total bits = 32
Network bits = 24 = 32-24 = Host bits = 8
2^8 - 2 = 256 - 2 = 254 ✅ Usable Hosts = 254

/16 = Host bits = 16 SO   2^16 - 2 = 65,536 - 2 = 65,534 ✅ Usable Hosts = 65,534

/28 = Host bits = 4 Therefore    2^4 - 2 = 16 - 2 = 14 ✅ Usable Hosts = 14

3. Explain in your own words: why do we need subnet?

`Subnetting = dividing a large network into smaller logical networks`

for control, performance, and security.


4. Quick exercise — fill in:

| CIDR | Subnet Mask | Total IPs | Usable Hosts |
|------|-------------|-----------|--------------|
| /24  |255.255.255.0| 256       | 254          |
| /16  |255.255.0.0  | 65536     | 65,534       |
| /28  |255.255.255.240| 16      | 14           |

--------------------------------------------------------------------------------

### Task 4: Ports – The Doors to Services
1. What is a port? Why do we need them?

A port is a logical communication endpoint on a device.
If an IP address identifies a machine,
a port identifies the specific application/service running on that machine.

192.168.1.10:80

192.168.1.10 → Device
80 → Web server running on that device

Ports are critical for:
Firewall rules
Security groups
Kubernetes services
Docker port mapping (-p 8080:80)
Load balancers
Network troubleshooting

If a port is:
Closed → Connection refused
Blocked → Timeout
Not listening → Service down


2. Document these common ports:

| Port | Service |
|------|---------|
| 22   | SSH     |
| 80   | HTTP    |
| 443  | HTTP S  |
| 53   | DNS     |
| 3306 | MYSQL   |
| 6379 | Redis   |
| 27017| MongoDB |

3. Run `ss -tulpn` — match at least 2 listening ports to their services

---------------------------------------------------------------------------------

### Task 5: Putting It Together
Answer in 2–3 lines each:
- You run `curl http://myapp.com:8080` — what networking concepts from today are involved?  =>  

DNS → TCP (8080) → IP Routing → HTTP Request/Response

- Your app can't reach a database at `10.0.1.50:3306` — what would you check first? =>   

ping or traceroute the app server
Check firewall / security groups / NACLs
Check Database service status by : netstat -tulnp | grep 3306
Confirm it’s bound to the correct IP and not just 127.0.0.1

`Network → Port → Firewall → Service`
----------------------------------------------------------------------