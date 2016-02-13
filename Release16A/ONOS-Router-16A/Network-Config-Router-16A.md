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




