### Installation Guide

There is actually nothing to install if you want to work with software switches - everything is included with the distribution VM. We use the [CPqD OF 1.3 Software switch](https://github.com/CPqD/ofsoftswitch13) in the [Mininet](http://mininet.org/) environment to create the Atrium router dataplane, the Quagga instance that is part of Atrium, as well as external routers (also Quagga) and hosts.

Importantly, we use the software switch to emulate the hardware switch, by replicating the same fowarding-table-pipeline  you will use in hardware. For example NoviFlow hardware and the emulation switch both use a 2-table pipeline for the Atrium router. Similarly, the Accton switch and the emulation switch (CPqD) both use the pipeline defined in the [OF-DPA spec](https://github.com/Broadcom-Switch/of-dpa/tree/master/OF-DPA-2.0). This emulation is done with the help of ONOS, where we use "drivers" to talk to different OpenFlow switches.

For the hardware Accton switch using OF-DPA, we use the regular "ofdpa" driver

     <driver name="ofdpa" extends="default"
                manufacturer="Broadcom Corp." hwVersion="OF-DPA.*" swVersion="OF-DPA.*">

For the CPqD switch emulating OF-DPA for the **router**, we use the "ofdpa-cpqd-vlan" driver

     <driver name="ofdpa-cpqd-vlan" extends="default"
                manufacturer="ONF"
                hwVersion="OF1.3 Software Switch from CPqD" swVersion="for Group Chaining">
      
For the hardware NoviFlow switch, as well as the software-switch emulating the NoviFlow hardware, we use the "softrouter" driver.

       <driver name="softrouter" extends="default"
                manufacturer="Various" hwVersion="various" swVersion="0.0.0">


  
Note that when using software switches, ONOS needs to be told which driver to use. This is done via the "devices" configuration in the ~/Applications/config/network-cfg.json file in the Atrium-ONOS distribution VM

### Quick Start

To quickly get started with software switches, follow the recipe below to launch ONOS

    admin@atrium16A:~$ cell atrium_router
    admin@atrium16A:~$ cp Applications/config/network-cfg.json.router.mn Applications/config/network-cfg.json
    admin@atrium16A:~$ ok clean

From a different shell, start the script to launch Mininet with the software-switch, quagga-instances and end-hosts. 


    admin@atrium16A:~$ sudo ./router-test.py

This will bring up the setup shown below

![](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/topoR.png)

There are a few things to note here:
* The picture shows the expected way to deploy the Atrium router with the router-deploy.py script. This is why we only see ONOS and Quagga inside the box marked "Atrium Distribution VM". But the script we just launched (router_test.py) creates everything you see in the picture within the Atrium  Distribution VM.
* The router-test.py script launches the "Dataplane Switch" using the CPqD software switch. Use the "driver" configuration in network-cfg.json to set "softrouter" for NoviFlow emulation, or "ofdpa-cpqd-vlan" for Accton emulation.
* The router-test.py creates 3 physical (dataplane) interfaces on the CPqD switch
    * port 1 is configured 192.168.10.101 and connected to an external router "peer 1" on vlan 100.
    * port 2 is configured 192.168.20.101 and connected to another external router "peer 2" on an untagged interface.
    * **port 3 is a dataplane interface that is used for a different purpose** -- connecting to the Quagga host that "speaks" the control protocols BGP, OSPF etc. See [here](https://github.com/onfsdn/atrium-docs/wiki/Hardware-Install-ONOS-Router-16A#special-requirements-for-hardware-switches) for details.
    * the switch also has a management interface over which it connects to ONOS using OpenFlow 1.3

It is possible to see all the instantiations as follows

    admin@atrium16A:~$ ps aux | grep mininet
    root     32305  0.0  0.0  21164  2156 pts/2    Ss+  19:26   0:00 bash --norc -is mininet:c0
    root     32311  0.0  0.0  21164  2152 pts/4    Ss+  19:26   0:00 bash --norc -is mininet:host1
    root     32316  0.0  0.0  21164  2156 pts/5    Ss+  19:26   0:00 bash --norc -is mininet:host2
    root     32320  0.0  0.0  21164  2152 pts/6    Ss+  19:26   0:00 bash --norc -is mininet:peer1
    root     32322  0.0  0.0  21164  2152 pts/7    Ss+  19:26   0:00 bash --norc -is mininet:peer2
    root     32324  0.0  0.0  21164  2152 pts/8    Ss+  19:26   0:00 bash --norc -is mininet:qh
    root     32328  0.0  0.0  21164  2152 pts/9    Ss+  19:26   0:00 bash --norc -is mininet:root1
    root     32330  0.0  0.0  21168  2176 pts/10   Ss+  19:26   0:00 bash --norc -is mininet:router
    admin    32750  0.0  0.0  11736   936 pts/11   S+   19:27   0:00 grep --color=auto mininet

Here "router" is the CPqD switch instantiating the dataplane-switch part of the Atrium router. "qh" is the Quagga Linux host that instantiates the routing-protocols for the Atrium router. The hosts and peers are as shown in the diagram.

Use the mininet utility to enter "qh", telnet into Quagga BGP and check the BGP peers the Atrium router sees.

    admin@atrium16A:~$ ./mininet/util/m qh
    
    root@atrium16A:~# telnet localhost 2605
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.
    
    Hello, this is Quagga (version 0.99.24.1).
    Copyright 1996-2005 Kunihiro Ishiguro, et al.

    User Access Verification
    Password: sdnip 

    qh> en
    qh# 
    qh# sh ip bgp summary 
    BGP router identifier 1.1.1.11, local AS number 65000
    RIB entries 3, using 336 bytes of memory
    Peers 2, using 9136 bytes of memory

    Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    192.168.10.1    4 65001      23      24        0    0    0 00:00:58        1
    192.168.20.1    4 65002      22      25        0    0    0 00:00:58        1

    Total number of neighbors 2

The display above shows that the Atrium router peers with the external routers (AS 65001 and 65002) and has learnt 1  route from each. 

Use the mininet utility again to enter one of the hosts, and ping the other host to verify dataplane connectivity.

    admin@atrium16A:~$ ./mininet/util/m host1

    root@atrium16A:~# ping 2.0.0.1
    PING 2.0.0.1 (2.0.0.1) 56(84) bytes of data.
    64 bytes from 2.0.0.1: icmp_req=1 ttl=62 time=0.397 ms
    64 bytes from 2.0.0.1: icmp_req=2 ttl=62 time=0.341 ms
    64 bytes from 2.0.0.1: icmp_req=3 ttl=62 time=0.380 ms
    ^C
    --- 2.0.0.1 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2000ms
    rtt min/avg/max/mdev = 0.341/0.372/0.397/0.032 ms
    root@atrium16A:~# 

Use the ONOS CLI to see information about devices and hosts that the controller sees

    onos> devices
    id=of:0000000000000001, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=softrouter, name=of:0000000000000001, channelId=127.0.0.1:49221
    onos> 
    onos> ports
    id=of:0000000000000001, available=true, role=MASTER, type=SWITCH, mfr=Stanford University, Ericsson Research and CPqD Research, hw=OpenFlow 1.3 Reference Userspace Switch, sw=May 21 2015 08:32:26, serial=1, managementAddress=127.0.0.1, protocol=OF_13, driver=softrouter, name=of:0000000000000001, channelId=127.0.0.1:49221
      port=local, state=enabled, type=copper, speed=10 , portName=tap:, portMac=00:00:00:00:00:01
      port=1, state=enabled, type=copper, speed=10485 , portName=router-eth1, portMac=1a:77:16:35:d5:f7
      port=2, state=enabled, type=copper, speed=10485 , portName=router-eth2, portMac=0a:65:6c:1a:a3:ce
      port=3, state=enabled, type=copper, speed=10485 , portName=router-eth3, portMac=7a:49:a2:35:d8:1f
    onos> 
    onos> portstats
    deviceId=of:0000000000000001
       port=1, pktRx=1849, pktTx=3691, bytesRx=147964, bytesTx=297090, pktRxDrp=0, pktTxDrp=0, Dur=2869
       port=2, pktRx=1874, pktTx=3732, bytesRx=142123, bytesTx=292503, pktRxDrp=0, pktTxDrp=0, Dur=2869
       port=3, pktRx=3729, pktTx=5545, bytesRx=290245, bytesTx=437699, pktRxDrp=0, pktTxDrp=0, Dur=2869
    onos> 
    onos> hosts
    id=00:00:00:00:00:01/100, mac=00:00:00:00:00:01, location=of:0000000000000001/3, vlan=100, ip(s)=[192.168.10.101]
    id=00:00:00:00:00:02/-1, mac=00:00:00:00:00:02, location=of:0000000000000001/3, vlan=-1, ip(s)=[192.168.20.101]
    id=00:00:00:00:10:01/100, mac=00:00:00:00:10:01, location=of:0000000000000001/1, vlan=100, ip(s)=[192.168.10.1]
    id=00:00:00:00:20:01/-1, mac=00:00:00:00:20:01, location=of:0000000000000001/2, vlan=-1, ip(s)=[192.168.20.1]
    onos> 

Notice that ONOS sees the peers 10.1 and 20.1 at the expected ports 1 and 2 on switch of:1 with VLANs 100 and -1 (untagged) respectively. It also sees it's own interface addresses 10.101 and 20.101 on port 3, which is connected to the Quagga host, because these addresses are actually configured on the Quagga host interface (the script router-test does that for you). 

You can also use the "flows" and "groups" commands on the ONOS CLI to see the flows/groups programmed in the dataplane switch. Of course the output of these commands depends on which "driver" was used in ONOS.