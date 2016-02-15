At [OpenTechIndia 2016](http://opentechindia.org/), a demonstration was done to show the
BGP peering between Atrium ONOS based stack with Atrium ODL based stack. Following
are the details for the topology and configuration files.

#### Topology

![Demo_1](https://www.dropbox.com/s/38j0i8e3zuojil6/OTI_Demo_1.png?dl=1)

#### Details for AS 100

Controller    : OpenDaylight

DP OF Switch  : OpenVswtich

Driver        : ovs-2TP

Peer AS       : 300, 500

Configuration Files :

* sdnip.json     :   https://www.dropbox.com/s/xtczoibnvilih6u/sdnip.json?dl=1

* addresses.json :   https://www.dropbox.com/s/cm8rr30237eg0c8/address.json?dl=1

Control Plane router deployment script : https://www.dropbox.com/s/rxzry9qokwit3gx/router-deploy.py?dl=1

Data plane switch (configuration) : https://www.dropbox.com/s/wyiu2slkvbjs81q/single-sw.py?dl=1



#### Details for AS 200

Controller : ONOS

OF Switch  : OpenVswtich

Driver     : SoftRouter

Peer AS    : 200, 500


Configuration Files :

* sdnip.json        : https://www.dropbox.com/s/ksea6gk6s42birc/sdnip.json?dl=1

* addresses.json    : https://www.dropbox.com/s/c4rua23oy5vfhp8/addresses.json?dl=1

* routerconfig.json : https://www.dropbox.com/s/1wz2rbaz8ikrcb7/mnrouterconfig.json?dl=1

Control Plane router deployment script : https://www.dropbox.com/s/620dquud07vu2ja/router-deploy.py?dl=1

Data plane switch (configuration)      : https://www.dropbox.com/s/i9x522ud40dn8y0/single-sw.py?dl=1

#### Details for AS 300

Controller    : ONOS

DP OF Switch  : Accton

Driver        : OFDPA

Peer AS       : 100, 200

Configuration Files :

* sdnip.json        : https://www.dropbox.com/s/3ulp0em8nyjo3mb/sdnip.json?dl=1

* addresses.json    : https://www.dropbox.com/s/32nk7qj5r8k966w/addresses.json?dl=1

* routerconfig.json : https://www.dropbox.com/s/dw8n5syzujzgwpg/acctonconfig.json?dl=1

Control Plane router deployment script : https://www.dropbox.com/s/rrnobov2me9f4e4/router-deploy.py?dl=1


#### Details for AS 400 and 500

Configuration file for the Spirent Traffic Generator : https://www.dropbox.com/s/iw4ll4gw9zy7e6j/SDN-India-Atrium-config-hosts-traffic-added.tcc?dl=1
