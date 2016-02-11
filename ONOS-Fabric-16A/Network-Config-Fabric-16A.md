### Introduction

In this section, we describe how to configure the fabric on ONOS using the network-cfg.json file. We will describe ports, devices and hosts configuration. Note that in this example we are assuming that the file network-cfg.json.fabric.nolearn.mn has been copied into network-cfg.json. Where necessary, we will point out differences to other sample config.

#### Configuring Interfaces

In ONOS, applications typically require configuration for external facing ports (external to the SDN island), to interact with the outside world. The "ports" config is designed such that multiple "interfaces" can be configured on a single physical port. The port is identified with the switch-dpid/port-number tuple (eg. `of:0000000000000001/3`). For each interface defined on a port, a MAC address, VLAN and a set of IP addresses can be configured.  A few notes: For the fabric application, the MAC address of the interface is ignored, and can be omitted from the config. The fabric currently forwards untagged traffic which is identified by `"vlan": "-1"`. The IP address configuration denotes the IP address of the interface as well as the subnet. For example on port 3 of switch of:1, the subnet is `10.0.1.0/24`, while the IP address of the interface is `10.0.1.254`, which is also the gateway-IP for all the hosts in that subnet. 

Note that the fabric is designed such that the leaf-switch (Top-of-Rack switch) is connected to all the servers in the Rack, and all the servers are in the same subnet. Thus in this example, ports 3 and 4 on switch 0x1, have the same gateway/interface IP and the same subnet prefix.
    
    "ports" : {
	  "of:0000000000000001/3" : {
	     "interfaces" : [
		    {
		      "ips" : [ "10.0.1.254/24" ],
		      "mac" : "00:00:00:00:01:80",
		      "vlan": "-1"
		    }
	     ]
	  },
	  "of:0000000000000001/4" : {
	     "interfaces" : [
	 	    {
		      "ips" : [ "10.0.1.254/24" ],
		      "mac" : "00:00:00:00:01:80",
		      "vlan": "-1"
		    }
	     ]
	  }
    }


### Configuring Devices

    "devices" : {
	  "of:0000000000000001" : {
	        "basic" : {
	          "driver" : "ofdpa-cpqd"
	        },
    	    "segmentrouting" : {
                "name" : "Leaf-R1",
                "nodeSid" : 101,
                "routerIp" : "192.168.0.101",
                "routerMac" : "00:00:00:00:01:80",
                "isEdgeRouter" : true,
                "adjacencySids" : []
            }
    	}
    }


hosts

    "hosts" : {
        "00:00:00:00:00:01/-1" : {
            "basic": {
                "ips": ["10.0.1.1"],
                "location": "of:0000000000000001/3"
            }
         }
    }
