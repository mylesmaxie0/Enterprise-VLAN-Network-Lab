# VLAN Configuration with DHCP and Inter-VLAN Routing Using L3 Switch
> ### Project Overview
This project demonstrates the configuration of three Virtual Local Area Networks (VLANs) in a Cisco Packet Tracer environment, using a Layer 3 (L3) switch to perform inter-VLAN routing and act as a DHCP server for dynamic IP assignment. The network consists of one L3 switch, one Layer 2 (L2) switch, and nine PCs, divided into three VLANs: VLAN 10 ("Sales"), VLAN 20 ("Engineering"), and VLAN 30 ("IT"). The project showcases VLAN setup, trunk link configuration, inter-VLAN routing on an L3 switch, and DHCP configuration, with connectivity tested using the ping command. This project highlights VLAN segmentation, trunking, L3 switching, and dynamic IP allocation.
#


> ### Project Objectives
- Configure three VLANs to segment network traffic.
- Set up trunk links between switches and the router to carry VLAN traffic.
- Implement inter-VLAN routing using an L3 switch.
- Configure the L3 switch as a DHCP server to assign IP addresses dynamically for each VLAN.
- Verify connectivity and DHCP functionality across all VLANs.
- Document the setup process.
#
> ### Network Diagram
#
<img width="848" alt="Screenshot 2025-06-17 at 5 12 46 PM" src="https://github.com/user-attachments/assets/771bbc32-b445-48e9-836d-06ac761e15a1" />


> ###  Devices
- 10 PCs (PC1 - PC5 for VLAN 10 "Sales", PC6 & PC7 for VLAN 20 "Enginnering", PC8 - PC10 for VLAN 30 "IT")

> ### IP Address Scheme

#### VLAN 10 (Sales): 10.0.0.0/26 (10.0.0.0–10.0.0.63)
- PCs: Dynamically assigned from 10.0.0.10–10.0.0.50
- L3 Switch VLAN Interface: 10.0.0.1 (default gateway)



#### VLAN 20 (Engineering): 10.0.0.64/26 (10.0.0.64–10.0.0.127)
- PCs: Dynamically assigned from 10.0.0.70–10.0.0.110
- L3 Switch VLAN Interface: 10.0.0.65 (default gateway)



#### VLAN 30 (IT): 10.0.0.128/26 (10.0.0.128–10.0.0.191)
- PCs: Dynamically assigned from 10.0.0.130–10.0.0.170
- L3 Switch VLAN Interface: 10.0.0.129 (default gateway)





> ### Learning Outcomes
- Mastered configuration of multiple VLANs for network segmentation.
- Learned to set up trunk links for VLAN traffic across switches.
- Implemented inter-VLAN routing using a Multilayer Switch.
- Configured and verified DHCP services for dynamic IP allocation in multiple VLANs.
- Gained experience with Cisco IOS commands for switches, routers.
- Developed skills in network testing, troubleshooting, and documentation.


