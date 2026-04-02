# 🔹 Cisco Lab: Implement IP SLA (ENCOR)

This lab demonstrates the implementation of **IP SLA**, **HSRP tracking**, and **enterprise-level troubleshooting** in a multi-layer Cisco network.

---

## 🖼️ Topology

### 🔸 Lab Diagram
![Topology](./topology.png)

### 🔸 Live Configuration / Lab Environment
![Console](./console.png)

---

## 📌 Overview

In this lab, I built and tested a multi-device Cisco topology consisting of:

- Core routers (R1, R2, R3)
- Distribution switches (D1, D2)
- Access switch (A1)
- End devices (PC1, PC2)

The main goal was to implement **IP SLA** to monitor network reachability and integrate it with **HSRP** for dynamic failover.

---

## 🔧 Technologies Used

- Cisco IOS
- IP SLA (ICMP Echo)
- HSRP (Hot Standby Router Protocol)
- VLANs & Inter-VLAN Routing
- EtherChannel (LACP)
- Trunking (802.1Q)
- DHCP & DHCP Relay
- IPv4 & IPv6

---

## 🧠 Lab Objective

The objective of this lab was to:

- Configure **IP SLA** to monitor remote network reachability  
- Use SLA tracking to influence **HSRP failover behavior**  
- Ensure **high availability** for end users  
- Troubleshoot DHCP and L3 connectivity issues  

---

## 🧩 What I Configured

---

### 🔹 1. VLAN & Layer 3 Interfaces

Configured SVIs on distribution switches:

```bash
interface vlan 2
 ip address 10.0.2.1 255.255.255.0

interface vlan 3
 ip address 10.0.3.1 255.255.255.0

🔹 2. EtherChannel (LACP)
Configured aggregated trunk links between switches:
interface range g1/0/1 - 4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
👉 Provided redundancy and increased bandwidth.

🔹 3. DHCP & Relay Configuration
Identified that DHCP was not working due to Layer 3 boundaries.
✔ Implemented DHCP relay:
interface vlan 2
 ip helper-address <DHCP_SERVER_IP>
👉 Result: Clients successfully received IP addresses from a remote DHCP server.

🔹 4. IP SLA Configuration
Configured IP SLA to monitor reachability:
ip sla 1
 icmp-echo 192.168.1.1
 frequency 10

ip sla schedule 1 start-time now life forever
👉 This continuously checks network availability.

🔹 5. Tracking Configuration
Linked IP SLA to tracking:
track 1 ip sla 1 reachability
👉 Track object reflects SLA state (UP/DOWN).

🔹 6. HSRP with Tracking
Configured HSRP with tracking:
interface vlan 3
 standby 3 ip 10.0.3.254
 standby 3 priority 120
 standby 3 preempt
 standby 3 track 1 decrement 20
👉 If SLA fails:
priority decreases
HSRP role changes (failover occurs)

🔄 How It Works
IP SLA sends ICMP echo to a remote device
Track object monitors SLA status
If SLA fails → track goes DOWN
HSRP priority is reduced
Another switch becomes Active gateway
