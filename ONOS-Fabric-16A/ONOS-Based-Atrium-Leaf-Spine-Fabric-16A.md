### Introduction

In the Atrium 16/A release, we have included experimental support for a new vertically integrated solution for a data-center Leaf-Spine Fabric. This is the first time an L2/L3 Clos Fabric has been built in open-source, on bare-metal hardware, and with SDN principles. 

The Atrium Fabric is designed to scale up to 16 racks, using well-established design principles of L3 down-to-the-ToR switch, where packets are L2 switched within a rack, and L3 routed across racks. The fabric is built on EdgeCore bare-metal hardware from the Open Compute Project, and switch software including OCP’s Open Network Linux, and Broadcom’s OF-DPA API. It leverages earlier work from the ONF’s SPRING-OPEN project that implemented segment-routing using SDN. 

