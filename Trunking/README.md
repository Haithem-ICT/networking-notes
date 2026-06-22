# 📁 VLAN Trunking (802.1Q)

### 📄 Definition
A port configuration that allows a single physical link between network switches (or a switch and a router) to carry traffic for multiple VLANs simultaneously.

### 🎯 Why It's Used
Without trunking, you would need to run a separate physical cable for *every single VLAN* between your switches. Trunking acts like a multi-lane highway. It achieves this by adding a small digital **802.1Q tag** to the frame header to identify which VLAN the data belongs to so the switches don't mix up the traffic.

### ⚙️ Basic Configuration (Cisco IOS)
To configure an uplink port between switches to allow all VLAN traffic to pass through:
```text
Switch> enable
Switch# configure terminal
Switch(config)# interface FastEthernet0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# end
Switch# write memory
```
⚠️ Common Mistakes
Native VLAN Mismatch: If Switch A expects untagged management traffic on VLAN 1, but Switch B expects it on VLAN 99, Cisco switches will throw a %CDP-4-NATIVE_VLAN_MISMATCH console error. Both sides of a trunk link must match!

Access Mode Mistake: Accidentally setting an inter-switch uplink connection to mode access. If you do this, only one single VLAN can cross between the switches, blocking all other departments.

🔍 Small Example
In our enterprise project, the connections linking the Admin Switch to the Core Switch, and the IT Switch to the Core Switch, were explicitly set to switchport mode trunk. This allowed the Admin traffic (VLAN 10) and IT traffic (VLAN 20) to share a single backbone line up to the central distribution hub.
