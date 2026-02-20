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

                      HOME SOC LAB NETWORK
                    ──────────────────────
                      192.168.1.0/24


                        ┌─────────────┐
                        │   Internet  │
                        │   (Isolated)│
                        └──────┬──────┘
                               │
                        ┌──────▼──────┐
                        │   pfSense   │
                        │  Firewall   │
                        │ 192.168.1.1 │
                        └──────┬──────┘
                               │
                               │
       ┌───────────────────────┼───────────────────────┐
       │                       │                       │
       ▼                       ▼                       ▼
┌─────────────┐         ┌─────────────┰─────────────┐
│    Kali     │         │   Wireshark  │             │
│  Attacker   │◄───────►│   Capture    │             │
│192.168.1.101│         │    Point     │             │
└─────────────┘         └─────────────┘             │
       │                                            │
       └────────────────────────────────────────────┘
                          │
                          ▼
                ┌─────────────────┐
                │ Metasploitable  │
                │    Target       │
                │ 192.168.1.100   │
                └─────────────────┘

                🔴 = Attacker
                🟢 = Target
                🔵 = Monitoring Point
