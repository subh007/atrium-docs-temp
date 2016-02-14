### Distribution VM
To get started with Atrium Release 2015/A, download the distribution VM (Atrium_ODL_2016_A.ova) from here: size ~ 3GB

[link for google drive](link for google drive)

login: admin

password: bgprouter


NOTE: This distribution VM is NOT meant for development. Its sole purpose is to have a working system up and running for test/deployment as painlessly as possible. A developer guide using mechanisms other than this VM will be available shortly after the release.

## Installation Steps
Once you have the VM up and running, the following steps will help you to bring up the system.

admin@atrium:~/atrium-odl$ ./distribution-karaf/target/assembly/bin/karaf
opendaylight-user@root>feature:install odl-atrium-all

You have two choices:

A) You can bring up the Atrium Router completely in software

B) Or you could bring up the Atrium Router in hardware

### Bring up the Atrium Router completely in software
 You can bring up the Atrium Router completely in software, completely self-contained in this VM. In addition, you will get a complete test infrastructure (other routers to peer with, hosts to ping from, etc.) that you can play with (via the router-test.py script). Note that when using this setup, we emulate  hardware-pipelines using software switches.

Following are the steps required to bring up the test topology:

<topology image url>

1) Start the ODL controller from the using the distribution VM. ODL based Atrium
codebase can be found in path "/home/admin/atrium-odl". Launch the controller

cd atrium-odl/
admin@atrium:~/atrium-odl$ ./distribution-karaf/target/assembly/bin/karaf

2) Install the feature "odl-atrium-all". This feature will install the required
components to make stack working. This feature will install BGP Application and
DIDM (Device Identification and Device Management).

opendaylight-user@root>feature:install odl-atrium-all

To ensure the correct feature installation one can check for similar logs:

2016-02-14 01:30:28,022 | INFO  | config-pusher    | Bgprouter                        | 296 - org.opendaylight.atrium.bgprouter-impl - 1.0.0.SNAPSHOT | BGP Router started

Following is the details of installed features:

opendaylight-user@root>feature:list | grep atrium
atrium-thirdparty                          | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :Atrium : thirdparty
atrium-utilservice                         | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: atrium-utilservice
atrium-bgpservice                          | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: bgpservice
atrium-routingconfig                       | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: routingconfig
odl-didm-all                               | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: didm
atrium-hostservice                         | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: hostservice
atrium-routingservice                      | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: routingservice
atrium-atriumcli                           | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: atrium-cli
odl-atrium-all                             | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: bgprouter

opendaylight-user@root>feature:list | grep didm
odl-didm-identification-api                | 0.2.0-SNAPSHOT   | x         | odl-didm-0.2.0-SNAPSHOT             | OpenDaylight :: didm identification :: api
odl-didm-identification                    | 0.2.0-SNAPSHOT   | x         | odl-didm-0.2.0-SNAPSHOT             | OpenDaylight :: didm identification
odl-didm-drivers-api                       | 0.2.0-SNAPSHOT   | x         | odl-didm-0.2.0-SNAPSHOT             | OpenDaylight :: didm drivers :: api
odl-didm-all                               | 1.0-SNAPSHOT     | x         | odl-atrium-1.0-SNAPSHOT             | OpenDaylight :: didm
odl-didm-mininet                           | 0.2.0-SNAPSHOT   | x         | odl-didm-0.2.0-SNAPSHOT             | OpenDaylight :: DIDM Mininet reference driver

opendaylight-user@root>feature:list | grep openflowplugin
odl-openflowplugin-all                     | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: All
odl-openflowplugin-southbound              | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: SouthBound
odl-openflowplugin-flow-services           | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: Flow Services
odl-openflowplugin-nsf-services            | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: OpenflowPlugin :: NSF :: Services
odl-openflowplugin-nsf-model               | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: OpenflowPlugin :: NSF :: Model
odl-openflowplugin-flow-services-rest      | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: Flow Services :
odl-openflowplugin-flow-services-ui        | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: Flow Services :
odl-openflowplugin-drop-test               | 0.2.0-SNAPSHOT   |           | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: Drop Test
odl-openflowplugin-app-table-miss-enforcer | 0.2.0-SNAPSHOT   |           | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: Application - t
odl-openflowplugin-app-config-pusher       | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: app - default c
odl-openflowplugin-app-lldp-speaker        | 0.2.0-SNAPSHOT   | x         | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: app lldp-speake
odl-openflowplugin-app-bulk-o-matic        | 0.2.0-SNAPSHOT   |           | openflowplugin-0.2.0-SNAPSHOT       | OpenDaylight :: Openflow Plugin :: app bulk flow o

3) Now, start the miniet topology using the "router-test.py" script. It will create topology
as described in the above topology diagram.

admin@atrium:~$ sudo ./router-test.py &

admin@atrium:~$ ps aux | grep mininet
root      3896  0.0  0.0  21144  2140 pts/2    Ss+  01:35   0:00 bash --norc -is mininet:c0
root      3903  0.0  0.0  21144  2140 pts/4    Ss+  01:35   0:00 bash --norc -is mininet:bgp1
root      3909  0.0  0.0  21144  2140 pts/5    Ss+  01:35   0:00 bash --norc -is mininet:host1
root      3911  0.0  0.0  21144  2140 pts/6    Ss+  01:35   0:00 bash --norc -is mininet:host2
root      3913  0.0  0.0  21144  2140 pts/7    Ss+  01:35   0:00 bash --norc -is mininet:peer1
root      3915  0.0  0.0  21144  2140 pts/8    Ss+  01:35   0:00 bash --norc -is mininet:peer2
root      3919  0.0  0.0  21144  2140 pts/9    Ss+  01:35   0:00 bash --norc -is mininet:root1
root      3924  0.0  0.0  21144  2144 pts/10   Ss+  01:35   0:00 bash --norc -is mininet:router
root      3927  0.0  0.0  21144  2140 pts/11   Ss+  01:35   0:00 bash --norc -is mininet:s1
admin     4198  0.0  0.0  11708   668 pts/1    S+   01:36   0:00 grep --color=auto mininet

