# Home Security Operations Center (SOC) 🔒
## Detecting Port Scans with Wireshark in a Virtual Lab Environment

![Project Banner](https://img.shields.io/badge/Project-Home%20SOC-blue)
![Tools](https://img.shields.io/badge/Tools-Kali%20Linux%20%7C%20Wireshark%20%7C%20Nmap%20%7C%20pfSense%20%7C%20Metasploitable-green)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner%20Friendly-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Lab Environment Setup](#-lab-environment-setup)
- [Step-by-Step Execution](#-step-by-step-execution)
- [Network Diagram](#-network-diagram)
- [Commands Used](#-commands-used)
- [Wireshark Analysis](#-wireshark-analysis)
- [Detection Methodology](#-detection-methodology)
- [Key Findings](#-key-findings)
- [Screenshots](#-screenshots)
- [Challenges Faced](#-challenges-faced)
- [Conclusion](#-conclusion)
- [References](#-references)

---

## 📖 Project Overview

This project simulates a real-world Security Operations Center (SOC) scenario where I:
- **Acted as an attacker**: Performed a port scan from Kali Linux against Metasploitable
- **Acted as a defender**: Captured and analyzed network traffic using Wireshark
- **Documented findings**: Created a professional report on detecting reconnaissance activity

**Goal**: Understand how attackers perform reconnaissance and how defenders can detect it using packet analysis.

---

## 🖥️ Lab Environment Setup

### Virtual Machines Used
| VM | Role | IP Address | Purpose |
|:---|:---|:---|:---|
| **pfSense** | Firewall/Gateway | 192.168.1.1 | Network routing and isolation |
| **Kali Linux** | Attacker | 192.168.1.100 | Running port scans (Nmap) |
| **Metasploitable** | Victim/Target | 192.168.1.50 | Vulnerable machine being scanned |

### Network Configuration
- **Network Type**: Host-only / Internal Network
- **Subnet**: 192.168.1.0/24
- **DHCP**: Enabled on pfSense

---

## 🗺️ Network Diagram
