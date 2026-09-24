IPv6 provides a much larger address space than IPv4.

---

## 10. Private IPv4 Addresses

Common private IPv4 ranges include:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

These are commonly used inside private networks.

---

## 11. Public IP Address

A public IP address can be used for communication across the public Internet.

Your device may not directly receive a public IP.

For example:

```text
Device
  ↓
Private IP
  ↓
Router/NAT
  ↓
Public IP
  ↓
Internet
```

---

## 12. MAC Address

A network interface commonly has a MAC address.

Example:

```text
00:11:22:33:44:55
```

MAC addresses operate at the data-link layer.

View interface information with:

```bash
ip link
```

---

## 13. Checking Your IP Address

Run:

```bash
ip addr
```

Look for:

```text
inet
```

Example:

```text
inet 192.168.1.20/24
```

Here:

```text
192.168.1.20
```

is the IPv4 address.

---

## 14. CIDR Notation

You may see:

```text
192.168.1.20/24
```

The:

```text
/24
```

represents the network prefix length.

A `/24` IPv4 network commonly corresponds to:

```text
255.255.255.0
```

---

## 15. Subnet Mask

A subnet mask determines which portion of an IPv4 address represents the network and which portion represents hosts.

Example:

```text
IP:
192.168.1.20

Mask:
255.255.255.0
```

CIDR representation:

```text
192.168.1.20/24
```

---

## 16. Default Gateway

A default gateway is the device used to reach destinations outside the local network.

Usually this is a router.

Conceptually:

```text
Computer
   ↓
Default Gateway
   ↓
Internet
```

---

## 17. Viewing the Routing Table

Run:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0
```

The line beginning with:

```text
default
```

shows the default route.

---

## 18. Understanding `ip route`

Example:

```text
default via 192.168.1.1 dev eth0
```

Means approximately:

```text
default
→ destination not otherwise matched

via 192.168.1.1
→ next-hop router

dev eth0
→ use interface eth0
```

---

## 19. Testing Connectivity With `ping`

Use:

```bash
ping example.com
```

or:

```bash
ping 8.8.8.8
```

On many Linux systems, press:

```text
Ctrl + C
```

to stop continuous pinging.

---

## 20. What Does `ping` Test?

`ping` commonly uses ICMP Echo Request and Echo Reply messages.

It can help determine whether a destination is reachable and how long responses take.

A successful response may look like:

```text
64 bytes from ...
time=20 ms
```

---

## 21. Ping Localhost

Test the local networking stack:

```bash
ping 127.0.0.1
```

If this works, the local TCP/IP stack is responding.

It does not prove that your Internet connection works.

---

## 22. DNS

DNS means:

```text
Domain Name System
```

DNS translates domain names into IP addresses.

For example:

```text
example.com
     ↓
IP address
```

Without DNS, users would often need to remember IP addresses instead of domain names.

---

## 23. DNS Lookup With `getent`

Use:

```bash
getent hosts example.com
```

This can resolve a hostname using the system's configured name-service mechanisms.

---

## 24. `nslookup`

Another DNS tool is:

```bash
nslookup example.com
```

It can display DNS resolution information.

It may not be installed on every Linux system.

---

## 25. `dig`

`dig` is a powerful DNS troubleshooting tool.

Example:

```bash
dig example.com
```

You can query specific record types:

```bash
dig example.com A
```

For IPv6:

```bash
dig example.com AAAA
```

---

## 26. DNS Configuration

Depending on the Linux distribution and network configuration, DNS settings may be managed by components such as:

```text
systemd-resolved
NetworkManager
resolv.conf
```

You may encounter:

```text
/etc/resolv.conf
```

Do not manually modify DNS configuration without understanding how your system manages it.

---

## 27. Hostname

A hostname identifies the computer on a network.

Check it:

```bash
hostname
```

Another useful command:

```bash
hostnamectl
```

---

## 28. Changing Hostname

On systems using `systemd`, you can use:

```bash
sudo hostnamectl set-hostname new-name
```

Example:

```bash
sudo hostnamectl set-hostname linux-pc
```

Verify:

```bash
hostname
```

---

## 29. `/etc/hosts`

Linux can use:

```text
/etc/hosts
```

for local hostname mappings.

Example:

```text
127.0.0.1 localhost
```

A custom mapping can look like:

```text
192.168.1.20 myserver
```

This allows the name:

```text
myserver
```

to resolve locally to that address.

---

## 30. Network Ports

A port identifies a service endpoint on a host.

Examples:

```text
22  → SSH
80  → HTTP
443 → HTTPS
53  → DNS
```

Ports range from:

```text
0 to 65535
```

---

## 31. TCP

TCP means:

```text
Transmission Control Protocol
```

TCP provides:

```text
Reliable delivery
Ordered data
Connection-oriented communication
Retransmission when needed
```

It is commonly used by:

```text
HTTP/HTTPS
SSH
Many application protocols
```

---

## 32. UDP

UDP means:

```text
User Datagram Protocol
```

UDP is connectionless and provides fewer delivery guarantees than TCP.

It is useful where:

```text
Low overhead
Low latency
Application-controlled reliability
```

are important.

Examples include many:

```text
DNS queries
Streaming applications
Real-time applications
```

---

## 33. TCP vs UDP

```text
TCP
→ Connection-oriented
→ Reliable
→ Ordered
→ More overhead

