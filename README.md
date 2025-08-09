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

Basic security and device identification are essential before implementing advanced features. Hostnames help distinguish devices in topologies.
### Switch Initial Setup
<img width="531" height="188" alt="Screenshot 2025-08-08 at 6 07 50 PM" src="https://github.com/user-attachments/assets/d98bfe40-e115-4a83-93f0-f0f3b14c9093" />

### Router Initial Setup
<img width="525" height="170" alt="Screenshot 2025-08-08 at 6 10 50 PM" src="https://github.com/user-attachments/assets/7f5011b5-529d-4c06-8515-248443bbef15" />


## Creating and Configuring VLANS

### VLAN Configurations
VLANs create separate broadcast domains on a single physical switch. This provides:
- Security: Departments can't see each other's traffic by default.
- Performance: Reduced broadcast traffic in each segment.
- Management: Easier troubleshooting and policy application.

#### Command Breakdown:
`vlan 10`: Creates VLAN 10 in the switch's VLAN database and enters VLAN configuration mode.

`name Sales`: Assigns a descriptive name to VLAN 10

`exit`: Exits VLAN configuration mode and returns to global configuration mode
  
<img width="506" height="451" alt="Screenshot 2025-08-08 at 6 37 41 PM" src="https://github.com/user-attachments/assets/a11a218a-9437-4dac-9c1f-eff143b71e7f" />
  
#
### Assigning Switch Ports to VLANs
Access ports connect end devices (PCs) to their designated VLAN. Traffic from these devices remains untagged - the switch handles VLAN tagging internally. Without proper VLAN assignment, devices would all be in VLAN 1 (default).

#### Command Breakdown:
`interface range f0/2 - 3`: Enters configuration mode for ports fa0/2 and fa0/3 simultaneously

`switchport mode access`: Configures port as an access port (connects to end devices, single VLAN)

`switchport access vlan 10`: Assigns the access port to VLAN 10


<img width="463" height="270" alt="Screenshot 2025-08-08 at 6 43 21 PM" src="https://github.com/user-attachments/assets/11072914-09c5-46bf-b998-2d62a545b64d" />

#
### Configuring Trunk Port to Router
Trunk ports are essential for router-on-a-stick. They:
- Transport multiple VLANs over a single physical link
- Add 802.1Q tags to identify which VLAN each frame belongs to
- Use native VLAN for management traffic (security best practice: not VLAN 1)
- Control VLAN propagation by specifying allowed VLANs

#### Command Breakdown:
`interface g0/1`: Enters interface configuration mode for the GigabitEthernet port

`switchport mode trunk`: Enables 802.1Q trunking (allows multiple VLANs on this port)

`switchport trunk native vlan 99`: Sets VLAN 99 as the native VLAN (frames sent untagged)

`switchport trunk allowed vlan 10, 20, 30, 99`: Restricts which VLANs can traverse this trunk

<img width="599" height="125" alt="Screenshot 2025-08-08 at 6 49 39 PM" src="https://github.com/user-attachments/assets/6654d151-5821-45d7-bbd9-00b362ef4a64" />

#
### Router Physical Interface Activation
The physical interface must be active before any subinterfaces can function. This single interface will handle traffic for all VLANs through subinterfaces.

#### Command Breakdown:
`interface g0/1`: Enters interface configuration mode for the physical GigabitEthernet port

`no shutdown`: Removes the default administrative state, bringing the interface up

<img width="719" height="275" alt="Screenshot 2025-08-08 at 6 56 43 PM" src="https://github.com/user-attachments/assets/a0825b64-11e5-4ece-993e-51d7261460c4" />



#
### Subinterface Configuration (Router-on-a-Stick)
Creates logical router interfaces for each VLAN, enabling inter-VLAN routing.

Subinterfaces enable the router to:
- Route between VLANs (inter-VLAN routing)
- Serve as the default gateway for each department
- Process tagged frames from specific VLANs
- Maintain separate routing tables per VLAN

#### Command Breakdown:
`interface g0/0.10`: Creates a logical subinterface numbered .10 (to match VLAN number)

`encapsulation dot1Q 10`: Tells router to process frames with 802.1Q tag value of 10

`ip address 192.168.10.1 255.255.255.0`: Assigns an IP address that serves as the default gateway for VLAN 10

`encapsulation dot1Q 99 native`: Processes untagged frames and assigns them to VLAN 99

<img width="787" height="707" alt="Screenshot 2025-08-08 at 7 13 23 PM" src="https://github.com/user-attachments/assets/6ad65f5e-99b6-4a79-9069-f5cb7b49eb50" />


#
### Learning Outcomes
- VLAN creation and management
- Switch port assignment and configuration
- 802.1Q trunking protocols
- Router-on-a-stick implementation
- Subinterface configuration and encapsulation
- Inter-VLAN routing concepts
- Access Control List implementation
