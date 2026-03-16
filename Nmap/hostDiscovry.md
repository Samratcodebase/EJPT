# 🚀 Host Discovery with Nmap

`nmap -sn` known as ping scan  
[no port scan, no service scan — just host discovery]

---

## What `-sn` actually does depends on context

### As root user
[uses all these together]

- ICMP PING REQUEST  
- TCP SYN to port 443  
- TCP ACK to port 80  
- ICMP timestamp request  

### why all these together?

-> All 4 probes, simultaneously, when run as root. cause all 4 is redundancy firewalls may block ICMP, Somwe Block SYN to 443 some Block ACK to 80 !!

By throwing all 4 probes at once, if any one gets a response, the host is marked alive.

---

### As unprivileged user

- Only TCP SYN (via connect()) to ports 80 and 443  
- No ICMP at all (requires raw sockets → needs root)

---

## Note

If the target is on the same local subnet  
[on same network], nmap skips all 4 of these entirely and just uses ARP.  
[no firewall]