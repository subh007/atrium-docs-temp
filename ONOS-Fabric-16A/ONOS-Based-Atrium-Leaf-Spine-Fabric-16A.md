### Introduction

In the Atrium 16/A release, we have included experimental support for a new vertically integrated solution for a data-center Leaf-Spine Fabric. This is the first time an L2/L3 Clos Fabric has been built in open-source, on bare-metal hardware, and with SDN principles. The Atrium Fabric is designed to scale up to 16 racks, using well-established design principles of L3 down-to-the-ToR switch, where packets are L2 switched within a rack, and L3 routed across racks. 

[[https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/fabric.png]]

The fabric is built on EdgeCore bare-metal hardware from the Open Compute Project, and switch software including OCP’s Open Network Linux, and Broadcom’s OF-DPA API. It leverages earlier work from the ONF’s SPRING-OPEN project that implemented segment-routing using SDN. 

[[https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/stack.png]]


### Getting Started

Follow the [Installation Guide](https://github.com/onfsdn/atrium-docs/wiki/Installation-Guide-ONOS-Based-Fabric-16A) to configure and launch ONOS from the [Atrium_ONOS_2016_A.ova](https://github.com/onfsdn/atrium-docs/wiki) distribution VM.
For the switches that make up the fabric, you have choice! You could either get started with hardware switches or with software switches. 

In principle any hardware switch that supports OF-DPA should work. However, in this release only a specific build of OF-DPA is certified to work for this fabric. In the near future, when OF-DPA 2.0 is GA and publicly available we will release a version of the fabric for the GA code. Right now, we provide a binary that you can load on your switch -- one of Accton/EdgeCore 5710, 5712 or 6712 models. Follow the directions to [download and install Atrium on your hardware switch](https://github.com/onfsdn/atrium-docs/wiki/Software-Install-ONOS-Fabric-16A).

If you don't have a hardware-switch, you could get started with a software switch that emulates the hardware-switch pipeline (ie. the OF-DPA pipeline). Follow the guide to [get started with software-switches](https://github.com/onfsdn/atrium-docs/wiki/Software-Install-ONOS-Fabric-16A).

Learn more about how the fabric works in the [System Architecture](https://github.com/onfsdn/atrium-docs/wiki/System-Architecture-ONOS-Based-Fabric-16A) section.


