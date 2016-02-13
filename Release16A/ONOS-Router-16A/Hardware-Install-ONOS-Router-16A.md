For the ONOS based router in Atrium Release 2016/A, you have two choices: [Accton/EdgeCore](http://www.edge-core.com/prodcat.asp?c=1) or [NoviFlow](http://noviflow.com/products/noviswitch/). First follow the guide below for setting up the switch of your choice. Then ensure that you wire up the hardware switch correctly, by following the [special requirements](https://github.com/onfsdn/atrium-docs/wiki/Hardware-Install-ONOS-Router-16A#special-requirements-for-hardware-switches).


### NoviFlow Installation

We have done our tests on NoviSwitch NS-2128 with the following software version:

    noviswitch# show status switch
	Hardware serial number: NS2128-00223D5A005A
	Kernel                : 2.6.32-504.16.2.el6.x86_64
    Switch uptime         : 10 days 06:01:29
	Overall CPU(%) usage  : 10.050251
	Overall Mem(KB) usage : 2131830

	-- latest (current) --
	NoviWare-OPE version: NW300.0.5 9c9bd26a05d7c67f74a2e7f1c639c417b2a53f15
	NoviWare-PPE version: NW300.0.5 4651a68fb61073572bf9f912f471c8dccabb27b9
	EZDriver version: 8.46a

but in principle, it should work on other NoviSwitch hardware and software versions.

SSH to the OF interface on the front panel, whose default IP address is 10.0.0.5. 

`Username: superuser and password: noviflow`

Set a new IP for this interface:

    noviswitch# set config switch device of netipaddr 192.168.1.10 net mask 255.255.255.0 gateway 192.168.1.1 scope global

For Atrium, set a pipeline to use two tables with all the match fields you need (setting a table width of 80 Bytes automatically includes all OF mandatory match fields plus some optional ones):

    noviswitch# set config pipeline tablesizes 10000 10000 tablewidths 80 80

You can change the tablesize in the previous command to make the 2nd table (Table id# 1) to be larger as this is the IP forwarding table for the Atrium Router. 

For this release of Atrium we need the ability to match on ARP SPA field in the packet header in the first table (Table id# 0). Enter the following config

    noviswitch# set config table tableid 0 matchfields 0 3 4 5 6 7 21 22 10 11 12 19 20 13 15 17 14 16 18

Point towards the controller:

    noviswitch# set config controller controllergroup 1 controllerid 1 priority 1 ipaddr <onos_ip> port 6633 security none

If you connect dataplane ports to one of those with a blue label on the front panel (1 to 20) make sure that they are configured to the desired speed (they can support 1Gbps and 10Gbps)

    noviswitch# set config port portno 1 speed 10Gbps


### Accton Installation

For the Atrium router on ONOS, we have certified Accton switch models# 5710, 5712 and 6712. The installation (which is the same for router or fabric) is described [here](https://github.com/onfsdn/atrium-docs/wiki/Hardware-Install-ONOS-Fabric-16A).

### Special Requirements for Hardware Switches

[[https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/cpcp.png]]

In the previous Atrium release (2015/A) the BGP traffic between the dataplane switch (from/to peers) and Quagga in the control plane, was encapsulated within OpenFlow and sent to the controller, and it was ONOS's responsibility to decapsulate and shovel it over to the Quagga host. This led to an interdependence between BGP and OF, which at high flow counts potentially led to stability issues.

In this release of the Atrium Router, we have separated the OpenFlow communications from all control-protocol communications (BGP, OSPF etc) by the arrangement shown in the picture above. OpenFlow communication still happens over the switch's management port, which is either directly connected to the eth0 interface of the Atrium Distribution VM, or carried over a management network (as OpenFlow runs over TCP).

For all other communication (such as BGP), a dataplane port is dedicated for connecting directly to Quagga. This port is called the "ControlPlaneConnectPoint" port in the configuration. ONOS will take care of programming rules that identify  all protocol communication coming in from all other ports of the dataplane switch, and then redirecting them out of this special port. The same is true for the reverse direction. Thus all protocol communication is directly handled in the dataplane ASIC, and never reaches the switch-CPU. The requirement of course is that this special (reserved) port needs to be:
* either physically connected to the eth1 interface of the Atrium Distribution VM
* or tunneled over to the eth1 interface of the VM over a management network. Please note that such a tunnel needs to be an L2 tunnel, which means that entire Ethernet frames emitted by this port needs to be delivered intact and unchanged to the eth1 interface of the VM, and vice-versa (ie. MAC addresses should not change, ARP traffic should not be intercepted and handled by anyone else, etc.).

