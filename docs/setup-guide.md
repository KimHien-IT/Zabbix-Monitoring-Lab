# 📖 Setup Guide – Zabbix Security Monitoring Lab

## 1. Environment Setup

* VMware Workstation
* 1 VM: CentOS Stream 9 (Zabbix Server)
* 2 VMs:

  * Windows 10
  * Windows Server 2022

Each VM is configured with:

* NAT Adapter (Internet access)
* Host-only Adapter (Internal network)

---

## 2. Network Configuration

Configure IP addresses for internal communication:

| Machine             | IP Address    |
| ------------------- | ------------- |
| Zabbix Server       | 192.168.10.10 |
| Windows Server 2022 | 192.168.10.20 |
| Windows 10          | 192.168.10.30 |

Test connectivity using ping between machines.

CentOS 9 ping to Windows Server 2022:
<p align="center">
 <img src="images/ping2.png" width="800">
 </p>

 CentOS 9 ping to Windows 10:
<p align="center">
 <img src="images/ping4.png" width="800">
 </p>

Window 10 ping to CentOS 9 and Windows Server 2022:
<p align="center">
 <img src="images/ping1.png" width="800">
 </p>

 Window Server 2022 ping to CentOS 9 and Windows 10:
<p align="center">
 <img src="images/ping3.png" width="800">
 </p>
---

## 3. Install Zabbix Server (CentOS 9)

**Install Zabbix repository:**
* rpm -Uvh https://repo.zabbix.com/zabbix/7.0/centos/9/x86_64/zabbix-release-latest-7.0.el9.noarch.rpm

**Install packages:**
* dnf install zabbix-server-mysql zabbix-web-mysql zabbix-nginx-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent 

**Setup database (MySQL/MariaDB):**
* mysql -u root -p
* password
  * mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
  * mysql> create user 'zabbix-sql'@'localhost' identified by 'sql';
  * mysql> grant all privileges on zabbix.* to 'zabbix-sql'@'localhost';
  * mysql> set global log_bin_trust_function_creators = 1;
  * mysql> quit; 
* Configure /etc/zabbix/zabbix_server.conf
  * DBUser=zabbix-sql
  * DBPassword=sql
* systemctl restart zabbix-server
* zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql -uzabbix-sql -p zabbix
    
**Start services:**

zabbix-server: 
* systemctl start zabbix-server
* systemctl enable zabbix-server

zabbix-agent: 
* systemctl start zabbix-agent
* systemctl enable zabbix-agent
  
Nginx:
* systemctl start nginx
* systemctl enable nginx
  
MariaDB:
* systemctl start mariadb
* systemctl enable mariadb
  
**Open port Firewall:**
* firewall-cmd --add-port=10051/tcp --permanent
* firewall-cmd --add-port=10050/tcp --permanent
* firewall-cmd --add-service=http --permanent
* firewall-cmd --reload
  
**Install SElinux policy**
* setsebool -P zabbix_can_network on
* setsebool -P httpd_can_connect_zabbix on
* setsebool -P zabbix_can_network_connect_db on
  
Access Web UI: "http://192.168.10.10
<p align="center">
  <img src="images/zabbixsetup.png" width="800">
</p>
Setup SQL:
<p align="center">
 <img src="images/zabbixsqlsetup.png" width="800">
</p>
Setup Information:
<p align="center">
 <img src="images/zabbixsetupinformation.png" width="800">
 </p>

## 4. Install Zabbix Agent (Windows)

* Download Zabbix Agent for Windows
* Install on:   https://cdn.zabbix.com/zabbix/binaries/stable/7.0/7.0.24/zabbix_agent-7.0.24-windows-amd64-openssl.msi
**Windows 10**

* Edit config file:
```

Server=192.168.10.10
ServerActive=192.168.10.10
Hostname=WIN10
```
**Windows Server 2022**

* Edit config file:

```
Server=192.168.10.10
ServerActive=192.168.10.10
Hostname=WIN SERVER-2022
```

* Start Zabbix Agent service.

---

## 5. Add Hosts to Zabbix

* Login to Zabbix Web UI
* Go to: Data collection -> Host -> Creat Host
  * Host: the name of the client machine.
  * Templates: a predefined set of monitoring items, triggers, and graphs applied to a host.
  * Host Group: a logical group of hosts for easier management and monitoring.
  * Interfaces: defines how the Zabbix server communicates with the host (e.g., Agent, SNMP, JMX, IPMI).
  * Description: provides additional information or notes about the host.
  * Monitored by: determines whether the host is monitored directly by the Zabbix server or through a proxy.
  **Windows 10:**
 <p align="center">
  <img src="images/createhost1.png" width="800">
  </p>
  
  ** Windows Server 2022:**
 <p align="center">
  <img src="images/createhost2.png" width="800">
  </p>
---

## 6. System Monitoring
Basic monitoring:
<p align="center">
 <img src="images/dashboard.png" width="800">
</p>

Custom monitoring:
<p align="center">
 <img src="images/customdashboard.png" width="800">
</p>

Important metrics:
* CPU usage
* Memory usage
* Disk usage
* Network traffic

## 7. Configure Windows Event Log Monitoring

On Windows Server 2022:

Enable Event Log monitoring via Zabbix Agent:
 <p align="center">
  <img src="images/eventlog.png" width="800">
 </p>
Security Log:
  <p align="center">
<img src="images/eventviewer.png" width="800">
  </p>
Filter:
 * Event ID: 4625 (failed Login)
<p align="center">
 <img src="images/4625.png" width="800">
</p>
---

## 8. Create Triggers
<p align="center">
 <img src="images/createtrigger.png" width="800">
</p>
Create trigger conditions:
<p align="center">
 <img src="images/createconditionfromtrigger.png" width="800">
</p>
Create items:
<p align="center">
 <img src="images/createitem.png" width="800">
</p>

---

## 9. Configure Discord Alert

### Step 1: Create Webhook in Discord

* Go to Discord → Server Settings → Integrations → Webhooks
* Copy Webhook URL
<p align="center">
 <img src="images/webhook.png" width="800">
</p>
---

### Step 2: Configure in Zabbix

* Go to: Administration → Media types
* Create a new media type:
  * Type: Webhook
  * Paste Discord Webhook URL
<p align="center">
 <img src="images/mediatype.png" width="800">
</p>

* Add the media type to the host:
<p align="center">
 <img src="images/mediauser.png" width="800">
</p>
---

### Step 3: Create Action

* Go to: Configuration → Actions
* Create trigger action:

  * Condition: Trigger = failed Login
  * Operation: Send message to Discord
Create action:
<p align="center">
 <img src="images/createaction.png" width="800">
</p>
---

## 10. Testing

* Perform a failed login attempt on Windows Server 2022
* Result: Security events (failed logins) are detected

  * Event ID: 4625 generated
<p align="center">
 <img src="images/servertest.png" width="800">
</p>
  * Trigger activated
<p align="center">
 <img src="images/triggeralert.png" width="800">
</p>
  * Alert sent to Discord
<p align="center">
 <img src="images/alertfromdiscord.png" width="800">
</p>
---

## ✅ Result

* Zabbix successfully monitors system performance
* Security events (failed login) are detected
* Real-time alerts are delivered via Discord

---

## 📌 Notes

* This lab simulates enterprise monitoring
* Network is implemented using VMware NAT + Host-only
* Security monitoring focuses on basic intrusion detection scenarios
