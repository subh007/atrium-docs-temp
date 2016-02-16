

## BGP Application Architecture

![BGP_Application](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/bgp_app.png)

## FlowObjective

Flow Objectives describe an SDN application’s objective (or intention) behind a flow it is sending to a device.

There are only 3 (already implemented in ONOS) :

* Filtering Objective

* Next Objective

* Forwarding Objective


#### FlowObjective Implementation in Opendaylight

![ODL_Architecture](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/ODL_ARCH_FO.png)


* Application communicates the flow installation requirement using flow Objectives.

* Driver translates Flow Objectives to device specific flows.

* DIDM handles device driver registration and communication.
