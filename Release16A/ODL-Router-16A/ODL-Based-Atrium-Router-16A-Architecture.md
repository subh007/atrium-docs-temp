## FlowObjective

Flow Objectives describe an SDN application’s objective (or intention) behind a flow it is sending to a device.

There are only 3 (already implemented in ONOS) :

* Filtering Objective

* Next Objective

* Forwarding Objective


#### FlowObjective Implementation in Opendaylight

![ODL_Architecture](https://www.dropbox.com/s/lukk1ez7ia0lu1u/ODL_Arch_FO.png?dl=1)


* Application communicates the flow installation requirement using flow Objectives.

* Driver translates Flow Objectives to device specific flows.

* DIDM handles device driver registration and communication.


## BGP Application Architecture

![BGP_Application](https://www.dropbox.com/s/tgnyxfj2yp5h55k/bgp_app.png?dl=1)