4) Application uses two configuration file (addresses.json and sdnip.json) to install the correct
flow rule for any topology. "addresses.json" holds the port level details for the OF switch and
"sdnip.json" holds the information required for the router peering.

Configuration can be modified at runtime using the "restconf".

Configuration file can be found at following path:
"/home/admin/atrium-odl/distribution-karaf/target/assembly/configuration/initial"

5) Controller will install the flows to control plane and data plane ovs switch
to enable the BGP peering. And finally the bgp peer will exchange the routes with
each other. Controller collects the routes from the control plane Quagga using the
i-bgp connection and push the flow to data plane switch for packet forwarding using
the ovs 2 table pipeline driver.

Flows in control plane switch:
$ sudo ovs-ofctl dump-flows s1 -O OpenFlow13
OFPST_FLOW reply (OF1.3) (xid=0x2):
 cookie=0x0, duration=1901.409s, table=0, n_packets=1203, n_bytes=94173, tcp,tp_dst=179 actions=CONTROLLER:65535
 cookie=0x0, duration=1901.409s, table=0, n_packets=1228, n_bytes=95786, tcp,tp_src=179 actions=CONTROLLER:65535
 cookie=0x0, duration=1901.408s, table=0, n_packets=2, n_bytes=88, arp actions=CONTROLLER:65535
 cookie=0x0, duration=1901.409s, table=0, n_packets=0, n_bytes=0, icmp actions=CONTROLLER:65535

 Flows in data plane switch:
 $ sudo ovs-ofctl dump-flows router -O OpenFlow13 table=1
OFPST_FLOW reply (OF1.3) (xid=0x2):
 cookie=0x0, duration=1972.617s, table=1, n_packets=0, n_bytes=0, priority=180,ip,nw_dst=1.0.0.0/16 actions=write_actions(group:1)
 cookie=0x0, duration=1971.616s, table=1, n_packets=0, n_bytes=0, priority=180,ip,nw_dst=2.0.0.0/16 actions=write_actions(group:2)
admin@atrium:~$ sudo ovs-ofctl dump-flows router -O OpenFlow13 table=0
OFPST_FLOW reply (OF1.3) (xid=0x2):
 cookie=0x0, duration=1983.152s, table=0, n_packets=0, n_bytes=0, ip,in_port=1,dl_vlan=100,dl_dst=00:00:00:00:00:01 actions=pop_vlan,goto_table:1
 cookie=0x0, duration=1983.151s, table=0, n_packets=0, n_bytes=0, ip,in_port=2,dl_vlan=200,dl_dst=00:00:00:00:00:02 actions=pop_vlan,goto_table:1
 cookie=0x0, duration=1983.151s, table=0, n_packets=1228, n_bytes=96392, priority=65535,ip,in_port=1,dl_vlan=100,dl_dst=00:00:00:00:00:01,nw_dst=192.168.10.101 actions=CONTROLLER:65535
 cookie=0x0, duration=1983.151s, table=0, n_packets=1236, n_bytes=96963, priority=65535,ip,in_port=2,dl_vlan=200,dl_dst=00:00:00:00:00:02,nw_dst=192.168.20.101 actions=CONTROLLER:65535
 cookie=0x0, duration=1983.151s, table=0, n_packets=2, n_bytes=92, arp actions=CONTROLLER:65535


 $ sudo ovs-ofctl dump-flows router -O OpenFlow13 table=1
OFPST_FLOW reply (OF1.3) (xid=0x2):
 cookie=0x0, duration=1972.617s, table=1, n_packets=0, n_bytes=0, priority=180,ip,nw_dst=1.0.0.0/16 actions=write_actions(group:1)
 cookie=0x0, duration=1971.616s, table=1, n_packets=0, n_bytes=0, priority=180,ip,nw_dst=2.0.0.0/16 actions=write_actions(group:2)

 In Opendaylight console we can see the learnt routes:

 opendaylight-user@root>atrium:fib
Type : UPDATE	Next Hop Ip : 192.168.10.1	Prefix : 1.0.0.0/16	Next Hop Mac : MacAddress [_value=00:00:00:00:10:01]
Type : UPDATE	Next Hop Ip : 192.168.20.1	Prefix : 2.0.0.0/16	Next Hop Mac : MacAddress [_value=00:00:00:00:20:01]

6) Now we are ready to test ping from one host to another

Using the mininet utilities login to host1(1.0.0.1) and ping to the host2(2.0.0.1).



### Bring up the Atrium Router completely in hardware (Noviflow)

To confiture the Atrium with the hardware openflow switch Required change in configuration
file (sdnip.json and addresses.json) according to topology.

Currently Noviflow hardware platform uses the ovs-2TP (mininet) driver to install
the learnt routes from control plane quagga.


### Known Issues:
* Lack of performance testing and optimizations for performance
* Functionality tests are incomplete both for hardware-switch based tests and emulated-hardware tests.
* Some time the controller goes into deadlock condition, in such cases we need to rebuild
distribuition-karaf artifact (using mvn clean install -DskipTests=True)
