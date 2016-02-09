### Prerequisites
We are assuming that you have already downloaded and installed the distribution VM.

Also generic questions on ONOS should be directed to the ONOS mailing lists. Questions specific to the use of ONOS in Atrium, should be directed to the atrium_eng@groups.opensourcesdn.org mailing list. The same applies for all the other software projects that are part of Atrium - eg. ONL, OFDPA, Quagga, Indigo, OVS, etc.

### Configuring ONOS
The distribution VM has taken care of most of the stuff necessary to configure and run ONOS. What we still need to do is configure the routing application within ONOS. In its first release, Atrium does not support the ability to change configuration at runtime, which is a known issue. Therefore, it is necessary to pre-configure the controller/app with router-interface addresses and expected BGP peers before we launch ONOS. As an example configuration, let us consider the network shown below

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/topo.jpg)

There are two files in which the configuration shown above needs to be entered -- addresses.json & sdnip.json.

First lets look at addresses.json which is in the folder Applications/config. This config file is mainly used for ARP handling and router interface IP/VLAN config.

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/addresses.jpg)

With reference to the topology diagram, it is clear that we are specifying the addresses of our router-interfaces. The "dpid" refers to the OpenFlow datapath-id of the data-plane switch; "port" refers to the data-plane switch port (the OpenFlow port number); "vlan" is one vlan-id configured on that port; "ips" are the set of ip-addresses assigned to that port/vlan;  and "mac" is the mac-address assigned to that port.

A few things to note:

* If you wanted to assign another vlan to the same port, you need to create another block within the "addresses" array as the two shown above.
* The "mac" does not need to be the actual mac address of the physical switch-port. But be careful that the mac-address you assign is unique, because this is what the router uses to reply to ARP-requests for its interface IPs (on that vlan).
* The "mac" can be the same for all port-vlans. In that case, you are essentiall using a single "Router-MAC", which is used to reply to ARP-requests for any interface IP, and the source-mac address on all Ethernet frames sent out of this router.
* Most switches don't have any dataplane restrictions on the use of "mac" addresses. However, the Pica8 switch requires the use of a single Router MAC (08:9e:01:82:38:68). See the example config files: Applications/config/pica_addresses.json and Applications/config/pica_sdnip.json

Now let's look at the other file: Applications/config/sdnip.json