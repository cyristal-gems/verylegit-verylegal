# Very Legit, Very Legal: A Noob’s First Cybersecurity Lab

This project is a fully virtualized cybersecurity lab that simulates a realistic attacker–victim–defender workflow using VirtualBox. You’ll stage attacks from **Kali Linux** against **Metasploitable 2** and observe detections on **pfSense + Snort**, covering the full lifecycle from reconnaissance and exploitation to detection and analysis.  
   
The lab runs entirely on isolated virtual networks, allowing you to explore offensive and defensive security techniques safely, legally, and hands-on — a perfect environment for building foundational skills in penetration testing, network defense, and incident response.

---

## 🔐 Features
- **Complete Lab Topology** – Kali (attacker), Metasploitable 2 (target), and pfSense (firewall with Snort IDS/IPS).
- **Hands-On Exploitation** – Use Metasploit to run the Samba `usermap_script` (CVE-2007-2447) and gain shell access.
- **Detection & Visibility** – Verify malicious traffic and intrusion attempts with Snort alerts on pfSense.
- **Network Segmentation** – Recreate internal subnets and gateway policies to mirror real-world environments.
- **Beginner-Friendly** – Includes detailed documentation and walkthroughs for each phase of the attack and defense lifecycle.

---

## 📚 Lab Sections
Each phase of the project is documented in its own markdown guide. Together, they form the complete lab walkthrough:

- [**Lab_Setup_and_Configuration.md**](./Lab_Setup_and_Configuration.md) — Virtual machines, network design, IP planning, and environment configuration.  
- [**Tools_and_System_Analysis.md**](./Tools_and_System_Analysis.md) — Vulnerability exploitation process, tool usage, and system behavior analysis.  
- [**Security_Relevance.md**](./Security_Relevance.md) — Importance of IDS/IPS, Snort deployment details, and key security insights.  
- [**Reflection.md**](./Reflection.md) — Lessons learned, main takeaways, and ideas for future improvements.

---

## 🛠️ Tech Stack

- **Kali Linux** – A penetration-testing distribution used for reconnaissance, vulnerability scanning, exploitation, and post-exploitation activities.  
- **Metasploitable 2** – A purposely vulnerable VM used as the target system to safely demonstrate real-world exploitation scenarios.  
- **Metasploit Framework** – An exploitation toolkit that streamlines payload delivery, session handling, and vulnerability execution.  
- **pfSense** – An open-source firewall/router platform providing segmentation, routing, and a secure layer for traffic monitoring.  
- **Snort** – A network IDS/IPS that inspects traffic, detects malicious patterns, and logs intrusion attempts for analysis.  
- **VirtualBox** – Cross-platform virtualization software that builds a reproducible, isolated lab environment on a single machine.

---

## 📊 Rough Diagram of Network Topology 

<img width="434" height="226" alt="Lab_Setup" src="https://github.com/user-attachments/assets/e2ccd147-27fd-466d-9a8f-2c94111349c5" />

---

## 🤝 Connect

- **GitHub:** [github.com/cyristal-gems](https://github.com/cyristal-gems)  
- **LinkedIn:** [linkedin.com/in/cyristalj](https://linkedin.com/in/cyristalj)  
