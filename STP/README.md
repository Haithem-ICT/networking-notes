# 📁 Spanning Tree Protocol (STP)

### 📄 Definition
A Layer 2 network protocol designed to prevent accidental switching loops in redundant network topologies by dynamically blocking specific redundant ports.



### 🎯 Why It's Used
In modern enterprise campus setups, we connect switches together with multiple redundant backup links so that if one cable breaks, the network stays alive. However, because Layer 2 ethernet frames do not have a TTL (Time-to-Live) counter, data packets would circulate infinitely around redundant lines, causing a catastrophic **broadcast storm** that crashes the entire network. STP runs an algorithm to find loops, elect a central "Root Bridge" switch, and safely place redundant ports into a standby "Blocking" state.

### ⚙️ Basic Verification Commands (Cisco IOS)
To check which switch is the leader (Root Bridge) and see which physical ports are forwarding or blocking data traffic:
```text
Switch> enable
Switch# show spanning-tree
Switch# show spanning-tree vlan 10
```
⚠️ Common Mistakes & Real-World Troubleshooting
Native VLAN Mismatch Errors: If two linked switches don't agree on which VLAN is the native untagged lane, the terminal will instantly loop an error message:
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/24 (1), with Switch FastEthernet0/24 (99).
When this happens, STP will temporarily put the port into a broken state to protect the network from unexpected traffic leaks.

Fixing Port Range Overlaps: Accidentally configuring native trunk commands across incorrect ranges can cause STP to drop link states. Verifying trunk uniformity with show interface trunk fixes this.

🔍 Small Example
During our campus configuration lab, a misconfigured trunk interface range caused an asymmetric native VLAN setup. The Cisco IOS terminal instantly generated a NATIVE_VLAN_MISMATCH alert. We isolated the problem port, explicitly realigned the port assignments to match on both sides of the backbone, and STP immediately returned the link state to healthy forwarding mode.
