Here’s an updated `README.md` file with the license section updated to reflect "NinjaCat811":

---

# Small Office Network Design

This project provides a detailed design and implementation of a small office network for XYZ Company in Eastern Australia. The network is designed using Cisco hardware and features VLAN segmentation to support multiple departments.

## Project Overview

Lab :
![Q2](https://github.com/user-attachments/assets/cbb1ec9c-a3ed-41db-b35a-164993bcbffe)

### Key Requirements
- **Router and Switch:** Cisco products are used for the network infrastructure.
- **Departments and VLANs:**
  - **Admin/IT:** VLAN 20, IP Range: 192.168.1.64/26
  - **Finance/HR:** VLAN 10, IP Range: 192.168.1.0/24
  - **Customer Service/Reception:** VLAN 30, IP Range: 192.168.1.128/26
- **Wireless Access:** Each department has its own wireless access point.
- **Automatic IP Assignment:** DHCP is configured to allocate IPv4 addresses automatically.
- **Inter-Department Communication:** Devices across all VLANs can communicate.

## Network Diagram

The network diagram includes:
- A **Cisco 2811 Router** connected to each department’s network through a switch.
- **Access Points** for wireless connectivity in each department.
- Devices like **PCs, Laptops, Printers, and Smartphones** for each department.


## IP Addressing Plan

| Department                | VLAN | IP Range            | Wireless Access Point |  
|---------------------------|------|---------------------|-----------------------|
| Admin/IT                   | 20   | 192.168.1.64/26     | Yes                   |
| Finance/HR                 | 10   | 192.168.1.0/24      | Yes                   |
| Customer Service/Reception | 30   | 192.168.1.128/26    | Yes                   |

## Configuration

### Router Configuration

This configuration handles routing between VLANs, DHCP setup, and inter-departmental communication.

```bash
# Router Sub-interfaces for VLANs

Router> enable
Router# configure terminal

# Sub-interface for VLAN 10 (Finance/HR)
Router(config)# interface FastEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.1.1 255.255.255.0
Router(config-subif)# exit

# Sub-interface for VLAN 20 (Admin/IT)
Router(config)# interface FastEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.1.65 255.255.255.192
Router(config-subif)# exit

# Sub-interface for VLAN 30 (Customer Service)
Router(config)# interface FastEthernet0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.1.129 255.255.255.192
Router(config-subif)# exit

# Enable DHCP on each VLAN
Router(config)# ip dhcp pool hr-pool
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 192.168.1.1
Router(dhcp-config)# domain-name finance.com
Router(dhcp-config)# exit

Router(config)# ip dhcp pool it-pool
Router(dhcp-config)# network 192.168.1.64 255.255.255.192
Router(dhcp-config)# default-router 192.168.1.65
Router(dhcp-config)# dns-server 192.168.1.65
Router(dhcp-config)# domain-name it.com

Router(dhcp-config)# exit

Router(config)# ip dhcp pool cs-pool
Router(dhcp-config)# network 192.168.1.128 255.255.255.192
Router(dhcp-config)# default-router 192.168.1.129
Router(dhcp-config)# dns-server 192.168.1.129
Router(dhcp-config)# domain-name cs.com
Router(dhcp-config)# exit

# Save configuration
Router(config)# do wr
```

### Switch Configuration

This will configure VLANs and assign ports for devices in different departments.

```bash
# VLANs Configuration on the Switch

Switch> enable
Switch# configure terminal

# Create VLAN 10 (Finance/HR)
Switch(config)# VLAN10
Switch(config-vlan)# name Finance_HR
Switch(config-vlan)# exit

# Create VLAN 20 (Admin/IT)
Switch(config)# vlan 20
Switch(config-vlan)# name VLAN20
Switch(config-vlan)# exit

# Create VLAN 30 (Customer Service/Reception)
Switch(config)# vlan 30
Switch(config-vlan)# name VLAN30
Switch(config-vlan)# exit

# Assign ports to VLANs
# Finance/HR (VLAN 10)
Switch(config)# interface range FastEthernet0/2 - 5
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

# Admin/IT (VLAN 20)
Switch(config)# interface range FastEthernet0/11 - 16
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

# Customer Service/Reception (VLAN 30)
Switch(config)# interface range FastEthernet0/6 - 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit

# Configure trunking between switch and router
Switch(config)# interface FastEthernet0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# exit

# Save configuration
Switch(config)# exit

```

## Final Network

![Small-Office-Network](https://github.com/user-attachments/assets/9bf8cc7f-5b83-4a0e-954a-66ac4d20e128)



## License

This project is licensed under the MIT License. This project was developed by **NinjaCat811**.

