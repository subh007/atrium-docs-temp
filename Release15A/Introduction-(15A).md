![](https://github.com/onfsdn/atrium-docs/blob/master/Atrium-Logo-300x123.jpg)

Project Atrium develops an open SDN Distribution -  a vertically integrated set of open source components which together form a complete SDN stack.

### Motivation
The current state of SDN technology suffers from two significant gaps that interfere with the development of a vibrant ecosystem. First, there is a **large gap in the integration** of the elements that are needed to build an SDN stack. While there are multiple choices at each layer, there are missing pieces and poor or no integration.

Second, there is a **gap in interoperability**. This exists both at a product level, where existing products from different vendors have limited compatibility, and at a protocol level, where interfaces between the layers are either over or under specified. For example, differences in the implementations of OpenFlow v1.3 makes it difficult to connect an arbitrary switch and controller. On the other hand, the interface for writing applications on top of a controller platform is mostly under specified, making it difficult to write a portable application. Project Atrium attempts to address these challenges, not by working in the specification space, but by generating code that integrates a set of production quality components. Their successful integration then allows alternative component instances (switches, controllers, applications) to be integrated into the stack.

Most importantly we wish to **work closely with network operators** on deployable use-cases, so that they could download near production quality code from one location, and trial functioning software defined networks on real hardware. **Atrium is the first fully open source SDN distribution** (akin to a Linux distribution). We believe that with operator input, requirements and deployment scenarios in mind, Atrium can be useful as distributed, while also providing the basis for future extensions and alternative distributions which focus on requirements different from those in the original release.

### Atrium Release 2015/A
In the first release (2015/A), Atrium is quite simply an open-source router that speaks BGP to other routers, and forwards packets received on one port/vlan to another, based on the next-hop learnt via BGP peering.

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/router.jpg)

Atrium creates a vertically-integrated stack to produce an SDN based router. This stack can have the forms shown below.

On the left, the stack includes a controller (ONOS) with a peering application (called BGP Router) integrated with an instance of [Quagga BGP](http://www.nongnu.org/quagga/index.html). The controller also includes a device-driver specifically written to control an OF-DPA based OpenFlow switch (more specifially it is meant to be used with OF-DPA v2.0). The controller uses OpenFlow v1.3.4 to communicate with the hardware switch. The hardware switch can be a bare-metal switch from either Accton (5710) or from Quanta (LY2). On the bare-metal switch we run an open switch operating system (ONL) and an open install environment (ONIE) from the Open Compute Project. In additon, we run the Indigo OpenFlow Agent on top of OF-DPA, contributed by Big Switch Networks and Broadcom. 

On the right, the control plane stack remains the same. The one change is that we use a different device-driver in the controller, depending on the vendor equipment we work with. Currently Atrium release 2015/A works with equipment from Noviflow (1132), Centec (v350), Corsa (6410), Pica8 (P-3295), and Netronome. The vendor equipment exposes the underlying switch capabilties necessary for the peering application via an OpenFlow agent (typically OVS) to the control plane stack.

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/atrium-1.jpg)    ![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/atrium-2.jpg)
![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/atrium-rack.jpg)

Details can be found in the Atrium 2015/A release contents. 