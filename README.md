# Home Security Operations Center (SOC) 🔒
## Detecting Port Scans with Wireshark in a Virtual Lab Environment

![Project Banner](https://img.shields.io/badge/Project-Home%20SOC-blue)
![Tools](https://img.shields.io/badge/Tools-Kali%20Linux%20%7C%20Wireshark%20%7C%20Nmap%20%7C%20pfSense%20%7C%20Metasploitable-green)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner%20Friendly-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)
![Date](https://img.shields.io/badge/Date-February%202025-lightgrey)

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Lab Environment Setup](#-lab-environment-setup)
- [Network Diagram](#-network-diagram)
- [Step-by-Step Execution](#-step-by-step-execution)
- [Commands Used](#-commands-used-complete-reference)
- [Wireshark Analysis](#-wireshark-analysis)
- [Detection Methodology](#-detection-methodology)
- [Key Findings](#-key-findings)
- [Screenshots](#-screenshots)
- [Indicators of Compromise (IOCs)](#-indicators-of-compromise-iocs)
- [Challenges Faced](#-challenges-faced-and-solutions)
- [Conclusion](#-conclusion)
- [References](#-references-and-resources)
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

**Step 4: Apply Display Filters to Find the Scan**

Now we'll use Wireshark's powerful display filters to isolate the port scan traffic.

Filter 1: View All Traffic to Metasploitable
Packets from Kali → Metasploitable

- Packets from Metasploitable → Kali
- ARP traffic
- Any other network chatter

<img width="900" height="600" alt="image" src="https://github.com/user-attachments/assets/c2fcad0a-e450-4d0b-9458-1114fb4de7fa" />