UDP
→ Connectionless
→ No built-in delivery guarantee
→ Lower overhead
```

---

## 34. Checking Listening Ports

A useful command is:

```bash
ss -tuln
```

This can show listening TCP and UDP sockets.

Options:

```text
-t → TCP
-u → UDP
-l → listening
-n → numeric output
```

---

## 35. `ss`

`ss` is a modern tool for inspecting sockets.

Example:

```bash
ss -tuln
```

You may see:

```text
Local Address:Port
0.0.0.0:22
0.0.0.0:80
```

This indicates services listening on those ports.

---

## 36. Checking Active Connections

Use:

```bash
ss -tun
```

This can show active TCP and UDP sockets.

For more process information:

```bash
sudo ss -tulpn
```

Use elevated privileges only when required.

---

## 37. `curl`

`curl` can communicate with URLs and network services.

Example:

```bash
curl https://example.com
```

It can be useful for:

```text
Testing HTTP
Downloading data
Testing APIs
Debugging web services
```

---

## 38. Checking HTTP Headers

Use:

```bash
curl -I https://example.com
```

This requests headers instead of downloading the full page content.

You may see:

```text
HTTP status
Content-Type
Server
Date
```

---

## 39. `wget`

`wget` is commonly used for downloading files.

Example:

```bash
wget https://example.com/file.zip
```

It can download resources from supported protocols.

---

## 40. `traceroute`

`traceroute` can help show the network path toward a destination.

Example:

```bash
traceroute example.com
```

Depending on the distribution, you may need to install it first.

---

## 41. `tracepath`

Another option is:

```bash
tracepath example.com
```

It can show network path information and may be available without installing `traceroute`.

---

## 42. Network Troubleshooting Workflow

When Internet access is not working:

```text
Check interface
      ↓
Check IP address
      ↓
Check default route
      ↓
Ping gateway
      ↓
Test public IP
      ↓
Test DNS
      ↓
