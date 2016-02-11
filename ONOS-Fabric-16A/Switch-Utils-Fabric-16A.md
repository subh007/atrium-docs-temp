### Accton Configuration


### OF-DPA Client Programs

OF-DPA comes with a set of utility programs for debugging support. They can be found in the example directory

    root@onl-x86:~# ls -l /usr/bin/ofdpa-i.12.1.1/examples/

The most useful ones for this release include the ability to see the flows and groups that are installed in the switch using client_flowtable_dump

    root@onl-powerpc:~# client_flowtable_dump 
    Table ID 10 (VLAN):   Retrieving all entries. Max entries = 16384, Current entries = 6.
    -- inPort = 12 vlanId:mask = 0x1064:0x1fff (VLAN 100) etherType:mask = 0x0000:0x0014 | GoTo = 0 (None) | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort = 24 vlanId:mask = 0x0000:0x1fff (VLAN 0) etherType:mask = 0x0000:0x0014 | GoTo = 0 (None) | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort = 24 vlanId:mask = 0x1ffe:0x1fff (VLAN 4094) etherType:mask = 0x0000:0x0014 | GoTo = 0 (None) | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort = 48 vlanId:mask = 0x0000:0x1fff (VLAN 0) etherType:mask = 0x0000:0x0014 | GoTo = 0 (None) | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort = 48 vlanId:mask = 0x1064:0x1fff (VLAN 100) etherType:mask = 0x0000:0x0014 | GoTo = 0 (None) | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort = 48 vlanId:mask = 0x1ffe:0x1fff (VLAN 4094) etherType:mask = 0x0000:0x0014 | GoTo = 0 (None) | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0

    Table ID 20 (Termination MAC):   Retrieving all entries. Max entries = 512, Current entries = 8.
    -- inPort:mask = 12:0xffffffff etherType = 0x0800 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 100:0xfff | GoTo = 30 (Unicast Routing) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 12:0xffffffff etherType = 0x8847 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 100:0xfff | GoTo = 23 (MPLS 0) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 24:0xffffffff etherType = 0x0800 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 4094:0xfff | GoTo = 30 (Unicast Routing) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 24:0xffffffff etherType = 0x8847 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 4094:0xfff | GoTo = 23 (MPLS 0) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 48:0xffffffff etherType = 0x0800 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 100:0xfff | GoTo = 30 (Unicast Routing) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 48:0xffffffff etherType = 0x8847 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 100:0xfff | GoTo = 23 (MPLS 0) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 48:0xffffffff etherType = 0x0800 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 4094:0xfff | GoTo = 30 (Unicast Routing) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0
    -- inPort:mask = 48:0xffffffff etherType = 0x8847 destMac:mask = a29b.329d.7fb3:ffff.ffff.ffff vlanId:mask = 4094:0xfff | GoTo = 23 (MPLS 0) outPort = 0 | priority = 32768 hard_time = 0 idle_time = 0 cookie = 0

    Table ID 30 (Unicast Routing):   Retrieving all entries. Max entries = 90112, Current entries = 2.
    -- etherType = 0x0800 vrf:mask = 0x0000:0x0000 dstIp4 = 1.0.0.0/255.255.0.0 dstIp6 = ::/:: | GoTo = 60 (ACL Policy) groupId = 0x2000000c | priority = 180 hard_time = 0 idle_time = 0 cookie = 0
    -- etherType = 0x0800 vrf:mask = 0x0000:0x0000 dstIp4 = 2.0.0.0/255.255.0.0 dstIp6 = ::/:: | GoTo = 60 (ACL Policy) groupId = 0x20000018 | priority = 180 hard_time = 0 idle_time = 0 cookie = 0

    Table ID 60 (ACL Policy):   Retrieving all entries. Max entries = 3072, Current entries = 13.
    -- inPort:mask = 0:0x0 srcMac:mask = 0000.0000.0000:0000.0000.0000 destMac:mask = 0000.0000.0000:0000.0000.0000 etherType = 0806 tunnelId = 0 srcIp4 = 0.0.0.0/0.0.0.0 dstIp4 = 0.0.0.0/0.0.0.0 srcIp6 = ::/:: dstIp6 = ::/:: DSCP = 0 VRF = 0 DEI = 0 ECN = 0 IP Protocol = 0x00 Source L4 Port = 0 Destination L4 Port = 0 ICMP Type = 0 ICMP Code = 0 | outPort = 0 | priority = 40000 hard_time = -3 idle_time = 0 cookie = 0
    -- inPort:mask = 0:0x0 srcMac:mask = 0000.0000.0000:0000.0000.0000 destMac:mask = 0000.0000.0000:0000.0000.0000 etherType = 88cc tunnelId = 0 srcIp4 = 0.0.0.0/0.0.0.0 dstIp4 = 0.0.0.0/0.0.0.0 srcIp6 = ::/:: dstIp6 = ::/:: DSCP = 0 VRF = 0 DEI = 0 ECN = 0 IP Protocol = 0x00 Source L4 Port = 0 Destination L4 Port = 0 ICMP Type = 0 ICMP Code = 0 | outPort = 0 | priority = 40000 hard_time = -3 idle_time = 0 cookie = 0
    -- inPort:mask = 0:0x0 srcMac:mask = 0000.0000.0000:0000.0000.0000 destMac:mask = 0000.0000.0000:0000.0000.0000 etherType = 8942 tunnelId = 0 srcIp4 = 0.0.0.0/0.0.0.0 dstIp4 = 0.0.0.0/0.0.0.0 srcIp6 = ::/:: dstIp6 = ::/:: DSCP = 0 VRF = 0 DEI = 0 ECN = 0 IP Protocol = 0x00 Source L4 Port = 0 Destination L4 Port = 0 ICMP Type = 0 ICMP Code = 0 | outPort = 0 | priority = 40000 hard_time = -3 idle_time = 0 cookie = 0

and client_grouptable_dump

    root@onl-powerpc:~# client_grouptable_dump 
    groupId = 0x0064000c (L2 Interface, VLAN ID = 100, Port ID = 12): duration: 616, refCount:3
	bucketIndex = 0: outputPort = 12 popVlanTag = 0 
    groupId = 0x00640030 (L2 Interface, VLAN ID = 100, Port ID = 48): duration: 616, refCount:3
	bucketIndex = 0: outputPort = 48 popVlanTag = 0 
    groupId = 0x0ffe0018 (L2 Interface, VLAN ID = 4094, Port ID = 24): duration: 616, refCount:3
	bucketIndex = 0: outputPort = 24 popVlanTag = 1 
    groupId = 0x0ffe0030 (L2 Interface, VLAN ID = 4094, Port ID = 48): duration: 616, refCount:3
	bucketIndex = 0: outputPort = 48 popVlanTag = 1 
    groupId = 0x2000000c (L3 Unicast, Index = 12): duration: 613, refCount:1
	bucketIndex = 0: referenceGroupId = 0x0064000c vlanId = 100 srcMac: A2:9B:32:9D:7F:B3 dstMac: 7A:A9:4F:1E:51:EE 
    groupId = 0x20000018 (L3 Unicast, Index = 24): duration: 614, refCount:1
	bucketIndex = 0: referenceGroupId = 0x0ffe0018 vlanId = 4094 srcMac: A2:9B:32:9D:7F:B3 dstMac: 92:4D:DA:D8:23:34 

Occasionally, during debugging, it may be necessary to remove all flows and groups in the switch before starting the OF client to connect to the controller. This is because the client does not take into account the flows that are already in the ASIC, and assumes the forwarding tables are empty at the time of connection to the controller. Emptying out the tables is done with `client_cfg_purge`



