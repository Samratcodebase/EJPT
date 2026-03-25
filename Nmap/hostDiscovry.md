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
[no firewall on layer 2]


## Core Host Discovry Types
Types           Commands   Default Ports
ICMP (echo req)-> -PE         no port  
ICMP TimeStamp -> -PP         no port
TCP SYN PING      -PS         80,443
TCP ACK PING      -PA         80
UDP               -PU         40125
SCTP INIT PING    -PY         132
IP PROTOCOL PING  -PO         prot 1 ,2,4

## TCP SYN PING [-PS]
send a TCP SYN Ping to the Target !! if the Target is Alive it  response with SYN-ACK or RST both Conclude the host is Up.


## TCP ACK PING  [-PA]
sends a TCP ACK PING to the Target !!   IF the target is Alive it Responsed With RST , if not  Alive No response is there !!
Some OS or FireWall Maybe Configired To Drop Random TCP ACK PING Cause it  Dont Fall into The HandShake Cycle !! 
no TCP SYN PING

## SCTP INIT PING [-PY] (Mostly Used in Telecom/Signaling)
sends an SCTP INIT chunk , if the host is alive SENDS INI-ACK 

##