Test application/service
```

Useful commands:

```bash
ip addr
ip route
ping
getent hosts
ss
curl
```

---

## 43. Troubleshooting Example

Check interfaces:

```bash
ip link
```

Check addresses:

```bash
ip addr
```

Check routes:

```bash
ip route
```

Test localhost:

```bash
ping 127.0.0.1
```

Test a known public IP:

```bash
ping 8.8.8.8
```

Test DNS:

```bash
getent hosts example.com
```

Test HTTPS:

```bash
curl -I https://example.com
```

---

## 44. If IP Works but Domain Does Not

Suppose:

```bash
ping 8.8.8.8
```

works but:

```bash
ping example.com
```

fails.

A possible problem is:

```text
DNS resolution
```

Check:

```bash
getent hosts example.com
```

and:

```bash
resolvectl status
```

if `resolvectl` is available.

---

## 45. If DNS Works but Website Does Not

If:

```bash
getent hosts example.com
```

works but:

```bash
curl -I https://example.com
```

fails, investigate:

```text
Network routing
Firewall
Proxy
TLS
Remote server
Application configuration
```

---

## 46. NetworkManager

Many desktop Linux distributions use:

```text
NetworkManager
```

You can inspect connections with:

```bash
nmcli connection show
```

Check device status:

```bash
nmcli device status
```

---

## 47. `nmcli`

`nmcli` is a command-line interface for NetworkManager.

Example:

```bash
nmcli device status
```

It can show:

```text
Device
Type
State
Connection
```

---

## 48. Wi-Fi Information

On systems using NetworkManager:

```bash
nmcli device wifi list
```

This can list nearby Wi-Fi networks.

Be careful when connecting to unknown networks.

---

## 49. Network Interface Up and Down

You can administratively bring an interface up:

```bash
sudo ip link set eth0 up
```

Bring it down:

```bash
sudo ip link set eth0 down
```

Replace `eth0` with the actual interface name.

Do not disable the interface you are using for remote access unless you understand the consequences.

---

## 50. Practice

Run:

```bash
ip addr
```

Identify:

```text
Loopback interface
Active network interface
IPv4 address
IPv6 address
```

Then run:

```bash
ip route
```

Identify:

```text
Default gateway
Active interface
```

---

## 51. Practice: Connectivity

Test:

```bash
ping 127.0.0.1
```

Then:

```bash
ping 8.8.8.8
```

Then:

```bash
ping example.com
```

Stop each test with:

```text
Ctrl + C
```

Compare the results.

---

## 52. Practice: DNS

Run:

```bash
getent hosts example.com
```

Then:

```bash
nslookup example.com
```

if available.

Then:

```bash
dig example.com
```

if available.

Compare the information returned by each command.

---

## 53. Practice: Ports

Run:

```bash
ss -tuln
```

Identify:

```text
TCP listening ports
UDP listening ports
Local addresses
Port numbers
```

If necessary:

```bash
sudo ss -tulpn
```

to see associated process information.

---

## 54. Practice: HTTP

Run:

```bash
curl -I https://example.com
```

Look for:

```text
HTTP status
Content-Type
Server
Date
```

Then try:

```bash
curl https://example.com
```

Observe the returned response.

---

## 55. Challenge

Complete this network inspection checklist:

```text
1. Find your hostname
2. Find your IP address
3. Find your network interface
4. Find your default gateway
5. Check localhost
6. Test Internet connectivity
7. Test DNS
8. List listening ports
9. Test HTTPS
```

Commands:

```bash
hostname
ip addr
ip link
ip route
ping 127.0.0.1
ping 8.8.8.8
getent hosts example.com
ss -tuln
curl -I https://example.com
```

---

## 56. Interview Questions

### Q1. What is an IP address?

An address used to identify a network interface on an IP network.

### Q2. What is the difference between IPv4 and IPv6?

```text
IPv4 → 32-bit addresses
IPv6 → 128-bit addresses
```

### Q3. What is `127.0.0.1`?

The standard IPv4 loopback address.

### Q4. What is a default gateway?

A router or next-hop device used to reach destinations outside the local network.

### Q5. What does `ip addr` do?

It displays network interfaces and their IP addresses.

### Q6. What does `ip route` do?

It displays the system's routing table.

### Q7. What does `ping` use?

Typically ICMP Echo Request and Echo Reply messages.

### Q8. What is DNS?

The Domain Name System translates domain names into network addresses and provides other DNS information.

### Q9. What is a port?

A logical endpoint used to identify network services on a host.

### Q10. What is the difference between TCP and UDP?

```text
TCP → connection-oriented and reliable
UDP → connectionless with lower protocol overhead
```

### Q11. What does `ss -tuln` do?

It displays listening TCP and UDP sockets using numeric addresses and ports.

### Q12. What does `curl` do?

It transfers data to or from a URL and is commonly used for testing HTTP services and APIs.

### Q13. What is a MAC address?

A link-layer address associated with a network interface.

### Q14. What is CIDR?

CIDR is a notation for specifying an IP address together with its network prefix length, such as `/24`.

### Q15. What is NetworkManager?

A Linux service and toolset used by many distributions to manage network connections and interfaces.

---

# Summary

Important Linux networking concepts:

```text
Network Interface
IP Address
IPv4
IPv6
MAC Address
Subnet
Gateway
Routing
DNS
Ports
TCP
UDP
Sockets
```

Important commands:

```bash
ip addr
ip link
ip route
ping
hostname
hostnamectl
getent hosts
nslookup
dig
ss
curl
wget
traceroute
tracepath
nmcli
```

Basic troubleshooting order:

```text
Interface
   ↓
IP address
   ↓
Routing
   ↓
Gateway
   ↓
Internet
   ↓
DNS
   ↓
Application
```

The key rule:

```text
Check each layer systematically instead of randomly changing network settings.
```

Next topic: Linux SSH and Remote Access

Commit message:

```text
Add Linux networking basics guide
```
```
