### ONOS CLI

    onos> devices
    id=of:0000000000000001, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD        Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000001, channelId=127.0.0.1:49103
    id=of:0000000000000002, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000002, channelId=127.0.0.1:49102
    id=of:0000000000000191, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000191, channelId=127.0.0.1:49105   
    id=of:0000000000000192, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000192, channelId=127.0.0.1:49104
    onos> hosts
    id=00:00:00:00:00:01/-1, mac=00:00:00:00:00:01, location=of:0000000000000001/3, vlan=-1, ip(s)=[10.0.1.1], name=00:00:00:00:00:01/-1
    id=00:00:00:00:00:02/-1, mac=00:00:00:00:00:02, location=of:0000000000000001/4, vlan=-1, ip(s)=[10.0.1.2], name=00:00:00:00:00:02/-1
    id=00:00:00:00:00:03/-1, mac=00:00:00:00:00:03, location=of:0000000000000002/3, vlan=-1, ip(s)=[10.0.2.1], name=00:00:00:00:00:03/-1
    id=00:00:00:00:00:04/-1, mac=00:00:00:00:00:04, location=of:0000000000000002/4, vlan=-1, ip(s)=[10.0.2.2], name=00:00:00:00:00:04/-1
    onos> 
    onos> summary
    node=127.0.0.1, version=1.5.0.SNAPSHOT
    nodes=1, devices=4, links=8, hosts=4, SCC(s)=1, flows=112, intents=0
    onos> 
    onos> apps -s -a
    *   6 org.onosproject.lldpprovider         1.5.0.SNAPSHOT ONOS LLDP link provider.
    *  16 org.onosproject.drivers              1.5.0.SNAPSHOT Default device drivers
    *  44 org.onosproject.openflow-base        1.5.0.SNAPSHOT OpenFlow protocol southbound providers
    *  56 org.onosproject.segmentrouting       1.5.0.SNAPSHOT Segment routing application.
    *  58 org.onosproject.netcfghostprovider   1.5.0.SNAPSHOT Host provider that uses network config service to discover hosts. 
    onos> 
    onos> 
    onos> ports
    id=of:0000000000000001, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000001, channelId=127.0.0.1:49103
      port=local, state=enabled, type=copper, speed=10 , portName=tap:, portMac=00:00:00:00:00:01
      port=1, state=enabled, type=copper, speed=10485 , portName=leaf1-eth1, portMac=4a:d9:e1:77:2a:08
      port=2, state=enabled, type=copper, speed=10485 , portName=leaf1-eth2, portMac=82:d6:3b:67:b4:9c
      port=3, state=enabled, type=copper, speed=10485 , portName=leaf1-eth3, portMac=d2:bc:55:55:e1:62
      port=4, state=enabled, type=copper, speed=10485 , portName=leaf1-eth4, portMac=12:87:aa:13:6d:9f
    id=of:0000000000000002, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000002, channelId=127.0.0.1:49102
      port=local, state=enabled, type=copper, speed=10 , portName=tap:, portMac=00:00:00:00:00:02
      port=1, state=enabled, type=copper, speed=10485 , portName=leaf2-eth1, portMac=9e:d7:32:3e:80:7e
      port=2, state=enabled, type=copper, speed=10485 , portName=leaf2-eth2, portMac=f6:33:53:08:dd:6b
      port=3, state=enabled, type=copper, speed=10485 , portName=leaf2-eth3, portMac=c6:fe:f1:38:2c:e9
      port=4, state=enabled, type=copper, speed=10485 , portName=leaf2-eth4, portMac=66:1d:77:a9:51:1b
    id=of:0000000000000191, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000191, channelId=127.0.0.1:49105
      port=local, state=enabled, type=copper, speed=10 , portName=tap:, portMac=00:00:00:00:01:91
      port=1, state=enabled, type=copper, speed=10485 , portName=spine401-eth1, portMac=2a:57:ba:17:9c:f4
      port=2, state=enabled, type=copper, speed=10485 , portName=spine401-eth2, portMac=76:37:ab:6e:5d:6c
    id=of:0000000000000192, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=ofdpa-cpqd, name=of:0000000000000192, channelId=127.0.0.1:49104
      port=local, state=enabled, type=copper, speed=10 , portName=tap:, portMac=00:00:00:00:01:92
      port=1, state=enabled, type=copper, speed=10485 , portName=spine402-eth1, portMac=32:f2:93:74:ba:6b
      port=2, state=enabled, type=copper, speed=10485 , portName=spine402-eth2, portMac=ee:48:04:89:91:a0
    onos> 
    onos> 
    onos> portstats
    deviceId=of:0000000000000001
       port=1, pktRx=361, pktTx=352, bytesRx=29394, bytesTx=28764, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=2, pktRx=362, pktTx=352, bytesRx=29472, bytesTx=28764, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=3, pktRx=35, pktTx=382, bytesRx=2904, bytesTx=30560, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=4, pktRx=35, pktTx=382, bytesRx=2884, bytesTx=30608, pktRxDrp=0, pktTxDrp=0, Dur=523
    deviceId=of:0000000000000002
       port=1, pktRx=360, pktTx=352, bytesRx=29316, bytesTx=28764, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=2, pktRx=361, pktTx=352, bytesRx=29394, bytesTx=28764, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=3, pktRx=37, pktTx=388, bytesRx=3042, bytesTx=30952, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=4, pktRx=37, pktTx=385, bytesRx=3042, bytesTx=30826, pktRxDrp=0, pktTxDrp=0, Dur=523
    deviceId=of:0000000000000191
       port=1, pktRx=361, pktTx=352, bytesRx=29442, bytesTx=28716, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=2, pktRx=360, pktTx=352, bytesRx=29364, bytesTx=28716, pktRxDrp=0, pktTxDrp=0, Dur=523
    deviceId=of:0000000000000192
       port=1, pktRx=362, pktTx=352, bytesRx=29520, bytesTx=28716, pktRxDrp=0, pktTxDrp=0, Dur=523
       port=2, pktRx=361, pktTx=352, bytesRx=29442, bytesTx=28716, pktRxDrp=0, pktTxDrp=0, Dur=523
    onos> 
    onos> flows pending_add
    deviceId=of:0000000000000001, flowRuleCount=0
    deviceId=of:0000000000000002, flowRuleCount=0
    deviceId=of:0000000000000191, flowRuleCount=0
    deviceId=of:0000000000000192, flowRuleCount=0
    onos> 
    onos> 
    onos> flows | grep Count
    deviceId=of:0000000000000001, flowRuleCount=36
    deviceId=of:0000000000000002, flowRuleCount=36
    deviceId=of:0000000000000191, flowRuleCount=20
    deviceId=of:0000000000000192, flowRuleCount=20
    onos> 
    onos> groups | grep Count
    deviceId=of:0000000000000001, groupCount=19
    deviceId=of:0000000000000002, groupCount=17
    deviceId=of:0000000000000191, groupCount=7
    deviceId=of:0000000000000192, groupCount=7
    onos> 
    onos> 
    onos> 

