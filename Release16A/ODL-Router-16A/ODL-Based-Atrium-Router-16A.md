### Introduction

This is the first release of the Open Daylight based Atrium Router. This release introduces a new BGP peering application for the Open Daylight Controller and also introduces the model for implementing flow objectives drivers for switches integrated with Open Daylight Atrium distribution. An overview of the architecture is available at <>. The Atrium Open Daylight distribution currently has hardware support from [NoviFlow](http://noviflow.com/products/noviswitch/).

<edit>

There are two major changes:
* In the previous release, BGP traffic from peers (dotted line above) reached Quagga in the control plane in an indirect way. BGP was encapsulated within OpenFlow and sent to the controller, and it was ONOS's responsibility (via the encap/decap app) to shovel it over to the Quagga host via a control-plane OVS. This happened in the reverse direction as well. This led to an interdependence between BGP and OF, which at high flow counts potentially led to stability issues. In this release, the BGP traffic is **redirected in the data-plane** to a port reserved for communication with Quagga directly. By separating the OF and BGP channels, we improve the stability of the router and simplify the architecture.

* The second major change has to do with the way ONOS receives routing information from Quagga in order to program the dataplane FIB table. In the previous release, ONOS had a lightweight implementation of BGP in the controller, and so it could internally peer with Quagga using I-BGP, to retrieve the routes Quagga learns via E-BGP communications with peers. It then used the Router app to program the dataplane FIB table by using the appropriate switch-driver that understands the internal pipeline of the dataplane switch. While I-BGP was an efficient mechanism to pull routes out of Quagga it could not be used to pull routes learnt by any other routing protocol such as an IGP (like OSPF or IS-IS). Fortunately, Quagga (or rather Zebra) includes a different mechanism ([Fib Push Interface](http://www.nongnu.org/quagga/docs/docs-info.html#zebra-FIB-push-interface)) to push the best route learnt by all Quagga protocols to an external server. In this release, **an FPM server was created in ONOS**, so it could receive the best routes from Zebra and program the dataplane switch. By implementing the FPM protocol, this release paves the path for the Atrium router to potentially support all routing protocols implemented by Quagga.

In addition to the two major changes, the ability to support both vlan-tagged and untagged interfaces was added, as was the ability to support static routes. Finally, the router in this release, once programmed, does not suspend dataplane traffic forwarding upon temporary loss of communication with the controller.

### Getting Started

Follow the [Installation Guide](https://github.com/onfsdn/atrium-docs/wiki/Installation-Guide-ODL-Based-Router-16A) to configure and launch ODL from the [Atrium_ODL_2016_A.ova](https://github.com/onfsdn/atrium-docs/wiki) distribution VM.

For the dataplane switch, you can use [hardware switches from either NoviFlow]

If you do not have a hardware-switch, you could get started with a software switch that emulates the hardware-switch pipeline (ie. the OVS 2-Table pipeline). Follow the guide to [Guide to developing device drivers for hardware switches](https://github.com/onfsdn/atrium-docs/wiki/Driver-Development-ODL-Based-Router-16A).

### Contributors
* Subhash Kumar Singh, Jayaprakash Kumar, Syed Zubair Ahmed, Priyanka K (Criterion Networks)
* Manoj Nair, Subhash Mondal, Harsh (Wipro Technologies)
* Davide T (Noviflow)
* Saurav Das (ONF)
