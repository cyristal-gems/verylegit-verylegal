# Lab Setup & Configuration

For this cybersecurity lab, I created a small virtual network to simulate a realistic environment for learning both offensive and defensive security techniques. The goal was to build a system where I could act as an attacker, defend a network, and explore how different tools like firewalls and intrusion detection systems (IDS) work. I used VirtualBox as the virtualization platform to run all the machines in the lab. VirtualBox allowed me to create and manage multiple virtual machines (VMs) on my personal computer. It’s a free and beginner-friendly platform that works on different operating systems. Each VM runs inside VirtualBox like its own separate computer, which made it possible for me to safely practice hacking without affecting my real machine or network.

## Virtual Machines and Roles
• Kali Linux is the attacker because it's a common penetration testing tool used in ethical hacking.  
• Metasploitable 2 is a purposely vulnerable machine to simulate real-world servers that could be exploited.  
• pfSense acts as the firewall to control and monitor traffic, like a real-world network's defense mechanism.

```
+-------------------------------+------------------+------------------------------+----------------------------------------------+
| Device                        | IP Address       | Network                      | Role                                         |
+-------------------------------+------------------+------------------------------+----------------------------------------------+
| Kali Linux (Attacker Machine) | 192.168.12.13    | AttackerNet                  | Attacker; scanning, exploitation, Metasploit |
| Metasploitable 2 (Target)     | 192.168.12.14    | ServerNet                    | Vulnerable target for exploitation           |
| pfSense Firewall              | WAN: 192.168.1.1 | WAN Network                  | Firewall / IDS/IPS / segmentation            |
|                               | LAN: 192.168.12.1| LAN: 192.168.12.0/24         |                                              |
+-------------------------------+------------------+------------------------------+----------------------------------------------+
```

## Networks
• WAN (Adapter 1): NAT (external connection for updates)  
• LAN (Adapter 2): Internal Network: attacker_net (192.168.11.x)  
• LAN (Adapter 3): Internal Network: server_net (192.168.12.x)  
• LAN (Adapter 4): Internal Network: management_net (192.168.13.x)

```
+--------------+--------------------+-----------------------------------------------+
| Network      | Subnet / Type      | Purpose                                       |
+--------------+--------------------+-----------------------------------------------+
| NAT Network  | — (pfSense WAN)    | External/Internet connectivity                |
| ServerNet    | 192.168.12.0/24    | Internal link: attacker ↔ target              |
| AttackerNet  | 192.168.11.x       | Dedicated attacker testing segment            |
+--------------+--------------------+-----------------------------------------------+
```

## pfSense Interfaces & IP Addresses
pfSense (Firewall and IDS/IPS)  
• Purpose: Acts as the firewall to control traffic between networks and runs Snort as an IDS/IPS to detect potential attacks.  
• Network Adapters:  
  WAN (Adapter 1): NAT (external connection for updates)  
  LAN (Adapter 2): Internal Network: attacker_net (192.168.11.x)  
  LAN (Adapter 3): Internal Network: server_net (192.168.12.x)  
  LAN (Adapter 4): Internal Network: management_net (192.168.13.x)  
• IP Addresses:  
  WAN (em0): DHCP (assigned by VirtualBox NAT)  
  LAN (em1): 192.168.11.1  
  LAN (em2): 192.168.12.1  
  LAN (em3): 192.168.13.1

```
+-----------+-----------------+-----------------+--------------------------+
| Interface | Adapter         | Network         | IP / Assignment          |
+-----------+-----------------+-----------------+--------------------------+
| WAN       | Adapter 1 / em0 | NAT (Internet)  | DHCP (VirtualBox NAT)    |
| LAN 1     | Adapter 2 / em1 | attacker_net    | 192.168.11.1             |
| LAN 2     | Adapter 3 / em2 | server_net      | 192.168.12.1             |
| LAN 3     | Adapter 4 / em3 | management_net  | 192.168.13.1             |
+-----------+-----------------+-----------------+--------------------------+
```

## Kali Linux – Network Adapters
Kali Linux (Attacker Machine)  
• Purpose: My main offensive machine used for penetration testing. Kali is a well-known Linux distribution that comes with tools like Metasploit, Nmap, and Netcat.  
• Network Adapters:  
  Adapter 1: Internal Network: attacker_net (192.168.11.x)  
  o Adapter 2: Internal Network: server_net (192.168.12.x)  
•  Address on server_net: 192.168.12.13  

Kali was used for running scans, exploits, and connecting to the target. I set it up to have access to both the attacker network and the server network so I could simulate an external attacker trying to reach an internal system.

```
+-----------+-----------------------------+-----------------+
| Adapter   | Network                     | IP Address      |
+-----------+-----------------------------+-----------------+
| Adapter 1 | attacker_net (192.168.11.x) | —               |
| Adapter 2 | server_net (192.168.12.x)   | 192.168.12.13   |
+-----------+-----------------------------+-----------------+
```

## Metasploitable 2 – Network Adapters
• Metasploitable 2: o IP: 192.168.12.14  
• Metasploitable 2 is a purposely vulnerable machine to simulate real-world servers that could be exploited.

```
+-----------+-----------------------------+-----------------+
| Adapter   | Network                     | IP Address      |
+-----------+-----------------------------+-----------------+
| Adapter 1 | server_net (192.168.12.x)   | 192.168.12.14   |
+-----------+-----------------------------+-----------------+
```

## Snort Configuration
I installed Snort on pfSense to monitor network traffic. Snort is a powerful IDS/IPS that can detect common attacks, like port scans and exploitation attempts. Installing it was simple through the pfSense web interface, and I configured it to monitor traffic on the server network (192.168.12.x).

```
+----------+--------------------------+----------------------------------------------+
| Location | Monitored Network        | Notes                                        |
+----------+--------------------------+----------------------------------------------+
| pfSense  | server_net (192.168.12.x)| Detects scans and Samba exploit attempts     |
+----------+--------------------------+----------------------------------------------+
```
