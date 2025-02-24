Huawei Switch Configartion all Commend 




1. Basic Configuration
system-view                                              # Enter configuration mode
sysname Switch1                                     # Set hostname
interface GigabitEthernet0/0/1              # Enter interface mode
ip address 192.168.1.1 255.255.255.0  # Set IP address
description Uplink                              # Add description
undo shutdown                                # Enable interface
quit                                                # Exit interface mode
save                                             # Save configuration

2. VLAN Configuration
system-view
vlan 10
name Sales                                  # Name the VLAN
quit

interface GigabitEthernet0/0/1
port link-type access                 # Set port as access
port default vlan 10                   # Assign VLAN 10 to port
quit

Trunk Port
interface GigabitEthernet0/0/2
port link-type trunk
port trunk allow-pass vlan 10 20 30       # Allow VLANs
quit


3. Spanning Tree (STP)
system-view
stp enable                             # Enable STP
stp mode mstp                    # Set mode (MSTP, RSTP, or STP)
stp instance 1 vlan 10 20    # Assign VLANs to MSTP
quit

4. LACP (Link Aggregation)
system-view
interface Eth-Trunk 1
mode lacp-static                        # Enable LACP
quit
interface GigabitEthernet0/0/1
eth-trunk 1                              # Assign port to trunk
quit

interface GigabitEthernet0/0/2
eth-trunk 1
quit

5. Routing Configuration
Static Route
ip route-static 0.0.0.0 0.0.0.0 192.168.1.254
Enable OSPF
system-view
ospf 1 router-id 1.1.1.1
area 0
network 192.168.1.0 0.0.0.255
quit

6. DHCP Server
system-view
dhcp enable
ip pool VLAN10_POOL
network 192.168.10.0 mask 255.255.255.0
gateway-list 192.168.10.1
dns-list 8.8.8.8
quit

interface Vlanif10
ip address 192.168.10.1 255.255.255.0
dhcp select global
quit

7. Security & Access Control
Set Password & Enable AAA
system-view
aaa
local-user admin password cipher Admin@123
local-user admin privilege level 15
quit

SSH Configuration
system-view
stelnet server enable
rsa local-key-pair create
user-interface vty 0 4
authentication-mode aaa
protocol inbound ssh
quit

8. Save & Backup Configuration
save                                                                                # Save configuration
display current-configuration                                        # Show running config
backup configuration to tftp 192.168.1.100 huawei.cfg
