# 📁 Virtual Local Area Networks (VLANs)

### 📄 Definition
A logical segmentation of a physical switch into distinct, isolated Layer 2 broadcast domains.

### 🎯 Why It's Used
* **Security:** Isolates sensitive departments (e.g., Administration vs. IT) so they cannot sniff each other's traffic.
* **Performance:** Reduces the size of broadcast domains, preventing network slowdowns from corporate broadcast storms.

### ⚙️ Basic Configuration (Cisco IOS)
```text
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name Administration
Switch(config-vlan)# exit
