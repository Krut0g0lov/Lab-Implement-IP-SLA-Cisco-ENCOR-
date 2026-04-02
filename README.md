Lab: Implement IP SLA (Cisco ENCOR)

This lab focuses on implementing and verifying IP SLA (Service Level Agreement) to monitor network performance and troubleshoot connectivity in a multi-switch environment.
🔧 Technologies & Concepts
Cisco IOS
IP SLA (ICMP Echo)
VLAN & Trunking (802.1Q)
DHCP & DHCP Relay
Layer 2 / Layer 3 interaction
Network troubleshooting
🧠 Lab Objective
The goal of this lab was to configure IP SLA operations to actively monitor network reachability and performance between devices.
📌 IP SLA allows Cisco devices to generate synthetic traffic (e.g., ping, TCP, UDP) and measure metrics such as:
latency
packet loss
response time
🧩 What I Configured
🔹 1. VLAN & L3 Interface Setup
Configured VLAN interfaces (SVI) for inter-VLAN communication
Assigned IP addressing to enable L3 connectivity between switches
🔹 2. DHCP Troubleshooting & Relay
Identified that DHCP traffic was not crossing L3 boundaries
Implemented DHCP relay using:
interface vlan 2
 ip helper-address <DHCP-SERVER-IP>
👉 Result: Clients successfully received IP addresses from a remote DHCP server
🔹 3. IP SLA Configuration
Configured an IP SLA operation using ICMP echo:
ip sla 1
 icmp-echo <destination-ip>
 frequency 10
ip sla schedule 1 start-time now life forever
💡 What IP SLA Does
Sends periodic test traffic (like ping)
Measures round-trip time and availability
Helps detect failures proactively
Can be used for routing decisions and failover
🔹 4. Verification & Monitoring
Used commands like:
show ip sla statistics
show ip sla summary
👉 Verified:
SLA operation status
response times
connectivity stability
⚠️ Challenges Faced
DHCP not working due to missing relay configuration
Understanding L2 broadcast limitations across L3 boundaries
Identifying correct source interface for IP SLA
🚀 Key Learning Outcomes
Deep understanding of how IP SLA works internally
Difference between connectivity vs performance monitoring
Practical troubleshooting of DHCP across routed networks
Experience with real-world enterprise monitoring tools
🧩 Final Result
Fully functional IP SLA monitoring
Stable L3 communication
Working DHCP via relay
Verified network performance metrics
