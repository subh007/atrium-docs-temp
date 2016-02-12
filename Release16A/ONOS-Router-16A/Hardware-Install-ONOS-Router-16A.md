For the ONOS based router in Atrium Release 2016/A, you have two choices: [Accton/EdgeCore](http://www.edge-core.com/prodcat.asp?c=1) or [NoviFlow](http://noviflow.com/products/noviswitch/).


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

