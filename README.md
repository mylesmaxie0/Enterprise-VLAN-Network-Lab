# VLAN Implementation with Router-on-a-Stick Configuration 
> ### Project Overview
This project demonstrates fundamental networking concepts by implementing VLAN segmentation across three departments (Sales, IT, HR) on a single physical switch. The solution uses router-on-a-stick configuration to enable controlled inter-VLAN communication while maintaining security boundaries.

> ### Features
- Multi-VLAN Configuration: Separate VLANs for different departments
- Trunk Port Setup: 802.1Q encapsulation for VLAN tag transport
- Router-on-a-Stick: Single router interface serving multiple VLANs
- Inter-VLAN Routing: Communication between different network segments
- Access Control: Security policies restricting cross-department access
- Scalable Design: Easy to add new VLANs and departments

#
> ### Network Topology
<img width="842" height="620" alt="Screenshot 2025-08-08 at 1 34 37 PM" src="https://github.com/user-attachments/assets/d05d3d0d-6832-466d-896b-4ebc7e281075" />



### VLAN Scheme
| VLAN ID | Name | Network | Description |
|----------|----------|----------|----------|
| 10    | Sales    | 192.168.10.0/24     | Sales Department     |
| 20    | IT     | 192.168.20.0/24      | IT Department     |
| 30    | HR     | 192.168.30.0/24      | HR Department     |
| 99    | Managment     | 192.168.99.0/24      | Native VLAN     |
#


## Device Configuration

### Switch Initial Setup
<img width="531" height="188" alt="Screenshot 2025-08-08 at 6 07 50 PM" src="https://github.com/user-attachments/assets/d98bfe40-e115-4a83-93f0-f0f3b14c9093" />

### Router Initial Setup
<img width="525" height="170" alt="Screenshot 2025-08-08 at 6 10 50 PM" src="https://github.com/user-attachments/assets/7f5011b5-529d-4c06-8515-248443bbef15" />







#
### Learning Outcomes
- VLAN creation and management
- Switch port assignment and configuration
- 802.1Q trunking protocols
- Router-on-a-stick implementation
- Subinterface configuration and encapsulation
- Inter-VLAN routing concepts
- Access Control List implementation
