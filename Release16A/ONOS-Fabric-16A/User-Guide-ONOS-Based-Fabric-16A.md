### Overview

The Atrium Fabric is based on SDN principles of data and control plane separation. We do not use any of the control plane protocols found in traditional networking. In other words we do not use OSPF, BGP, STP, MSTP, LAG, MLAG, VLANs etc. The only protocol in use is OpenFlow 1.3.4 (designated as a Long Term Stable version of OpenFlow by the ONF).

As a result, to understand the fabric and how traffic flows through the fabric, it is necessary to understand OpenFlow 1.3 flows and groups and how they relate to the OF-DPA pipeline. The [System Architecture](https://github.com/onfsdn/atrium-docs/wiki/System-Architecture-ONOS-Based-Fabric-16A) gives more detail on the traffic flow through the pipeline for leaf and spine switches. In this User Guide, we show how to use the [controller CLI](https://github.com/onfsdn/atrium-docs/wiki/ONOS-Utils-Fabric-16A) and [switch tools](https://github.com/onfsdn/atrium-docs/wiki/Switch-Utils-Fabric-16A) to develop an understanding of the traffic flow through the fabric.

Also since the fabric is an SDN system, it is unnecessary to configure the switches individually, like in traditional networking. Instead all configuration for the fabric happens in the controller. We take a detailed look at the configuration in the [Network Config](https://github.com/onfsdn/atrium-docs/wiki/Network-Config-Fabric-16A) page.



