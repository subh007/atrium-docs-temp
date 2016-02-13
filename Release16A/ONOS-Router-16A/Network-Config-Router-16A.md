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
