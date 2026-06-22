# 📁 Virtual Local Area Networks (VLANs)

### 📄 Definition
A logical segmentation of a physical switch into distinct, isolated Layer 2 broadcast domains. 

### 🎯 Why It's Used
* **Security:** Isolates sensitive departments (e.g., Administration vs. IT) at the hardware layer so they cannot capture or sniff each other's traffic.
* **Performance:** Reduces the size of broadcast domains. Instead of a broadcast packet hitting every single computer in the building, it stays trapped inside its designated department, preventing network slowdowns.

### ⚙️ Basic Configuration (Cisco IOS)
To create a VLAN and give it a descriptive name on a Cisco switch:
```text
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name Administration
Switch(config-vlan)# exit
```
To assign a physical switch interface to a specific VLAN:
```text
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit
```
To assign a large range of ports at the same time:
```text
Switch(config)# interface range fastEthernet0/1 - 5
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```
⚠️ Common Mistakes
Port Range Gaps: Accidentally leaving important hardware (like your shared printer or a forgotten laptop) out of your interface range command. If a port isn't explicitly assigned, it stays trapped in default VLAN 1 and cannot talk to the rest of the department.

Access vs. Trunk Confusion: Setting an uplink port (a line connecting two switches together) to mode access. If you do this, only one VLAN can cross the wire instead of all of them.

🔍 Small Example
In a campus network simulation, Admin PCs use ports Fa0/1 - 5 assigned to VLAN 10, while IT PCs use ports Fa0/2 - 5 assigned to VLAN 20. Because they are isolated at Layer 2, they cannot communicate with each other until a Layer 3 routing device moves the packets across the boundaries.
