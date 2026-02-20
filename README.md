# Home Security Operations Center (SOC) 🔒
## Detecting Port Scans with Wireshark in a Virtual Lab Environment

![Project Banner](https://img.shields.io/badge/Project-Home%20SOC-blue)
![Tools](https://img.shields.io/badge/Tools-Kali%20Linux%20%7C%20Wireshark%20%7C%20Nmap%20%7C%20pfSense%20%7C%20Metasploitable-green)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner%20Friendly-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)
![Date](https://img.shields.io/badge/Date-February%202025-lightgrey)

---

## 📋 Table of Contents
## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Lab Environment Setup](#-lab-environment-setup)
- [Network Diagram](#-network-diagram)
- [Step-by-Step Execution](#-step-by-step-execution)

  - [Phase 1: Environment Preparation](#phase-1-environment-preparation)
  - [Phase 2: Launching the Attack](#phase-2-launching-the-attack)
  - [Phase 3: Wireshark Analysis](#phase-3-wireshark-analysis)

- [Indicators of Compromise (IOCs)](#-indicators-of-compromise-iocs)
- [Challenges Faced](#-challenges-faced-and-solutions)
- [Conclusion](#-conclusion)
  - [Project Summary](#project-summary)
  - [Key Achievements](#key-achievements)
  - [What I Learned](#what-i-learned)
  - [Real-World Applications](#real-world-applications)
  - [Challenges Overcome](#challenges-overcome)
  - [Future Improvements](#future-improvements)
- [License](#-license)
- [Author](#-author)

---

## 📖 Project Overview

This project simulates a real-world Security Operations Center (SOC) scenario where I:
- **Acted as an attacker**: Performed a port scan from Kali Linux against Metasploitable
- **Acted as a defender**: Captured and analyzed network traffic using Wireshark
- **Documented findings**: Created a professional report on detecting reconnaissance activity

**Primary Goal**: Understand how attackers perform reconnaissance and how defenders can detect it using packet analysis.

**Secondary Goals**:
- Master Wireshark filters and statistical analysis
- Understand TCP handshake mechanics
- Practice professional security documentation
- Build a portfolio-worthy cybersecurity project

---

## 🖥️ Lab Environment Setup

### Virtual Machines Used
| VM | Role | IP Address | Operating System | Purpose |
|:---|:---|:---|:---|:---|
| **pfSense** | Firewall/Gateway | 192.168.1.1 | FreeBSD | Network routing, firewall, and traffic logging |
| **Kali Linux** | Attacker | 192.168.1.101 | Kali Linux 2024.x | Running port scans using Nmap |
| **Metasploitable** | Victim/Target | 192.168.1.100 | Ubuntu 8.04 | Vulnerable machine being scanned |

### Network Configuration
- **Network Type**: Host-only / Internal Network (isolated from internet)
- **Subnet**: 192.168.1.0/24
- **Subnet Mask**: 255.255.255.0
- **DHCP**: Enabled on pfSense (range: 192.168.1.100 - 192.168.1.200)
- **Default Gateway**: 192.168.1.1 (pfSense)

### System Specifications
- **Hypervisor**: VirtualBox / VMware (specify which you used)
- **Host Machine**: [Your computer specs - optional]
- **RAM Allocation**: 
  - Kali: 4GB
  - Metasploitable: 512MB
  - pfSense: 1GB

---

## 🗺️ Network Diagram

<img width="250" height="500" alt="Isolated Network Attack Flow" src="https://github.com/user-attachments/assets/ff1660a2-fc97-4f6b-8c6e-daddba29c479" />


                🔴 = Attacker
                🟢 = Target
                🔵 = Monitoring Point


**Traffic Flow Legend:**
- 🔴 **Kali (192.168.1.101)** → SYN packets → 🟢 **Metasploitable (192.168.1.100)**
- 🔵 **Wireshark** captures all traffic at the network level

---

## 🚀 Step-by-Step Execution

### Phase 1: Environment Preparation

**Step 1: Verify Network Connectivity**


First, ensure all VMs can communicate with each other:

# Ping Kali To Metasploitable

<img width="515" height="169" alt="image" src="https://github.com/user-attachments/assets/73c568a4-c573-4794-818b-77e7cb72b3b9" />


# Ping Kali To pfSense gateway

<img width="515" height="201" alt="image" src="https://github.com/user-attachments/assets/069d27bd-ff0e-4a08-b9cf-4498dfbd91c0" />

# Ping Metasploitable To Kali

<img width="569" height="162" alt="image" src="https://github.com/user-attachments/assets/8ee79ad8-9100-495e-886c-8abf65b763ee" />



### **Step 2: Setup wireshark **

Open Terminal and hit **sudo wireshark**
Then select your adapter (eth0)

<img width="726" height="587" alt="image" src="https://github.com/user-attachments/assets/0e3258a6-71a4-464b-82dc-5d5da4941ed6" />



### Phase 2: Launching the Attack
**Step 3: Perform SYN Scan from Kali**

<img width="640" height="557" alt="image" src="https://github.com/user-attachments/assets/f5a25d74-ad29-4d48-956e-6dcd6155243b" />

**Step 4: Perform Detailed Version Scan**

<img width="989" height="634" alt="image" src="https://github.com/user-attachments/assets/032e4d69-e4b5-40f9-b913-1cc437058fe5" />


### Phase 3: Wireshark Analysis

**Step 5: Apply Display Filters to Find the Scan**

Now we'll use Wireshark's powerful display filters to isolate the port scan traffic.

Filter 1: View All Traffic to Metasploitable
Packets from Kali → Metasploitable

- Packets from Metasploitable → Kali
- ARP traffic
- Any other network chatter

<img width="900" height="600" alt="image" src="https://github.com/user-attachments/assets/c2fcad0a-e450-4d0b-9458-1114fb4de7fa" />


Filter 2: Isolate SYN Packets (The Magic Filter!)

This is the MOST IMPORTANT FILTER for detecting SYN scans.


| Filter Component | Meaning |
|:---|:---|
| **ip.addr == 192.168.1.100** | Show packets to/from Metasploitable |
| **&&** | AND (all conditions must be true) |
| **tcp.flags.syn == 1** | Packet has SYN flag set |
| **tcp.flags.ack == 0** | Packet does NOT have ACK flag set |

 
<img width="900" height="600" alt="image" src="https://github.com/user-attachments/assets/93206e67-7cdb-4c51-b1f8-ac30d6301616" />


**Step 6: Use Statistics → Conversations**

+ This is where you prove it was a scan, not just normal traffic.
+ Navigate: Statistics → Conversations

<img width="1000" height="895" alt="image" src="https://github.com/user-attachments/assets/c1246d1a-95b4-4764-9fba-882e9e7862d9" />


### Key Indicators of a Port Scan in Conversations:

1. Multiple entries from same source IP to same destination IP
2. Different destination ports for each conversation
3. Very short duration (0.001s) - connections are aborted
4. Small packet count (2 packets per conversation) - SYN then RST
5. Sequential port numbers in the list



## ✅ Conclusion

### Project Summary
This Home Security Operations Center (SOC) project successfully demonstrated the complete cycle of a cybersecurity operation:
- **Simulated a real-world attack** by performing an Nmap SYN scan from Kali Linux (192.168.1.101) against Metasploitable (192.168.1.100)
- **Captured malicious traffic** using Wireshark with proper capture timing (starting capture BEFORE the attack)
- **Detected the scan** using precise Wireshark filters: `ip.addr == 192.168.1.100 && tcp.flags.syn == 1 && tcp.flags.ack == 0`
- **Confirmed findings** through statistical analysis (Statistics → Conversations in Wireshark)
- **Documented the entire process** in a professional, portfolio-ready format

### Key Achievements
| Area | Achievement |
|:-----|:------------|
| **Technical Skills** | Mastered Wireshark filters, Nmap scanning, TCP flag analysis |
| **Detection Capability** | Successfully identified all 178 SYN packets from the scan |
| **Analysis Skills** | Used Conversations tab to prove multiple port connection attempts |
| **Documentation** | Created comprehensive report with screenshots and command references |

### What I Learned

**Technical Takeaways:**
1. **TCP Handshake Deep Dive**: Understood the difference between normal connections (SYN → SYN-ACK → ACK) and SYN scans (SYN → SYN-ACK → RST)
2. **Wireshark Mastery**: Learned to use display filters, capture filters, and statistical tools effectively
3. **Attack Methodology**: Understood how attackers perform reconnaissance before launching exploits
4. **Defense Techniques**: Know exactly what indicators to look for when monitoring network traffic

**Professional Takeaways:**
1. **Documentation Matters**: Clear, well-organized reports are essential in cybersecurity
2. **Methodical Approach**: Capture FIRST, attack SECOND, analyze THIRD - order matters!
3. **Evidence Collection**: Screenshots and packet captures provide proof of findings
4. **Communication**: Translating technical findings into clear explanations is a valuable skill

### Real-World Applications

This project directly translates to actual SOC analyst responsibilities:

| Scenario | How This Project Prepares You |
|:---------|:------------------------------|
| **Incident Response** | When a suspicious IP is reported, you know how to analyze its traffic patterns |
| **Threat Hunting** | You can proactively look for SYN scan patterns in network logs |
| **Forensic Analysis** | You can examine PCAP files to reconstruct attacker activities |
| **Security Monitoring** | You understand what alerts to create for scan detection |

### Challenges Overcome

Throughout this project, I successfully troubleshooted:
- ✅ Network connectivity issues between VMs
- ✅ Proper Wireshark capture timing (capture BEFORE attack)
- ✅ Filter syntax and application
- ✅ Interpreting TCP flags correctly
- ✅ Gathering clear screenshot evidence

### Future Improvements

If I were to extend this project, I would:

1. **Add More Scan Types**
   ```bash
   # UDP scan detection
   sudo nmap -sU 192.168.1.100
   
   # FIN scan detection (stealthier)
   sudo nmap -sF 192.168.1.100
   
   # Xmas tree scan
   sudo nmap -sX 192.168.1.100
## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Copyright (c) 2025 [M.T.Malinga]
