### Overview

The Atrium Router is based on SDN principles of data and control plane separation. The only protocol in use between the switch and the controller is OpenFlow 1.3.4 (designated as a Long Term Stable version of OpenFlow by the ONF). All other protocols communications (like BGP and potentially OSPF, IS-IS etc.) between the Atrium router and neighboring routers, do not go to the controller. Instead they happen directly with Quagga via a dataplane port reserved for this communication. This is different from the Release 2015/A router (and the ODL based router in this release) where the BGP communication is encapsulated within OpenFlow and sent to the controller.

To understand the router and how traffic flows through it, it is necessary to understand OpenFlow 1.3 flows and groups and how they relate to the pipeline of the chosen switch. In this User Guide, we show how to use the [controller CLI and switch tools](https://github.com/onfsdn/atrium-docs/wiki/Utils-Router-16A) to develop an understanding of the control and data traffic flow through the router.

Also since the router is an SDN system, it is unnecessary to configure the dataplane switch, like in traditional networking. Instead all configuration for the router happens in the controller. We take a detailed look at the configuration in the [Network Config](https://github.com/onfsdn/atrium-docs/wiki/Network-Config-Router-16A) page.


