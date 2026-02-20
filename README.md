# home-security-operations-center
A hands on project simulating a port scan attack and detection using Wireshark in a virtual lab environment.
# Home Security Operations Center 🔒

[![Project Status](https://img.shields.io/badge/status-completed-brightgreen)]()
[![License](https://img.shields.io/badge/license-MIT-blue)]()

A hands-on cybersecurity project simulating a port scan attack and detection using Wireshark in a virtual lab environment.

## 📋 Project Overview
This project demonstrates how to detect network reconnaissance activity by:
- Running an Nmap SYN scan from Kali Linux against Metasploitable
- Capturing the malicious traffic using Wireshark
- Analyzing patterns to identify the port scan

## 🛠️ Lab Environment
- **pfSense**: Network firewall/gateway
- **Kali Linux**: Attacker machine running Nmap
- **Metasploitable**: Vulnerable target machine
- **Wireshark**: Packet analysis tool

## 📊 Key Findings

### Detecting the SYN Scan
Using Wireshark's display filter:
