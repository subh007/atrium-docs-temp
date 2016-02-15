#### Network Config for ONOS

In this section, we describe how to configure the router on ONOS using the network-cfg.json file. We will describe ports, peers, devices and app configuration. Note that in this example we are assuming that the file network-cfg.json.router.mn has been copied into network-cfg.json. Where necessary, we will point out differences to other sample config.

We continue the example instantiated by the router-test.py script.

![](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/topoR.png)


    admin@atrium16A:~$ cat Applications/config/network-cfg.json.router.mn
    {
     "devices" : {
          "of:0000000000000001" : {
              "basic" : {
                      "driver" : "softrouter"
                }
          }
    },


    "ports" : {
        "of:0000000000000001/1" : {
            "interfaces" : [
                {
                    "ips" : [ "192.168.10.101/24" ],
                    "mac" : "00:00:00:00:00:01",
                    "vlan" : "100"
                }
            ]
        },
        "of:0000000000000001/2" : {
            "interfaces" : [
                {
                    "ips" : [ "192.168.20.101/24" ],
                    "mac" : "00:00:00:00:00:02"
                }
            ]
        }
    },
    "apps" : {
        "org.onosproject.router" : {
            "bgp" : {
                "bgpSpeakers" : [
                    {
                        "connectPoint" : "of:0000000000000001/3",
                        "peers" : [
                            "192.168.10.1",
                            "192.168.20.1"
                        ]
                    }
                ]
            },
            "router" : {
                "controlPlaneConnectPoint" : "of:0000000000000001/3",
                "ospfEnabled" : "true",
                "pimEnabled" : "true"
            }
        }
    }
    }

Device configuration is necessary when using software switches to tell ONOS which driver to load for the software switch when it connects to ONOS. In this example the `softrouter` driver is loaded for the dataplane switch. The other option is to load the `ofdpa-cpqd-vlan` driver.

Ports configuration is related to assigning interface-addresses to the dataplane interfaces. For each interface defined on a port, a MAC address, VLAN and a set of IP addresses can be configured. The IP address configuration denotes the IP address of the interface as well as the subnet. For example on port 1 of the dataplane switch of:1, the subnet is `192.168.10.0/24`, while the IP address of the interface is `192.168.10.101`. The MAC address of the interface can be of your choice and does not have to relate to the actual physical interface MAC address. They can also be the same for all interfaces. Vlan ID -1 denoted an untagged interface. It is important that the same addresses are configured on the Quagga linux host interface connected to the dataplane "special" port reserved for protocol communications. The router-test and router-deploy scripts do this for you.

Under apps configuration, we need to specify the controlPlaneConnectPoint, which is the special port reserved for protocol communication with external routers. Note that in this example, it is port 3 on the dataplane switch of:1. Finally the BGP peers (external routers) are configured in the "bgp" app configuration, although this is redundant and will be removed in future releases.

#### Network Config for Quagga

The script router-deploy.py and router-test.py create a linux-host, configures its interfaces, and instantiates Quagga within. Here's how to change the config for your deployment, using router-deploy.py as an example:

<snip>
    class SDNTopo( Topo ):
    "Sets up control plane components for Router deployment"
    
    def __init__( self, *args, **kwargs ):
        Topo.__init__( self, *args, **kwargs )
     
        # Set up the SDN router's quagga BGP instance i.e the BGP speaker
        # first set the AS number for the SDN router
        sdnAs = 65000

        # Quagga Host (qh) interfaces
        # eth0 interface is used to communicate with ONOS 
        qheth0 = { 'ipAddrs' : ['1.1.1.11/24'] }
        # eth1 interface is used to communicate with peers
        qheth1 = [
            # tagged interface
            { 'vlan': 100,
              'mac':'00:00:00:00:00:01', 
              'ipAddrs' : ['192.168.10.101/24'] },
            # untagged interface 
            { 'mac':'00:00:00:00:00:02', 
              'ipAddrs' : ['192.168.20.101/24'] }
        ]

        qhIntfs = { 'qh-eth0' : qheth0,
                    'qh-eth1' : qheth1 }
        # bgp peer config
        neighbors = [{'address':'192.168.10.1', 'as':65001},
                     {'address':'192.168.20.1', 'as':65002}]
                     
    <snip>


The only parameters that need to be changed are `sdnAs`, `qheth1` and `neighbors`.

Using BGP as a routing protocol requires the configuration of an AS number for the Atrium router. This can be accomplished by assigning one to `sdnAs`. The BGP peer's IP address and AS number are configured in `neighbors`.

The `qheth1` port belongs to the Quagga Linux host and is directly connected to the dataplane switch at the `controlPlaneConnectPoint` port (see `network-cfg.json` above). It is here that we configure the IP/MAC/VLAN interfaces for the Atrium router. Once the script is running and the quagga host has been brought up, one can use the mininet utility to get "in" to the host and check the interfaces.

    admin@atrium16A:~$ ./mininet/util/m qh                                                                                                                                                                                                                                                                                                                                

    root@atrium16A:~# ifconfig
    lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:111 errors:0 dropped:0 overruns:0 frame:0
          TX packets:111 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:6529 (6.5 KB)  TX bytes:6529 (6.5 KB)

    qh-eth0   Link encap:Ethernet  HWaddr 1e:a6:15:7e:7a:d3  
          inet addr:1.1.1.11  Bcast:0.0.0.0  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:16 errors:0 dropped:0 overruns:0 frame:0
          TX packets:14 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1048 (1.0 KB)  TX bytes:1344 (1.3 KB)

    qh-eth1   Link encap:Ethernet  HWaddr 00:00:00:00:00:02  
          inet addr:192.168.20.101  Bcast:0.0.0.0  Mask:255.255.255.0
          inet6 addr: fe80::200:ff:fe00:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:19780 errors:0 dropped:6656 overruns:0 frame:0
          TX packets:13315 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1562663 (1.5 MB)  TX bytes:1036063 (1.0 MB)

    qh-eth1.100 Link encap:Ethernet  HWaddr 00:00:00:00:00:01  
          inet addr:192.168.10.101  Bcast:0.0.0.0  Mask:255.255.255.0
          inet6 addr: fe80::200:ff:fe00:1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:6592 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6554 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:408269 (408.2 KB)  TX bytes:498045 (498.0 KB)




