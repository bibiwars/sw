# basic config
	Switch(config)#hostname S1
	S1(config)#enable secret cisco
	S1(config)#no ip domain-lookup
	S1(config)#line console 0
	S1(config-line)#password cisco
	S1(config-line)#login
	S1(config)#banner motd &AccesInterdit&
	S1(config)#service password-encryption

# config telnet
	S1(config)#line vty0 15
	S1(config-line)#password pass
	S1(config-line)#login

# config ssh
	S1(config)#crypto key generate rsa
	S1(config)#ip ssh version 2
	S1(config)#line vty0 15
	S1(config-line)#transport input ssh
	S1(config-line)#login local
	S1(config)#username admin password 123

# port security
	S1(config-if)#switchport port-security maximum 1
	S1(config-if)#switchport port-security mac-address sticky
	S1(config-if)#switchport port-security violation {protect | restrict | shutdown}

# config switch-switch
	S3(config)#interfacefa0/1
	S3(config-if)#switchport mode trunk
	S3(config-if)#switchport trunk allowed vlan 10,20,99
	S3(config-if)#switchport trunk native vlan 99

# config switch-pc
	S3(config)#interface fa0/3
	S3(config-if)#switchport mode access
	S3(config-if)#switchport access vlan 20

# VTP
	Switch(config)#vtp domain Esprit
	Switch(config)#vtp password cisco
	Switch(config)#vtp version 2
	Switch(config)#vtp mode {server,client,transparent}
	Switch#show vtp status
	Switch#show vlan brief
	Configure trunk between switchs

# Ether-Channel
	Port Aggreg Protocol/Link Aggreg Control Protocol
	S3(config)#interface range fa0/3-4
	S3(config-if)#channel-group 1 mode auto
	S3(config-if)#exit
	S3(config-if)#interface port-channel 1
	encapsulation, mode trunk, trunk allowed, trunk native

# STP
	Switch#show spanning-tree
	Switch(config)#spanning-tree vlan vid priority{0-255}
	Switch(config)#spanning-tree vlan vid root primary
	Switch(config)#interface fa0/2
	Switch(config-if)#spanning-tree portfast
	Switch(config-if)#spanning-tree bpduguard enable

# HSRP: Hot Standby Router Protocol
	R1(config)#interface fa0/0
	R1(config-if)#ip address 192.168.1.1 255.255.255.0
	R1(config-if)#standby 1 ip 192.168.1.254
	R1(config-if)#standby 1 priority 120
	R1(config-if)#standby 1 preempt
	R2(config)#interface fa0/0
	R2(config-if)#ip address 192.168.1.2 255.255.255.0
	R2(config-if)#standby 1 ip 192.168.1.254
	R1#show standby brief

# Inter Vlan w/Router
	Router(config)#interface fa0/0.10
	Router(config-subif)#encapsulation dot1q 10
	Router(config-subif)#ip address 192.168.10.1 255.255.255.0

# Inter Vlan w/Switch
	Switch(config)#interface fa0/1
	Switch(config-if)#switchport trunk encapsulation dot1q
	Switch(config-if)#switchport mode trunk ec
	Switch(config-if)#switchport trunk allowed vlan 10,20,99
	Switch(config-if)#switchport trunk native vlan 99
	Switch(config)#interface vlan 10
	Switch(config-if)#ip address 192.168.10.1 255.255.255.0
	Switch(config)#interface vlan 20
	Switch(config-if)#ip address 192.168.20.1 255.255.255.0
