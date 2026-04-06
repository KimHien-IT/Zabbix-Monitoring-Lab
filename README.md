# 📊 Zabbix Security Monitoring Lab (CentOS Stream 9)

## 📌 Overview

This repository hosts a Zabbix-based Security Monitoring Lab designed to simulate an enterprise-level monitoring and alerting infrastructure. Built on CentOS Stream 9, the project demonstrates the integration of system performance tracking with proactive security event detection. By monitoring Windows Event Logs (ID 4625), the system identifies unauthorized access attempts in real-time and dispatches automated alerts via Discord webhooks, bridging the gap between infrastructure health and security awareness.
The lab demonstrates:

* System monitoring
* Network design (simulated environment)
* Security event detection
* Real-time alerting integration

---

## 🏗️ Architecture

* **Zabbix Server** (CentOS Stream 9)
* **Windows Server 2022** (Zabbix Agent)
* **Windows 10 Client** (Zabbix Agent)

Each machine is configured with:

* **NAT Network** → Internet access (simulated via VMware)
* **Host-only Network** → Internal communication (LAN simulation)

---

## 🌐 Network Design

* Internal network simulates a **LAN environment**
* NAT network simulates **Internet connectivity**
* Network segmentation:

  * Monitoring Network
  * Internal Network

> Note: Network components such as Firewall, Switches, and Trunk Links are **conceptual and simulated** in the virtual environment.

---

## 🔍 Monitoring Mechanism

* Zabbix Server collects metrics via **Zabbix Agent**
* Communication:

  * **TCP Port 10050 (Agent passive checks)**

### Metrics monitored:

* CPU usage
* Memory usage
* Disk usage
* Network activity

---

## 🔐 Security Monitoring

This lab includes **basic security monitoring capabilities**:

* Monitored **Windows Event Logs**
* Focused on:

  * **Event ID 4625 (Failed Login Attempt)**
* Trigger configured to detect:

  * Unauthorized access attempts (even a single failure)

👉 This simulates:

* Brute-force attempts
* Suspicious login behavior

---

## 🔔 Alerting Integration

* Configured Zabbix Trigger for security events
* Integrated with **Discord Webhook**

### Alert Flow:

1. Failed login detected (Event ID 4625)
2. Trigger activated in Zabbix
3. Alert sent to Discord in real-time

---

## 🖼️ Topology
<p align="center">
<img width="814" height="806" alt="Topology" src="https://github.com/user-attachments/assets/593e021b-a0df-42ad-a327-65c6544bccda" />
</p>
---

## ⚙️ Technologies Used

* Zabbix Server
* CentOS Stream 9
* Windows 10
* Windows Server 2022
* VMware Workstation

---

## 🚀 Features

* Agent-based monitoring system
* Real-time performance metrics
* Security event detection (Windows Event Log)
* Trigger-based alert system
* Discord integration for instant notifications
* Simulated enterprise network design

---

## 📷 Screenshots

### Dashboard
<p align="center">
<img width="1707" height="917" alt="Dashboard" src="https://github.com/user-attachments/assets/f9acb219-3560-4878-84a3-b6a167c9f87d" />
</p>
### Host Monitoring
<p align="center">
<img width="1717" height="921" alt="Host" src="https://github.com/user-attachments/assets/572263f5-d26e-45f3-9cb0-56b29aabb7bb" />
</p>
### Trigger Alert
<p align="center">
<img width="1717" height="915" alt="Trigger-alert" src="https://github.com/user-attachments/assets/43bff103-77a6-4ecf-8a44-b41d4cf15b96" />
</p>
### Discord Alert
<p align="center"
<img width="1830" height="812" alt="Discord-alert" src="https://github.com/user-attachments/assets/ef7be350-19f6-4992-b083-44eda8e81d27" />
</p>
---

## 📖 Setup Guide

Detailed setup instructions:

👉 `docs/setup-guide.md`

---

## 🎯 Learning Outcomes

* Deploy and configure Zabbix monitoring system
* Understand agent-based monitoring architecture
* Monitor Windows Event Logs for security events
* Detect failed login attempts using triggers
* Integrate alert systems with external platforms (Discord)
* Design a simulated enterprise network environment

---

## ⚠️ Notes

* This is a **lab environment**, not production
* Network infrastructure is **simulated**
* VMware NAT is used for Internet access
* Host-only network is used for internal communication

---

## 👨‍💻 Author

* Nguyễn Thị Kim Hiền
* Major: Network & Information Security

---

## 📌 Future Improvements

* Detect multiple failed logins (brute-force detection)
* Email / Telegram alert integration
* Grafana dashboard integration
* SNMP monitoring
* Log aggregation & SIEM integration

---
