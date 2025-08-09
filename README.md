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

#### Verifying VLAN Configurations
<img width="760" height="328" alt="Screenshot 2025-08-09 at 11 51 22 AM" src="https://github.com/user-attachments/assets/90fa9db3-04ac-4b4b-b36a-cc321b1e134e" />

`interface f0/2 - 3 is set to VLAN 10 (Sales)`

`interface f0/4 - 5 is set to VLAN 20 (IT)`

`interface f0/6 - 7 is set to VLAN 30 (HR)`
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
### Configuring End Devices
#### PC IP Configuration
Sales PC1: IP: 192.168.10.10, Gateway: 192.168.10.1

Sales PC2: IP: 192.168.10.11, Gateway: 192.168.10.1

IT PC1: IP: 192.168.20.10, Gateway: 192.168.20.1

IT PC2: IP: 192.168.20.11, Gateway: 192.168.20.1

HR PC1: IP: 192.168.30.10, Gateway: 192.168.30.1

HR PC2: IP: 192.168.30.11, Gateway: 192.168.30.1

#

### Applying Access Control List

#### Real-World Scenario:
In this lab, I created 3 departments:

Sales: handles customer data

HR: manages payroll, personal info

IT: supports all departments, needs access everywhere

#### ACL Requirements
Sales ↔ IT: IT needs to support Sales systems

HR ↔ IT: IT needs to support HR systems

Sales ↔ HR: Sales shouldn't see payroll data, and HR shouldn't see customer deals        


#### Deny any IP traffic from HR network (192.168.30.0/24) to Sales network (192.168.10.0/24)
`R1(config)# access-list 100 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255`

#### Deny any IP traffic from Sales network (192.168.10.0/24) to HR network (192.168.30.0/24)
`access-list 100 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255`

#### Allow all other IP traffic (required because ACLs have implicit deny-all at end)
`access-list 100 permit ip any any`

#### Apply ACL to Sales VLAN subinterface (inbound direction)
`R1(config)# interface gi0/0.10`

`R1(config-subif)# ip access-group 100 in`

#### Apply ACL to HR VLAN subinterface (inbound direction)
`R1(config)# interface gi0/0.30`

`R1(config-subif)# ip access-group 100 in`

<img width="700" height="239" alt="Screenshot 2025-08-09 at 12 31 00 PM" src="https://github.com/user-attachments/assets/3bb66cd5-c3ac-434d-942a-4f74750ca416" />
#

# Testing and Validation
#### Test 1: Intra-VLAN Communication

#### Sales PC1 → Sales PC2: ✅ Should work
<img width="1253" height="472" alt="Screenshot 2025-08-09 at 12 51 33 PM" src="https://github.com/user-attachments/assets/2e9165a8-688b-467a-9714-1df1ee87caf8" />



#### IT PC3 → IT PC4: ✅ Should work  
<img width="1242" height="441" alt="Screenshot 2025-08-09 at 12 59 58 PM" src="https://github.com/user-attachments/assets/7ce7f3b1-df4d-47f6-93de-f7091a9c8969" />


#### HR PC5 → HR PC6: ✅ Should work
<img width="1252" height="442" alt="Screenshot 2025-08-09 at 1 02 22 PM" src="https://github.com/user-attachments/assets/8fb315cf-9618-478f-9ce7-6fdb5a6a80d4" />


#### Test 2: Inter-VLAN Communication

#### Sales PC1 → IT PC3: ✅ Should work
<img width="1254" height="465" alt="Screenshot 2025-08-09 at 1 06 30 PM" src="https://github.com/user-attachments/assets/f50e3879-ac78-4c99-b349-a57592811da9" />

#### IT PC1 → HR PC5: ✅ Should work
<img width="1240" height="447" alt="Screenshot 2025-08-09 at 1 12 59 PM" src="https://github.com/user-attachments/assets/77345c6e-fcf3-4aa1-a11c-cdb544c0c289" />

#### Sales PC1 → HR PC5: ❌ Should be blocked by ACL
<img width="1238" height="462" alt="Screenshot 2025-08-09 at 1 14 44 PM" src="https://github.com/user-attachments/assets/eb7995e8-764c-4d63-a33a-08a820723c68" />

#

### Troubleshooting 

#### Common Issues
| Issue | Symptoms | Solution |
|----------|----------|----------|
| No inter-VLAN communication    | PC's can't reach other VLANs    | Check trunk configuration and subinterfaces    |
| VLAN assignment    | PC can't reach gateway    | Verify port VLAN assignment    |
| Trunk not passing VLANs    | show interfaces trunk shows no VLANs    | Check allowed VLAN list    |
#


### Learning Outcomes
- VLAN creation and management
- Switch port assignment and configuration
- 802.1Q trunking protocols
- Router-on-a-stick implementation
- Subinterface configuration and encapsulation
- Inter-VLAN routing concepts
- Access Control List implementation
