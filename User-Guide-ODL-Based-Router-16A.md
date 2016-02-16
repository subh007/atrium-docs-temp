An all-in-built virtual machine is available  [here](https://github.com/onfsdn/atrium-docs/wiki) to play around with the Opendaylight implementation of BGP Peering Router use case.  Before getting into the details, first let’s familiarize the main components in the BGP Peering Router. 
* Data Plane Switch (OpenvSwitch or different vendor switches, e.g: Pica8, Noviflow, Accton etc) 
* Opendaylight Controller (aligned to Lithium release) 
* BGP Routing Application 
* DIDM Drivers 
* Flow Objectives API
* Control Plane Switch (OpenvSwitch)  
* Quagga Soft Router 

Following diagram shows how these components fit together to form BGP Router. 
![Atrium BGP Peering Router: Main Components](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/MainComponents.jpg)

The data plane switch is the entity uses the flow table entries installed by the BGP Routing Application through SDN controller. In simplest form data plane switch with the installed flows act like a BGP Router. The data plane switches are connected to the SDN controller on the north side and other peer routers on the east west sides. 

Opendaylight SDN controller has many utility applications or plugins which are leveraged by the BGP Router application to manage the control plane information.  In its core Opendaylight SDN controller has a service abstraction layer which is called Model Driven Service Abstraction Layer (MD-SAL). MD-SAL provides a set of common infrastructure services for plugin developers. This include a configuration subsystem manage the life cycle of the plugins, a data store to store the configuration and states , a set of brokers which enable interaction with the data store as well as other plugins. More details of MD-SAL can be found [here](https://wiki.opendaylight.org/view/OpenDaylight_Controller:MD-SAL). The BGP Peering Router application is implemented as a plugin within the Opendaylight run time environment (Apache Karaf container). 

The BGP Routing functionality is split between two components – an application running within the Opendaylight runtime environment and an opensource routing software called Quagga. The split in functionality is purely to differentiate the handling of E-BGP and the I-BGP updates. In other words, the Quagga soft-router receives BGP route updates from peers through E-BGP channel and it notifies the routes to application running on Opendaylight runtime as I-BGP updates. The reason for splitting the BGP functionality between the two components is to reduce the complexity of handling E-BGP routes and associated BGP attributes, and to implement a layer to process the routes and translate them in to the flow rules required by the switches. 

The Device Identification and Driver Management (DIDM) component is one of the active projects in Opendaylight which is used to develop drivers encapsulating south bound device specific functionality. DIDM manages the drivers specific to each data plane switch connected to the controller.  The drivers are created primarily to hide the underlying complexity of the devices and to expose a uniform API to applications. In Atrium Release 2016/A the driver implementation provides a pipeline abstraction and exposes Flow Objectives API. This means applications need to be aware of only the Flow Objectives API without worrying about the Table IDs or the pipelines. 

One more important component in BGP Peering Router implementation is control plane switch. This component is primarily used to connect the Opendaylight SDN controller with the Quagga Soft-Router and establish a path for forwarding EBGP packets to and from Quagga. The BGP Application in Opendaylight has a component by name TunnelingConnectivityManager which forwards the BGP packets between control plan and data plane switch.  There is an obvious question as to why there is no direct connection between the CP and DP switches and why controller is tunneling the BGP packets. While this is the expected behavior that is intended, but controller based tunneling is used to overcome limitations of some of the DP Switch implementation which are currently not able to forward BGP packets. 

Having briefed on the internal components, next we will see how to setup the environment for Atrium BGP Peering Router. 
