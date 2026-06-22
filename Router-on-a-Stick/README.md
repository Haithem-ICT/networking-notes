# 📁 Router-on-a-Stick (Inter-VLAN Routing)

### 📄 Definition
A deployment method where a single physical interface on a router routes traffic between multiple VLANs by dividing that interface into virtual "subinterfaces."

### 🎯 Why It's Used
By default, Layer 2 VLANs are completely isolated. To allow communication between them, you need a Layer 3 router. Instead of wasting multiple expensive physical router ports and running a separate cable for every single VLAN, Router-on-a-Stick lets you use **just one single physical cable** connecting your switch up to the router. 

### ⚙️ Basic Configuration (Cisco IOS)
First, you must ensure the main physical interface on the router is turned on but does **not** have an IP address directly assigned to it:
```text
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# no shutdown
Router(config-if)# exit
```
Next, you create the virtual subinterfaces for each VLAN, configure the 802.1Q encapsulation, and assign the Default Gateway IPs:
```text
Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# end
Router# write memory
```
⚠️ Common Mistakes
Forgetting Encapsulation: Trying to type the ip address command on a subinterface before typing encapsulation dot1Q [VLAN_ID]. Cisco IOS will throw an error and block the IP because it doesn't know which VLAN tag to expect yet.

Main Interface Shutdown: Configuring all subinterfaces perfectly but forgetting to run no shutdown on the main physical interface (Gig0/0). If the main interface is down, all virtual subinterfaces remain completely inactive.

🔍 Small Example
In our corporate simulation, the Core Switch was connected to the Router's Gig0/0 port via a trunk link. We split Gig0/0 into Gig0/0.10 (Gateway for Admin PCs) and Gig0/0.20 (Gateway for IT PCs). When Admin PC1 pinged IT PC1, the packet traveled up this single link, was routed between subinterfaces internally, and traveled right back down to its destination.
