At [OpenTechIndia 2016](http://opentechindia.org/), a demonstration was done to show the
BGP peering between Atrium ONOS based stack with Atrium ODL based stack. Following
are the details for the topology and configuration files.

#### Topology

![Demo_1](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/OTI_Demo_1.png)

#### Details for AS 100

Controller    : OpenDaylight

DP OF Switch  : OpenVswtich

Driver        : ovs-2TP

Peer AS       : 300, 500

Configuration Files :

* [sdnip.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/odl/sdnip.json)

* [addresses.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/odl/address.json)

[Control Plane router deployment script](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/odl/router-deploy.py)

[Data plane switch configuration](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/odl/single-sw.py)



#### Details for AS 200

Controller : ONOS

OF Switch  : OpenVswtich

Driver     : SoftRouter

Peer AS    : 200, 500


Configuration Files :

* [sdnip.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/onos/sdnip.json)

* [addresses.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/onos/addresses.json)

* [routerconfig.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/onos/mnrouterconfig.json)

[Control Plane router deployment script](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/onos/router-deploy.py)

[Data plane switch configuration](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/onos/single-sw.py)

#### Details for AS 300

Controller    : ONOS

DP OF Switch  : Accton

Driver        : OFDPA

Peer AS       : 100, 200

Configuration Files :

* [sdnip.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/accton/sdnip.json)

* [addresses.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/accton/addresses.json)

* [routerconfig.json](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/accton/acctonconfig.json)

[Control Plane router deployment script](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/accton/router-deploy.py)


#### Details for AS 400 and 500

[Configuration file for the Spirent Traffic Generator]( https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/scripts/spirent/SDN-India-Atrium-config-hosts-traffic-added.tcc)
