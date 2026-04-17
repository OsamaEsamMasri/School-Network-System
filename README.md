# 🏫 Local Network System For Sharing School

![Network](https://img.shields.io/badge/Network-Local%20LAN-1D4ED8?style=for-the-badge&logo=cisco)
![GNS3](https://img.shields.io/badge/Simulation-GNS3-00A9CE?style=for-the-badge)
![Ubuntu](https://img.shields.io/badge/Server-Ubuntu%2024.04-E95420?style=for-the-badge&logo=ubuntu)
![Moodle](https://img.shields.io/badge/LMS-Moodle%204.4-F98012?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

> A complete, internet-independent Local Area Network designed for a school environment, providing all essential digital educational services entirely within the local network.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Network Topology](#-network-topology)
- [VLAN Design](#-vlan-design)
- [Services](#-services)
- [Technologies Used](#-technologies-used)
- [Configuration Files](#-configuration-files)
- [Web Portals](#-web-portals)
- [Security — ACL](#-security--acl)
- [How to Run](#-how-to-run)
- [Screenshots](#-screenshots)
- [Project Structure](#-project-structure)

---

## 🌐 Project Overview

This project designs and implements a **fully self-contained school Local Area Network (LAN)** that operates **without any internet connection**. It provides:

- 📡 **Automatic IP assignment** (DHCP)
- 🔍 **Internal DNS resolution** (`school.local`)
- 🌐 **School intranet web portal**
- 🎓 **Moodle 4.4 Learning Management System**
- 📁 **Samba file sharing + FTP server**
- 🛡️ **Role-based network security (ACL)**
- 👥 **Three role-based web portals** (Admin / Teacher / Student)

---

## 🗺️ Network Topology

```
MainRouter (Cisco IOS)
    │
    └── MainSwitch (Core Switch)
            ├── SW-Admin    → PC-Admin    [VLAN 10 — 192.168.10.0/24]
            ├── SW-Teachers → PC-Teachers [VLAN 20 — 192.168.20.0/24]
            ├── SW-Students → PC-Students [VLAN 30 — 192.168.30.0/24]
            └── SW-Servers
                    ├── DNS-DHCP-WEB-Moodle  [192.168.40.10]
                    └── Samba-FTP            [192.168.40.20]
```
<img width="979" height="598" alt="لقطة شاشة 2026-04-16 130243" src="https://github.com/user-attachments/assets/64c60887-b539-471f-b6b4-2f92c3ac7bf9" />


**Simulation Tool:** GNS3 v2.2 on VMware Workstation  
**Router/Switch:** Cisco IOS (CiscoIOSvL2 15.2)  
**Servers:** Ubuntu 24.04 LTS

---

## 🔀 VLAN Design

| VLAN | Name | Network | Gateway | DHCP Range |
|------|------|---------|---------|------------|
| 10 | Administration | 192.168.10.0/24 | 192.168.10.1 | .10 – .50 |
| 20 | Teachers | 192.168.20.0/24 | 192.168.20.1 | .10 – .100 |
| 30 | Students | 192.168.30.0/24 | 192.168.30.1 | .10 – .200 |
| 40 | Servers | 192.168.40.0/24 | 192.168.40.1 | Static IPs |

**Routing:** Router-on-a-Stick (sub-interfaces with dot1Q encapsulation)

---

## ⚙️ Services

### Primary Server — `192.168.40.10`
| Service | Software | Port | Purpose |
|---------|----------|------|---------|
| DNS | BIND9 | 53 | Internal hostname resolution for `school.local` |
| DHCP | ISC-DHCP | 67 | Automatic IP assignment for VLANs 10, 20, 30 |
| Web Server | Apache 2.4 | 80 | School intranet portal |
| LMS | Moodle 4.4 | 80 | E-learning platform at `moodle.school.local` |
| Database | MySQL 8.0 | 3306 | Moodle backend |

### Secondary Server — `192.168.40.20`
| Service | Software | Port | Purpose |
|---------|----------|------|---------|
| File Sharing | Samba 4.x | 445 | SMB/CIFS shared folders |
| FTP | vsftpd 3.0 | 21 | FTP file access |

---

## 🛠️ Technologies Used

```
Network Simulation:  GNS3 v2.2.54
Virtualization:      VMware Workstation Pro
Router/Switch OS:    Cisco IOS (CiscoIOSvL2)
Server OS:           Ubuntu 24.04 LTS
Web Server:          Apache 2.4
DNS:                 BIND9 9.18.x
DHCP:                ISC-DHCP 4.4.3
LMS:                 Moodle 4.4 (MOODLE_404_STABLE)
Database:            MySQL 8.0
PHP:                 PHP 8.x + extensions
File Sharing:        Samba 4.x
FTP:                 vsftpd 3.0.5
```

---

## 📁 Configuration Files

### Router — Inter-VLAN + DHCP + ACL
```cisco
! Sub-interfaces for Inter-VLAN routing
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

interface FastEthernet0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0

! DHCP Pools
ip dhcp pool ADMIN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.40.10

ip dhcp pool TEACHERS
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.40.10

ip dhcp pool STUDENTS
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.40.10

! ACL — Block Students from Admin and Teacher VLANs
ip access-list extended BLOCK_STUDENTS
 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 permit ip any any

interface FastEthernet0/0.30
 ip access-group BLOCK_STUDENTS in
```

See full configs in [`/configs/`](./configs/) directory.

---

## 🌐 Web Portals

| Portal | URL | Credentials | Description |
|--------|-----|-------------|-------------|
| Login | `http://192.168.40.10` | — | Unified login page |
| Admin | `http://192.168.40.10/admin.html` | admin / admin123 | Full network dashboard |
| Teacher | `http://192.168.40.10/teacher.html` | teacher / teacher123 | File upload portal |
| Student | `http://192.168.40.10/student.html` | student / student123 | File browser |
| Moodle | `http://192.168.40.10/moodle` | admin / [set during install] | LMS platform |

### Admin Portal Features
- Live network dashboard with device IP list
- VLAN details (network, gateway, DHCP range)
- ACL rules and test results
- File management (view/delete all uploaded files)
- Service status panel

### Teacher Portal Features
- Upload files with subject, type, and audience metadata
- View and delete own uploaded files
- Direct link to Moodle LMS

### Student Portal Features
- Browse files uploaded by teachers
- Search and filter by subject/type
- Download files
- Direct link to Moodle LMS
- Auto-refreshes every 10 seconds

---

## 🛡️ Security — ACL

```
ACL Name: BLOCK_STUDENTS
Applied:  interface FastEthernet0/0.30 — inbound

Rule 1: DENY  192.168.30.0/24 → 192.168.10.0/24  (Students cannot reach Admin VLAN)
Rule 2: DENY  192.168.30.0/24 → 192.168.20.0/24  (Students cannot reach Teacher VLAN)
Rule 3: PERMIT any → any                           (All other traffic allowed)
```

**Verified Test Results:**
- ✅ `ping 192.168.10.1` from PC-Students → **BLOCKED** (Packet filtered)
- ✅ `ping 192.168.20.1` from PC-Students → **BLOCKED** (Packet filtered)
- ✅ `ping 192.168.40.10` from PC-Students → **ALLOWED** (0% packet loss)

---

## 🚀 How to Run

### Requirements
- GNS3 v2.2+ installed
- VMware Workstation
- Cisco IOS image (CiscoIOSvL2 15.2)
- Ubuntu 24.04 Desktop ISO

### Steps

**1. Import topology in GNS3**
```
File → Import portable project → school-network.gns3project
```

**2. Configure Router (paste from configs/MainRouter.txt)**
```
> enable
> configure terminal
> [paste configuration]
```

**3. Configure Switches (repeat for each switch)**
```
> enable
> configure terminal
> [paste configuration from configs/SW-*.txt]
```

**4. Set up Primary Server (192.168.40.10)**
```bash
# Static IP
sudo bash -c 'cat > /etc/netplan/01-netcfg.yaml << EOF
network:
  version: 2
  renderer: networkd
  ethernets:
    ens3:
      dhcp4: no
      addresses: [192.168.40.10/24]
      routes:
        - to: default
          via: 192.168.40.1
      nameservers:
        addresses: [192.168.40.10]
EOF'
sudo netplan apply

# Install services
sudo apt update && sudo apt install -y \
  apache2 bind9 bind9-utils isc-dhcp-server \
  mysql-server php php-mysql php-xml php-curl \
  php-zip php-gd php-mbstring php-intl php-soap

# Copy config files
sudo cp server-configs/dhcpd.conf /etc/dhcp/
sudo cp server-configs/named.conf.local /etc/bind/
sudo cp server-configs/db.school.local /etc/bind/
sudo cp web-portals/* /var/www/html/

# Start services
sudo systemctl restart isc-dhcp-server named apache2
```

**5. Set up Secondary Server (192.168.40.20)**
```bash
# Install Samba and FTP
sudo apt install -y samba vsftpd
sudo mkdir -p /srv/school/{shared,teachers,students}
sudo cp server-configs/smb.conf /etc/samba/
sudo cp server-configs/vsftpd.conf /etc/
sudo systemctl restart smbd vsftpd
```

**6. Install Moodle**
```bash
cd /var/www/html
sudo git clone --depth=1 --branch=MOODLE_404_STABLE \
  https://github.com/moodle/moodle.git moodle
sudo mkdir -p /var/moodledata
sudo chown -R www-data:www-data /var/moodledata /var/www/html/moodle
# Then open http://192.168.40.10/moodle to complete setup
```

## 👤 Author

Osama Masri
Computer Networks Course — Najah National University 

---

<div align="center">
  <strong>Built with 💙 using GNS3 + Cisco IOS + Ubuntu + Moodle</strong><br>
  <em>school.local — 100% Internet-Free Educational Network</em>
</div>
