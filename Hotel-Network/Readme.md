
This project is about designing and implementing modern network to a hotel with three floors.
## Project Overview

![lab3](https://github.com/user-attachments/assets/a8e1fb52-e1a9-4987-b3bb-ba5af9709328)

## Key Requirements


#### **1. Router Setup:**
- Three routers should be used to connect each floor (placed in the IT server room).
- All routers should be connected using serial DCE cables.
- The network between the routers should be:
  - `10.10.10.0/30`
  - `10.10.10.4/30`
  - `10.10.10.8/30`

#### **2. Switch and Access Points:**
- Each floor must have one switch (placed on the respective floor).
- Each floor is expected to have **WiFi networks** connected to laptops and phones.

#### **3. VLAN Setup:**
- Each department should be in a different VLAN:
  
  **1st Floor:**
  - Reception: **VLAN 80**, Network: `192.168.8.0/24`
  - Store: **VLAN 70**, Network: `192.168.7.0/24`
  - Logistics: **VLAN 60**, Network: `192.168.6.0/24`
  
  **2nd Floor:**
  - Finance: **VLAN 50**, Network: `192.168.5.0/24`
  - HR: **VLAN 40**, Network: `192.168.4.0/24`
  - Sales: **VLAN 30**, Network: `192.168.3.0/24`
  
  **3rd Floor:**
  - Admin: **VLAN 20**, Network: `192.168.2.0/24`
  - IT: **VLAN 10**, Network: `192.168.1.0/24`

##### **4. OSPF Routing:**
- Use **OSPF** as the routing protocol to advertise routes across the network.

##### **5. DHCP Configuration:**
- All devices in the network should obtain their IP addresses dynamically from their respective router, configured as the **DHCP server**.

##### **6. Device Communication:**
- All devices on the network should be able to **communicate with each other** across VLANs.

##### **7. SSH Configuration:**
- **SSH** should be configured on all routers for remote login.

##### **8. IT Department Security:**
- Add **Test-PC** in the IT department, assign it to interface `fa0/1` for remote login testing.
- Configure **port security** in the IT department to allow only **Test-PC** to access `fa0/1`:
  - Use **sticky** method to obtain the MAC address.
  - Configure **violation mode** to be **shutdown**.

---

# 1st Floor Router


```shell Router>

Router>en
Router#conf t
Router(config)#int se0/3/0
Router(config-if)#no sh
Router(config-if)#int se0/3/1
Router(config-if)#no sh

Router(config-if)#int gig0/0

Router(config-if)#no sh

Router(config-if)#int se0/3/1

Router(config-if)#clock rate 64000
Router(config-if)#do wr

```




# 3rd Floor Router


```shell Router>

Router>en
Router#conf t
Router(config)#int se0/3/0
Router(config-if)#no sh
Router(config-if)#int se0/3/1
Router(config-if)#no sh

Router(config-if)#int gig0/0

Router(config-if)#no sh

Router(config-if)#int se0/3/0

Router(config-if)#clock rate 64000
Router(config-if)#do wr

```



# 1. 1st Floor 

| /            | Store              | Logistics           | Reception          |
| ------------ | ------------------ | ------------------- | ------------------ |
| VLAN         | VLAN 60            | VLAN 70             | VLAN 80            |
| Network      | 192.168.6.0/24     | 192.168.7.0/24      | 192.168.8.0/24     |
| Switch Ports | FastEthernet 0/2-4 | FastEthernet 0/9-12 | FastEthernet 0/6-8 |

### Switch Configuration



```shell Switch>
 
Switch>en

Switch#conf t

# Creating 1st Floor VLANs

Switch(config)#vlan 70

Switch(config-vlan)#name stote-vlan

Switch(config-vlan)#vlan 80

Switch(config-vlan)#name reception-vlan

Switch(config-vlan)#vlan 60

Switch(config-vlan)#name logistics-vlan

Switch(config-vlan)#exit 

# Assigning VLAN 80 (Reception Vlan) for Fastethernet ports 6 - 8

Switch(config)#int range fa0/6-8

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 80
Switch(config-if-range)#exit


# Assigning VLAN 60 (Logistics Vlan) for Fastethernet ports 9 - 12

Switch(config)#

Switch(config)#int range fa0/9-12

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 60

Switch(config-if-range)#exit


# Assigning VLAN 70 (Store Vlan) for Fastethernet ports 9 - 12

Switch(config)#int range fa0/2-4

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 70

Switch(config-if-range)#exit

# Changing the mode of fa0/1 to trunk


Switch(config)#int f0/1

Switch(config-if)#switchport mode trunk

# allow all the first floor vlans to connect with each other

Switch(config-if)#switchport trunk allow vlan 60,70,80

Switch(config-if)# exit


```

#### Overview


![1st-floor-poc](https://github.com/user-attachments/assets/0089fc64-3e34-4e2c-b511-954b0c3304b2)


-----

# 2. 2nd Floor 



| /            | Sales              | HR                  | Finance            |
| ------------ | ------------------ | ------------------- | ------------------ |
| VLAN         | VLAN 30            | VLAN 40             | VLAN 50            |
| Network      | 192.168.3.0/24     | 192.168.4.0/24      | 192.168.5.0/24     |
| Switch Ports | FastEthernet 0/2-4 | FastEthernet 0/5-7 | FastEthernet 0/8-11 |

### Switch Configuration



```shell Switch>

Switch>en

Switch#conf t

# Creating the 2nd floor VLANs 

Switch(config)#vlan 30

Switch(config-vlan)#name sales-vlan

Switch(config-vlan)#vlan 40

Switch(config-vlan)#name hr-vlan

Switch(config-vlan)#vlan 50

Switch(config-vlan)#name finance-vlan

Switch(config-vlan)#exit 


# Assigning VLAN 30 (Sales Vlan) for Fastethernet ports 2 - 4

Switch(config)#int range fa0/2-4

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 30

Switch(config-if-range)#exit



# Assigning VLAN 40 (HR Vlan) for Fastethernet ports 5 - 7


Switch(config)#int range fa0/5-7

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 40

Switch(config-if-range)#exit


# Assigning VLAN 50 (Finance Vlan) for Fastethernet ports 8 - 11


Switch(config)#int range fa0/8-11

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 50


Switch(config-if-range)#exit


# Changing the mode of fa0/1 to trunk

Switch(config)#int f0/1

Switch(config-if)#switchport mode trunk

# allow all the Second floor vlans to connect with each other

Switch(config-if)#switchport trunk allow vlan 30,40,50

Switch(config-if)# exit


```

#### Overview

![2nd-floor-poc](https://github.com/user-attachments/assets/938aeaaf-a7d4-4151-bad3-74ee4783471f)


----

# 3. 3rd Floor



| /            | IT                 | Admin               |
| ------------ | ------------------ | ------------------- |
| VLAN         | VLAN 10            | VLAN 20             |
| Network      | 192.168.1.0/24     | 192.168.2.0/24      |
| Switch Ports | FastEthernet 0/3-6 | FastEthernet 0/8-10 |

### Switch Configuration



```shell Switch>

Switch>en

Switch#conf t

# Creating the 3rd floor VLANs 

Switch(config)#vlan 10

Switch(config-vlan)#name it-vlan

Switch(config-vlan)#vlan 20

Switch(config-vlan)#name admin-vlan

Switch(config-vlan)#exit


# Assigning VLAN 10 (IT Vlan) for Fastethernet ports 3 - 6

Switch(config)#int range fa0/3-6

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 10

Switch(config-if-range)#exit



# Assigning VLAN 20 (Admin Vlan) for Fastethernet ports 7 - 10


Switch(config)#int range fa0/7-10

Switch(config-if-range)#switchport mode access

Switch(config-if-range)#switchport access vlan 20

Switch(config-if-range)#exit


# Changing the mode of fa0/1 to trunk

Switch(config)#int f0/1

Switch(config-if)#switchport mode trunk

# allow all the Third floor vlans to connect with each other

Switch(config-if)#switchport trunk allow vlan 10,20

Switch(config-if)# exit


```


#### Overview



![3rd-floor-poc](https://github.com/user-attachments/assets/374c8b57-90e0-48a6-932b-37884f244384)




# *To Be Continued*

