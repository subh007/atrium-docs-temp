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


#### Configuring Devices

For the fabric, each individual switch must be configured. In this example, we show a single switch with dpid 0x1.  
First up there is the `"basic"` configuration, which tells ONOS the "driver" that should be loaded for this switch when it connects to ONOS. Driver configuration is **necessary** for software switches. In this example, the driver is the `ofdpa-cpqd` driver. When using hardware switches, one can eliminate this config, or assign the driver `ofdpa`.

Next, for the fabric application, we need to make the `"segmentrouting"` configuration for each switch. This is because the fabric derives from earlier work in the [ONF's SPRING-OPEN project](https://wiki.onosproject.org/display/ONOS/Archived+Content%3A+ONF%27s+SPRING-OPEN+project).   
The `"name"` can be anything you want but it is prudent to name the switch after the function it is performing. In this example, the switch 0x1 is a Leaf Switch in Rack 1, `"Leaf-R1"`. Next identify this switch with a globally-unique MPLS label, under `"NodeSid"`. In this example, label `101` has been configured - this label is used for routing in the fabric. Next, assign an IP address and MAC address to this switch/router's loopback interface, using `"routerIp"` and `"routerMac"` - of course both should be globally unique and all the router-IPs should be in a subnet different from the subnets configured for the racks (servers). Finally all ToRs (leafs) should have `"isEdgeRouter"` assigned `true`, and all spines should configure this parameter `false`. Leave the `"adjacencySids"` blank - we do not use this SR functionality in the fabric.

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


#### Configuring Hosts

End host config (servers) is **necessary** when using hardware switches. This is because in this release the ofdpa lacks the functionality to "learn" the location and addresses of end-hosts (Issue #). When using software switches, end-host configuration is necessary when in "no-learn" mode (see [here](https://github.com/onfsdn/atrium-docs/wiki/Configuring-ONOS-Fabric-16A)), but should be omitted when in learning mode.

Host config identifies hosts via the MAC-address/VLAN-id tuple. In the example below, the MAC address of the host-server is `"00:00:00:00:00:01"` and the vlan-id is `-1` for untagged. The `basic` host config includes a set of IP addresses for the host, and the location of the server with respect to the switch it connects to - in this example, the host is connected to switch 0x1 of physical port 3.

    "hosts" : {
        "00:00:00:00:00:01/-1" : {
            "basic": {
                "ips": ["10.0.1.1"],
                "location": "of:0000000000000001/3"
            }
         }
    }
