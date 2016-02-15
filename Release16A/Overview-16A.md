![](https://github.com/onfsdn/atrium-docs/blob/master/Atrium-Logo-300x123.jpg)

Project Atrium is an open source SDN distribution -  a vertically integrated set of open source components which together form a complete SDN stack. It’s goals are threefold:  

1. Close the large integration-gap of the elements that are needed to build an SDN stack - while there are multiple choices at each layer, there are missing pieces with poor or no integration.
1. Overcome a massive gap in interoperability - This exists both at the switch level, where existing products from different vendors have limited compatibility, making it difficult to connect an arbitrary switch and controller; and at an API level, where its difficult to write a portable application across multiple controller platforms.
1. Work closely with network operators on deployable use-cases, so that they could download near production quality code from one location, and get started with functioning software defined networks on real hardware. 

The second release from ONF’s Project Atrium is not one but three open-source artifacts.

**Atrium Router with OpenDaylight**

In [2015/A release](https://github.com/onfsdn/atrium-docs/wiki/Introduction-(15A)), we built seven SDN-based, BGP-speaking routers with seven different OpenFlow hardware switches. In the first release, we undoubtedly had a data-plane focus where we wanted to show that the same control plane solution can work across and interoperate with multiple different hardware technologies and switching pipelines. This time around, in keeping with the Atrium philosophy of building vertically integrated stacks with interchangeable parts, we focused on the control plane. The 2016/A release [router is built on OpenDaylight (ODL)](https://github.com/onfsdn/atrium-docs/wiki/ODL-Based-Atrium-Router-16A), controlling hardware OpenFlow switches and using Quagga as the BGP control plane protocol. The ONF has included the same features, namely Flow Objectives and Device Drivers, in the ODL DIDM module that allowed ONOS (in the A release) to work across multiple different OF 1.3 hardware pipelines. 

**Atrium Router with ONOS**

Meanwhile, we continued to gain experience with, and improve the ONOS based Atrium router from the A release. We underwent lab trials at AT&T, Bell Canada and AARnet/CSIRO, and performed interoperability demonstrations at ONS (June'15) and the SDN & OpenFlow World Congress in Dusseldorf (October'15) and the Open Networking Symposium in Bangalore, India (Jan' 16). In this release, we improved the scalability and stability of the [Atrium Router on ONOS](https://github.com/onfsdn/atrium-docs/wiki/ONOS-Based-Atrium-Router-16A), and added experimental support for an IGP (like OSPF or IS-IS) in addition to the existing support of BGP. 

**Atrium Leaf-Spine Fabric**

Finally, in this release,  we have included a new vertically integrated solution for a data-center leaf-spine fabric. This is the first time an L2/L3 Clos Fabric has been built in open-source, on white-box hardware, and with SDN principles. The [Atrium Fabric](https://github.com/onfsdn/atrium-docs/wiki/ONOS-Based-Atrium-Leaf-Spine-Fabric-16A) uses well established design principles of L3 down-to-the-ToR switch, where packets are L2 switched within a rack, and L3 ECMP routed across racks. The fabric is built on EdgeCore bare-metal hardware from the OCP project, and switch software including OCP’s Open Network Linux, and Broadcom’s OF-DPA API. 