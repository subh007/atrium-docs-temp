### Introduction

This is the second release of the ONOS-based Atrium Router. The [previous release](https://github.com/onfsdn/atrium-docs/wiki/Introduction-(15A)#atrium-release-2015a) (2015/A) had an architecture as shown on the left side of the figure below. The current architecture (2016/A) is shown on the right and is supported by hardware from [NoviFlow](http://noviflow.com/products/noviswitch/) and [Accton/EdgeCore](http://www.edge-core.com/prodcat.asp?c=1).

[[https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/router.png]]

There are two major changes:
* In the previous release, BGP traffic from peers (dotted line above) reached Quagga in the control plane in an indirect way. BGP was encapsulated within OpenFlow and sent to the controller, and it was ONOS's responsibility (via the encap/decap app) to shovel it over to the Quagga host via a control-plane OVS. This happened in the reverse direction as well. This led to an interdependence between BGP and OF, which at high flow counts potentially led to stability issues. In this release, the BGP traffic is **redirected in the data-plane** to a port reserved for communication with Quagga directly. By separating the OF and BGP channels, we improve the stability of the router and simplify the architecture.

* The second major change has to do with the way ONOS receives routing information from Quagga in order to program the dataplane FIB table. In the previous release, ONOS had a lightweight implementation of BGP in the controller, and so it could internally peer with Quagga using I-BGP, to retrieve the routes Quagga learns via E-BGP communications with peers.