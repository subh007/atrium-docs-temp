### Introduction

This is the first release of the Open Daylight based Atrium distribution. The Atrium 2016/A distribution implements an open source router that speaks BGP to other routers, and forwards packets received on one port/vlan to another, based on the next-hop learnt via BGP peering. A BGP peering application for the Open Daylight Controller and a new model for flow objective drivers for switches integrated with the Open Daylight Atrium distribution was developed for this project. The implementation has the same level of feature partity that was introduced by the Atrium 2015/A distribution on the ONOS controller.

An overview of the architecture is available at <>. 

The stack includes a ODL controller with a BGP peering application integrated with an instance of Quagga BGP. As with the previous release, BGP traffic from peers is handed by Quagga. BGP is encapsulated within OpenFlow and sent to the controller, and ODL does the encap/decap to send it to Quagga host via a control-plane OVS. The BGP application internally peers with Quagga using I-BGP, to retrieve the routes Quagga learns via E-BGP communications with peers. It then used the router app to program the dataplane FIB table by using the appropriate switch-driver (implemented as flow objectives) that understands the internal pipeline of the dataplane switch. The flow objectives driver which provides data plane abstraction has implementation for an OVS 2-Table Pipeline reference driver and hardware driver support for [NoviFlow](http://noviflow.com/products/noviswitch/) switches.

The release brings in the framework for vendors looking to develop Open Daylight applications that are interoperable across different open flow switch products. All the code in this Atrium distribution will be integrated into upstream projects like DIDM in the Open Daylight Controller post the Beryllium release that will allow the ODL community developers to develop more applications that leverage the interoperability elements provided by flow objectives.

### Getting Started

Follow the [Installation Guide](https://github.com/onfsdn/atrium-docs/wiki/Installation-Guide-ODL-Based-Router-16A) to configure and launch ODL from the [Atrium_ODL_2016_A.ova](https://github.com/onfsdn/atrium-docs/wiki) distribution VM.

For the data plane, you can either use the hardware switches from NoviFlow (more hardware drivers will be added soon) or if you do not have a hardware-switch, you could get started with a software switch that emulates the hardware-switch pipeline (OVS 2-Table pipeline). Follow the [Guide to developing flow objective drivers for hardware switches](https://github.com/onfsdn/atrium-docs/wiki/Driver-Development-ODL-Based-Router-16A).

### Contributors
* Subhash Kumar Singh, Jayaprakash Kumar, Syed Zubair Ahmed, Priyanka K (Criterion Networks)
* Manoj Nair, Subhash Mondal, Harsh (Wipro Technologies)
* Davide T (Noviflow)
* Saurav Das (ONF)
