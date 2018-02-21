В каждом роутере 1-3 добалены физические модули NM-1E(знаю, что нужны FE)

Ip адреса:
	PC0
		ip address:10.16.1.27
		subnet mask:255.255.255.0
		default gateway:10.16.1.1
	PC1
		ip address:10.16.1.26
		subnet mask:255.255.255.0
		default gateway:10.16.1.1
	Server0
		ip address:192.168.10.26
		subnet mask:255.255.255.252
	Server1
		ip address:192.168.10.26
		subnet mask:255.255.255.252
	Server2
		ip address:10.16.1.26
		subnet mask:255.255.255.0
Router0
	
	Router(config)#interface FastEthernet0/0
	Router(config-if)#ip address 192.168.10.1 255.255.255.252
	Router(config-if)#no shutdown
	Router(config-if)#exit

	Router(config)#interface FastEthernet0/1
	Router(config-if)#ip address 10.16.1.1 255.255.255.0
	Router(config-if)#no shutdown
	Router(config-if)#exit

	Router(config)#interface Ethernet1/0
	Router(config-if)#ip address 192.168.1.1 255.255.255.0
	Router(config-if)#no shutdown
	Router(config-if)#exit

	RIP
	Router(config)#router rip
	Router(config-router)#network 192.168.1.0
	Router(config-router)#network 192.168.10.0
	Router(config-router)#network 10.16.1.0
	Router(config-router)#exit

	Router#show ip interface brief
	Interface              IP-Address      OK? Method Status                Protocol 
	FastEthernet0/0        192.168.10.1    YES manual up                    up 
	FastEthernet0/1        10.16.1.1       YES manual up                    up 
	Ethernet1/0            192.168.1.1     YES manual up                    up 
	Vlan1                  unassigned      YES unset  administratively down down

	Router#show ip rip database
	10.16.1.0/24    auto-summary
	10.16.1.0/24    directly connected, FastEthernet0/1
	192.168.1.0/24    auto-summary
	192.168.1.0/24    directly connected, Ethernet1/0
	192.168.2.0/24    auto-summary
	192.168.2.0/24
	    [1] via 192.168.10.2, 00:00:18, FastEthernet0/0
	192.168.3.0/24    auto-summary
	192.168.3.0/24
	    [1] via 10.16.1.2, 00:00:00, FastEthernet0/1
	192.168.10.0/30    auto-summary
	192.168.10.0/30    directly connected, FastEthernet0/0
	192.168.10.4/30    auto-summary
	192.168.10.4/30
	    [1] via 192.168.10.2, 00:00:18, FastEthernet0/0

	Router#show ip route
	Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
	       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
	       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
	       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
	       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
	       * - candidate default, U - per-user static route, o - ODR
	       P - periodic downloaded static route
	
	Gateway of last resort is not set
	
	     10.0.0.0/24 is subnetted, 1 subnets
	C       10.16.1.0 is directly connected, FastEthernet0/1
	C    192.168.1.0/24 is directly connected, Ethernet1/0
	R    192.168.2.0/24 [120/1] via 192.168.10.2, 00:00:22, FastEthernet0/0
	R    192.168.3.0/24 [120/1] via 10.16.1.2, 00:00:02, FastEthernet0/1
	     192.168.10.0/30 is subnetted, 2 subnets
	C       192.168.10.0 is directly connected, FastEthernet0/0
	R       192.168.10.4 [120/1] via 192.168.10.2, 00:00:22, FastEthernet0/0

