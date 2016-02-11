### Introduction

In this section, we describe how to configure the fabric on ONOS using the network-cfg.json file. We will describe ports, devices and hosts configuration. Note that in this example we are assuming that the file network-cfg.json.fabric.nolearn.mn has been copied into network-cfg.json. Where necessary, we will point out differences to other sample config.

#### Configuring Interfaces

In ONOS, applications typically require configuration for external facing ports (external to the SDN island), to interact with the outside world.

    
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


devices

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
