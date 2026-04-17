# School-Network-System
Internet-independent school network using GNS3, VLANs, Moodle, DNS, DHCP
# Local Network System For Sharing School

## 📌 Project Overview

This project presents a complete Local Area Network (LAN) designed for a school environment that operates entirely without internet access.

The system provides:

* DHCP & DNS services
* Internal web portal
* Moodle LMS
* File sharing (Samba & FTP)
* VLAN segmentation with security (ACL)

## 🧠 Technologies Used

* GNS3
* Cisco IOS
* Ubuntu 24.04
* Apache
* Moodle 4.4
* BIND9 (DNS)
* ISC-DHCP
* Samba & vsftpd

## 🌐 Network Design

The network is divided into 4 VLANs:

* VLAN 10: Administration
* VLAN 20: Teachers
* VLAN 30: Students
* VLAN 40: Servers

Inter-VLAN routing is implemented using Router-on-a-Stick.

## 🔐 Security

ACL is applied to block students from accessing:

* Admin VLAN
* Teacher VLAN

While still allowing access to:

* Server VLAN (Moodle & Files)

## 💻 Features

* Internal domain: school.local
* Moodle LMS fully working offline
* Role-based web portals (Admin / Teacher / Student)
* File sharing system
* Full network isolation and control

## 📸 Screenshots

See `/screenshots` folder

## 📄 Report

Full report is available in `/report`

## 🚀 Conclusion

This project proves that a full digital educational system can run completely without internet using open-source tools.
