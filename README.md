# School-Network-System
Internet-independent school network using GNS3, VLANs, Moodle, DNS, DHCP
# Local Network System For Sharing School

# 🎓 School Network System (Internet-Independent)

A complete local school network that provides digital educational services **without any internet connection**.

---

## 📌 Project Overview

This project presents a fully functional Local Area Network (LAN) designed for schools that suffer from limited or no internet access.

The system simulates a real-world educational infrastructure and delivers essential services entirely within a local environment.

---

## 🚀 Key Features

* 🌐 Internal DNS system (`school.local`)
* 📡 Automatic IP assignment using DHCP
* 🖥️ Local web portal (Admin / Teacher / Student)
* 📚 Moodle LMS running بالكامل بدون إنترنت
* 📂 File sharing using Samba & FTP
* 🔐 VLAN segmentation with ACL security
* 🔄 Full inter-VLAN communication using Router-on-a-Stick

---

## 🧠 Technologies Used

* GNS3 (Network Simulation)
* Cisco IOS (Routing & Switching)
* Ubuntu 24.04 (Server OS)
* Apache HTTP Server
* Moodle 4.4 (LMS)
* BIND9 (DNS)
* ISC-DHCP Server
* Samba & vsftpd (File Sharing)

---

## 🌐 Network Design

The network is divided into 4 VLANs:

| VLAN | Name     | Network         |
| ---- | -------- | --------------- |
| 10   | Admin    | 192.168.10.0/24 |
| 20   | Teachers | 192.168.20.0/24 |
| 30   | Students | 192.168.30.0/24 |
| 40   | Servers  | 192.168.40.0/24 |

Inter-VLAN routing is implemented using **Router-on-a-Stick**.

---

## 🔐 Security

An Extended ACL is applied to enforce security:

* ❌ Students cannot access Admin VLAN
* ❌ Students cannot access Teacher VLAN
* ✅ Students can access Server VLAN (Moodle & Files)

---

## 💻 System Components

### 🖥️ Primary Server (192.168.40.10)

* DNS (BIND9)
* DHCP Server
* Apache Web Server
* Moodle LMS
* MySQL Database

### 📂 Secondary Server (192.168.40.20)

* Samba File Server
* FTP Server

---

## 📸 Screenshots

### 🔹 Network Topology

<img src="https://github.com/user-attachments/assets/ffb6b0c5-f5dc-4c8f-8808-945b539833f9" width="800"/>

### 🔹 VLAN Configuration

<img src="https://github.com/user-attachments/assets/69243cf5-e34c-417f-9112-a13926678c96" width="800"/>

### 🔹 ACL Configuration

<img src="https://github.com/user-attachments/assets/468fa145-c0f0-4a23-8e41-268d91fad273" width="600"/>

### 🔹 Netplan Configuration

<img src="https://github.com/user-attachments/assets/13487d2a-9d75-4834-b94e-38f83c3c3db8" width="500"/>

### 🔹 Services Status

**Apache** <img src="https://github.com/user-attachments/assets/fac453f0-76fc-40ab-bc8e-31f6c45d3998" width="800"/>

**DHCP** <img src="https://github.com/user-attachments/assets/79e352b6-6be9-4e7f-be44-3e0c40f26d35" width="800"/>

**DNS** <img src="https://github.com/user-attachments/assets/0da9b6b8-bb14-489e-ada4-2c147dd3f130" width="800"/>

---

## 📄 Report

The full documentation is available here:
📁 `Network Report.docx`

---

## 🎯 Conclusion

This project demonstrates that a complete digital educational system — including an LMS — can be deployed and operated entirely within a local network.

It provides a practical and cost-effective solution for schools in areas with limited internet access.

---

## 👩‍💻 Author
Osama Masri


