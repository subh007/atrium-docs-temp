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

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/sdnip.jpg)

First some terminology: "bgpPeers" refers to other routers connected to our Atrium router. These peers could be traditional routers or other Atrium routers. "bgpSpeakers" refers to our Atrium router i.e there is only one bgpSpeaker.

For the "bgpPeers", our app needs to know two things: what is the IP address of the peer's interface, and where is it attached to our Atrium router. From the example, for peer1, the IP address of the peer's interface is 192.168.10.1, and it is attached to our Atrium router (attachmentDpid: "00:00:00:15:4d:0a:0c:24") on port 10.

For the "bgpSpeakers", our app needs to know our Atrium router's interface IP addresses, in the "interfaceAddresses" section -- yes this is a repeat of information already entered in the addresses.json file, but bear with us, and enter it again. For now, ignore the other stuff under "bgpSpeakers" like "attachmentDpid" and "attachmentPort" - you don't need to change that (in fact, you should not change that).

That's it - you are done with configuring ONOS. Note that this kind of configuration (interface IPs, vlans, peers etc.) is typically done using the router CLI when using traditional networking. In SDN, this configuration is done in the controller; the underlying data-plane switch is completely unaware of this configuration, and never needs to be touched!

### Launching ONOS for Test
This is the easy part. If you are launching ONOS just to test, then running the following command from the shell (over ssh) is just fine. But, if you are looking to deploy the router, it is recommended to use the instructions in the next section.

To be sure that there is no remaining processes left over from previous runs, it is a good idea to run "./router-cleanup.sh".

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/clean.jpg)

ONOS may already be running. Use the following command to ensure that ONOS is not started automatically

`admin@atrium:~$ onos-service localhost stop`

Then to launch, just run "ok clean" (note that this is one way to launch ONOS - another way is to use onos-service start/stop)

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/clean.jpg)

ONOS may already be running. Use the following command to ensure that ONOS is not started automatically

`admin@atrium:~$ onos-service localhost stop`

Then to launch, just run "ok clean" (note that this is one way to launch ONOS - another way is to use onos-service start/stop)

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/tmux.jpg)
