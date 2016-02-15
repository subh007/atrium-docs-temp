### FlowObjective

Flow Objectives describe an SDN application’s objective (or intention) behind a flow it is sending to a device.

There are only 3 (already implemented in ONOS) :

* Filtering Objective

* Next Objective

* Forwarding Objective


### FlowObjective Implementation in Opendaylight

![ODL_Architecture](https://www.dropbox.com/s/eu4p4scxts941gy/ODL_Arch.png?dl=1)

Key constructs:

* Application communicates the flow installation requirement using flow Objectives.

* Driver translates Flow Objectives to device specific flows.

* DIDM handles device driver registration and communication.
