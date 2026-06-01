# Twiga-Foods-VLAN-Subnetting
Mapped physical edge access interfaces directly into their respective VLAN boundaries:
TWIGA-CORE(config)# interface FastEthernet0/1
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 10

TWIGA-CORE(config)# interface FastEthernet0/2
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 20

TWIGA-CORE(config)# interface FastEthernet0/3
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 30

TWIGA-CORE(config)# interface FastEthernet0/4
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 40

TWIGA-CORE(config)# interface FastEthernet0/5
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 50

Created virtual internal interfaces on the Multilayer Switch to allow line-rate routing across the newly provisioned VLAN segments:
TWIGA-CORE(config)# interface vlan 10
TWIGA-CORE(config-if)# ip address 10.10.0.1 255.255.255.192

TWIGA-CORE(config)# interface vlan 20
TWIGA-CORE(config-if)# ip address 10.10.0.65 255.255.255.224

TWIGA-CORE(config)# interface vlan 30
TWIGA-CORE(config-if)# ip address 10.10.0.97 255.255.255.240

TWIGA-CORE(config)# interface vlan 40
TWIGA-CORE(config-if)# ip address 10.10.0.113 255.255.255.240

TWIGA-CORE(config)# interface vlan 50
TWIGA-CORE(config-if)# ip address 10.10.0.129 255.255.255.248

Enabled the Layer 3 routing engine capability on the 3560 switch platform to safely permit traffic to bridge subnets internally without relying on an external firewall or Router-on-a-Stick setup:
TWIGA-CORE(config)# ip routing

# VERIFICATION AND TROUBLESHOOTING 
Attached on the images 


