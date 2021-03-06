### Introduction

This is the first release of the Open Daylight based Atrium distribution. The Atrium 2016/A distribution implements an open source router that speaks BGP to other routers, and forwards packets received on one port/vlan to another, based on the next-hop learnt via BGP peering. A BGP peering application for the Open Daylight Controller and a new model for flow objective drivers for switches integrated with the Open Daylight Atrium distribution was developed for this project. The implementation has the same level of feature partly that was introduced by the Atrium 2015/A distribution on the ONOS controller.

In its simplest form the BGP Peering Router is an open source Router which speaks BGP with other routers and builds control plane information for routing packets. This control plane information is configured in the data plane switches from different vendors using Openflow 1.3 protocol. The SDN controller platform helps in this provisioning by providing platform services to interact with the providers or plugins which implement the Openflow interface.

There are few minor differences between the ONOS and ODL variants of the BGP Router application, mainly arising due to the architectural differences between two controllers as well as the maturity of few dependent projects. These aspects are covered in detail in other sections, but to summarize following are the key differences  

* The driver layer and flow objectives interfaces are implemented leveraging the DIDM project in Opendaylight.
* BGP application leverages the Yang modelling methodology in Opendaylight and uses this for exposing the components APIs and services. Similarly Opendaylight also implements a layer above Karaf called config-subsystem to manage the lifecycle of the application features. This is one additional difference which required consideration while porting from ONOS.
* Opendaylight implementation provides a RESTCONF interface to update BGP Router configuration, which is enabled through the built in RESTCONF plugin in Opendaylight.
* ARP and ICMP packet handling in the Opendaylight based BGP Application is different from ONOS implementation, mainly due to lack of reusable applications or services handling this functionality in Opendaylight.  
* In Opendaylight the intent based interface is under development and this is not integrated with the BGP application or the driver layer to enable an abstract interface. In contrast, the ONOS implementation had a rich set of intents as well as Karaf CLIs to verify various data points and configurations.


An overview of the architecture is available at [here](https://github.com/onfsdn/atrium-docs/wiki/ODL-Based-Atrium-Router-16A-Architecture). The stack includes a ODL controller with a BGP peering application integrated with an instance of Quagga BGP. As with the previous release, BGP traffic from peers is handed by Quagga. BGP is encapsulated within OpenFlow and sent to the controller, and ODL does the encap/decap to send it to Quagga host via a control-plane OVS. The BGP application internally peers with Quagga using I-BGP, to retrieve the routes Quagga learns via E-BGP communications with peers. It then used the router app to program the dataplane FIB table by using the appropriate switch-driver (implemented as flow objectives) that understands the internal pipeline of the dataplane switch. The flow objectives driver which provides data plane abstraction has implementation for an OVS 2-Table Pipeline reference driver and hardware driver support for [NoviFlow](http://noviflow.com/products/noviswitch/) switches.

The release brings in the framework for vendors looking to develop Open Daylight applications that are interoperable across different open flow switch products. All the code in this Atrium distribution will be integrated into upstream projects like DIDM in the Open Daylight Controller post the Beryllium release that will allow the ODL community developers to develop more applications that leverage the interoperability elements provided by flow objectives.

A demonstration of the ODL Atrium distribution was done recently at OpenTechIndia, to show the BGP peering between Atrium ONOS based stack and the Atrium ODL based stack. More details on the same can be found [here](https://github.com/onfsdn/atrium-docs/wiki/OpenTechIndia-Demo).

### Getting Started

Follow the [Installation Guide](https://github.com/onfsdn/atrium-docs/wiki/Installation-Guide-ODL-Based-Router-16A) to configure and launch ODL from the [Atrium_ODL_2016_A.ova](https://github.com/onfsdn/atrium-docs/wiki) distribution VM.

For the data plane, you can either use the hardware switches from NoviFlow (more hardware drivers will be added soon) or if you do not have a hardware-switch, you could get started with a software switch that emulates the hardware-switch pipeline (OVS 2-Table pipeline). Follow the [Guide to developing flow objective drivers for hardware switches](https://github.com/onfsdn/atrium-docs/wiki/Driver-Development-ODL-Based-Router-16A).

### Contributors
* Subhash Kumar Singh, Jayaprakash Kumar, Syed Zubair Ahmed, Priyanka K (Criterion Networks)
* Manoj Nair, Subhash Mondal, Harsh (Wipro Technologies)
* Davide T (Noviflow)
* Saurav Das (ONF)
