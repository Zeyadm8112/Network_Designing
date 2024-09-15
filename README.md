# Simple Network Project: Accounts and Delivery Departments

This project simulates a simple network in **Cisco Packet Tracer** to connect two departments—**Accounts** and **Delivery**—using routers and switches. The setup follows the provided instructions to ensure that all PCs can communicate with each other, especially between departments.

LAB :

![Q1](https://github.com/user-attachments/assets/cbeea121-1618-4526-a9eb-8934ccca57af)

---

## Network Overview

This network is designed with the following structure:
- **Router**: 2911
- **Switches**: One switch for each department
  - **Accounts Department**: 2960-24TT (with 2 PCs and 1 Printer)
  - **Delivery Department**: 2960-24TT (with 2 PCs and 1 Printer)
- **IP Addresses**:
  - **Accounts Department**: 198.162.40.0/25
  - **Delivery Department**: 198.162.40.128/25
- **Objective**: PCs in the Delivery department should be able to ping the PCs in the Accounts department.

---

## Configuration

### 1. **Router Configuration**
The router connects the two departments and acts as the gateway for each subnet:
- **Gig0/0**: Configured with IP address `198.162.40.1` (Subnet for the Accounts department).
- **Gig0/1**: Configured with IP address `198.162.40.129` (Subnet for the Delivery department).
  
Commands for router configuration:
```bash
Router> enable
Router# configure terminal

# Interface GigabitEthernet 0/0 (for Accounts)
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 198.162.40.1 255.255.255.128
Router(config-if)# no shutdown

# Interface GigabitEthernet 0/1 (for Delivery)
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip address 198.162.40.129 255.255.255.128
Router(config-if)# no shutdown

```
### 2. **PC and Printer Configuration**

Each PC and printer in both departments were configured with static IP addresses, following the subnet scheme provided:
- Accounts Department (198.162.40.0/25):
  - PC0: 198.162.40.2, default gateway: 198.162.40.1
  - PC1: 198.162.40.3, default gateway: 198.162.40.1
  - Printer0: 198.162.40.4
- Delivery Department (198.162.40.128/25):
  - PC2: 198.162.40.130, default gateway: 198.162.40.129
  - PC3: 198.162.40.131, default gateway: 198.162.40.129
  - Printer2: 198.162.40.132
 

Resualt:


![Network](https://github.com/user-attachments/assets/160134f5-d6be-4788-a0fd-0c551a87b9e0)


### 3. **Testing**
After configuring the devices, the connectivity was tested by pinging between the PCs in both departments. PCs in the Delivery department were able to successfully ping PCs in the Accounts department, confirming that routing was working correctly.

Screenshots
Network Design in Packet Tracer

Project Requirements

Conclusion
This project demonstrates a basic network configuration using a router and two switches to connect different departments. The two departments are assigned different subnets and can communicate across the router. All objectives were met as outlined in the task.




