# standard-acl
Layer 3 Enterprise Network featuring a Router-on-a-Stick (ROAS) access edge (Switch1 and Router R1), an intermediate WAN transit cloud, a core router (R2), and an internal enterprise server farm (Switch0) alongside an external public web/DNS server (WEB-SRV at 8.8.8.8).

🌐 Quick Overview & Network Specs
🟢 Architecture & Topology: Layer 3 Enterprise Network featuring a Router-on-a-Stick (ROAS) access edge (Switch1 and Router R1), an intermediate WAN transit cloud, a core router (R2), and an internal enterprise server farm (Switch0) alongside an external public web/DNS server (WEB-SRV at 8.8.8.8).   
PNG

🔵 VLAN & Subnet Allocations:

VLAN 10 (IT Department): Host PC0 (192.168.10.2/24) and PC1 (192.168.10.3/24).   
PNG

VLAN 20 (HR Department): Host PC2 (192.168.20.2/24).   
PNG

Server Farm Segment: IT-SRV (10.0.0.3) and HR-SRV (10.0.0.2) connected via Switch0 behind Router R2.   
PNG

External Web/DNS Services: WEB-SRV (8.8.8.8) connected upstream.   
PNG

🟣 Core Security Mechanism (Standard ACLs): Standard Access Control Lists (numbered 1–99 or named) filtering IP packet flows based strictly on source IP addresses without inspecting destination ports, protocols, or upper-layer header data.

💡 Key Technical Insights & What This Lab Demonstrates
🔍 Source-Only IP Filtering: Demonstrates how Standard ACLs evaluate incoming or outgoing packets by inspecting only the source network or host IP address, making them ideal for coarse perimeter control.   
PNG

⚡ Strategic Placement Rule: Highlights the critical design principle that Standard ACLs should be placed as close to the destination as possible. Placing a standard ACL too close to the source can unintentionally block traffic to multiple destinations across the network.

🛠️ Implicit Deny Behavior: Illustrates the mandatory Cisco rule where every ACL concludes with an invisible deny any statement, requiring explicit permit statements for all authorized traffic flows.

🛡️ Departmental Isolation & Access Control: Restricts unauthorized departmental subnets (e.g., preventing HR subnets from accessing IT administrative servers) while maintaining permitted enterprise routing paths.   
PNG

🛠️ Essential Cisco CLI Configuration Snippets
Configuring a Standard ACL on Router R2
Cisco CLI
! Create Standard ACL 10 to permit only IT subnet access to IT Server
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any

! Apply Standard ACL outbound on the interface facing the server farm
interface GigabitEthernet0/1
 ip access-group 10 out
🧪 Verification & Audit Commands
Bash
# Verify ACL configuration, rule order, and active match hit counters
show ip access-lists 10

# Inspect interface bindings and direction (inbound vs. outbound) for applied ACLs
show ip interface gigabitEthernet 0/1

# Test policy enforcement via end-to-end ping verification from host PCs
ping 10.0.0.3