Router1
	
	Router(config)#interface FastEthernet0/0
	Router(config-if)#ip address 192.168.10.2 255.255.255.252
	Router(config-if)#no shutdown
	Router(config-if)#exit

	Router(config)#interface FastEthernet0/1
	Router(config-if)#ip address 192.168.10.6 255.255.255.252
	Router(config-if)#no shutdown
	Router(config-if)#exit

	Router(config)#interface Ethernet1/0
	Router(config-if)#ip address 192.168.2.1 255.255.255.0
	Router(config-if)#no shutdown
	Router(config-if)#exit

	RIP
	Router(config)#router rip
	Router(config-router)#network 192.168.2.0
	Router(config-router)#network 192.168.10.0
	Router(config-router)#network 10.16.1.0
	Router(config-router)#exit

	Router#show ip interface brief
	Interface              IP-Address      OK? Method Status                Protocol 
	FastEthernet0/0        192.168.10.2    YES manual up                    up 
	FastEthernet0/1        192.168.10.6    YES manual up                    up 
	Ethernet1/0            192.168.2.1     YES manual up                    up 
	Vlan1                  unassigned      YES unset  administratively down down

	Router#show ip rip database
	10.0.0.0/8    auto-summary
	10.0.0.0/8
	    [1] via 192.168.10.5, 00:00:08, FastEthernet0/1    [1] via 192.168.10.1, 00:00:01, FastEthernet0/0
	192.168.1.0/24    auto-summary
	192.168.1.0/24
	    [1] via 192.168.10.1, 00:00:01, FastEthernet0/0
	192.168.2.0/24    auto-summary
	192.168.2.0/24    directly connected, Ethernet1/0
	192.168.3.0/24    auto-summary
	192.168.3.0/24
	    [1] via 192.168.10.5, 00:00:08, FastEthernet0/1
	192.168.10.0/30    auto-summary
	192.168.10.0/30    directly connected, FastEthernet0/0
	192.168.10.4/30    auto-summary
	192.168.10.4/30    directly connected, FastEthernet0/1

	Router#show ip route
	Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
	       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
	       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
	       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
	       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
	       * - candidate default, U - per-user static route, o - ODR
	       P - periodic downloaded static route
	
	Gateway of last resort is not set
	
	R    10.0.0.0/8 [120/1] via 192.168.10.5, 00:00:14, FastEthernet0/1
	                [120/1] via 192.168.10.1, 00:00:10, FastEthernet0/0
	R    192.168.1.0/24 [120/1] via 192.168.10.1, 00:00:10, FastEthernet0/0
	C    192.168.2.0/24 is directly connected, Ethernet1/0
	R    192.168.3.0/24 [120/1] via 192.168.10.5, 00:00:14, FastEthernet0/1
	     192.168.10.0/30 is subnetted, 2 subnets
	C       192.168.10.0 is directly connected, FastEthernet0/0
	C       192.168.10.4 is directly connected, FastEthernet0/1


Router2
	
	Router(config)#interface FastEthernet0/0
	Router(config-if)#ip address 10.16.1.2 255.255.255.0
	Router(config-if)#no shutdown
	Router(config-if)#exit

	Router(config)#interface FastEthernet0/1
	Router(config-if)#ip address 192.168.10.5 255.255.255.252
	Router(config-if)#no shutdown
	Router(config-if)#exit

	Router(config)#interface Ethernet1/0
	Router(config-if)#ip address 192.168.3.1 255.255.255.0
	Router(config-if)#no shutdown
	Router(config-if)#exit

	RIP
	Router(config)#router rip
	Router(config-router)#network 192.168.3.0
	Router(config-router)#network 192.168.10.0
	Router(config-router)#network 10.16.1.0
	Router(config-router)#exit

	Router#show ip interface brief
	Interface              IP-Address      OK? Method Status                Protocol 
	FastEthernet0/0        10.16.1.2       YES manual up                    up 
	FastEthernet0/1        192.168.10.5    YES manual up                    up 
	Ethernet1/0            192.168.3.1     YES manual up                    up 
	Vlan1                  unassigned      YES unset  administratively down down

	Router#show ip rip database
	10.16.1.0/24    auto-summary
	10.16.1.0/24    directly connected, FastEthernet0/0
	192.168.1.0/24    auto-summary
	192.168.1.0/24
	    [1] via 10.16.1.1, 00:00:01, FastEthernet0/0
	192.168.2.0/24    auto-summary
	192.168.2.0/24
	    [1] via 192.168.10.6, 00:00:01, FastEthernet0/1
	192.168.3.0/24    auto-summary
	192.168.3.0/24    directly connected, Ethernet1/0
	192.168.10.0/30    auto-summary
	192.168.10.0/30
	    [1] via 192.168.10.6, 00:00:01, FastEthernet0/1
	192.168.10.4/30    auto-summary
	192.168.10.4/30    directly connected, FastEthernet0/1

	Router#show ip route
	Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
	       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
	       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
	       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
	       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
	       * - candidate default, U - per-user static route, o - ODR
	       P - periodic downloaded static route

	Gateway of last resort is not set

	     10.0.0.0/24 is subnetted, 1 subnets
	C       10.16.1.0 is directly connected, FastEthernet0/0
	R    192.168.1.0/24 [120/1] via 10.16.1.1, 00:00:20, FastEthernet0/0
	R    192.168.2.0/24 [120/1] via 192.168.10.6, 00:00:17, FastEthernet0/1
	C    192.168.3.0/24 is directly connected, Ethernet1/0
	     192.168.10.0/30 is subnetted, 2 subnets
	R       192.168.10.0 [120/1] via 192.168.10.6, 00:00:17, FastEthernet0/1
	C       192.168.10.4 is directly connected, FastEthernet0/1