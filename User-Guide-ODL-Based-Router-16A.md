An all-in-built virtual machine is available  [here](https://github.com/onfsdn/atrium-docs/wiki) to play around with the Opendaylight implementation of the BGP Peering Router use case.  Before getting into the details, first let’s familiarize the main components in the BGP Peering Router. 
* Data Plane Switch (OpenvSwitch or different vendor switches, e.g: Pica8, Noviflow, Accton etc) 
* Opendaylight Controller (aligned to Lithium release) 
* BGP Routing Application 
* DIDM Drivers 
* Flow Objectives API
* Control Plane Switch (OpenvSwitch)  
* Quagga Soft Router 

Following diagram shows how these components fit together to form BGP Router. 
![Atrium BGP Peering Router: Main Components](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/MainComponents.jpg)

