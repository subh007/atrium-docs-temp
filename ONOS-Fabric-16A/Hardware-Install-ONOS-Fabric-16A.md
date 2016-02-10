### Hardware Switches
In principle any hardware switch that supports OF-DPA should work. However, in this release only a specific build of OF-DPA is certified to work for this fabric. In the near future, when OF-DPA 2.0 is GA and publicly available we will release a version of the fabric for the GA code. Right now, we provide a binary that you can load on your switch -- one of [Accton/EdgeCore 5710, 5712 or 6712 models](http://www.edge-core.com/prodcat.asp?c=1). These switches come pre-installed with ONIE (from the OCP project).

### Software Components to Install on Hardware Switches
The following software needs to be installed on the Accton switches.

##### Broadcom's OF-DPA
For this release we use a pre-GA version of Broadcom's OF-DPA code. Details of [OF-DPA can be found here](https://www.broadcom.com/products/Switching/Software-Defined-Networking-Solutions/OF-DPA-Software). More information on the datapath pipeline can be found [here](https://github.com/Broadcom-Switch/of-dpa/tree/master/OF-DPA-2.0). Also take a deeper dive in the [System Architecture] (https://github.com/onfsdn/atrium-docs/wiki/System-Architecture-ONOS-Based-Fabric-16A) page. 

##### Open Network Linux

The OS to install on the Hardware Switch comes from the Open Compute Project. More [details on ONL can be found here](http://opennetlinux.org/).

##### Indigo OpenFlow Agent

We use the [Indigo OF Agent](http://www.projectfloodlight.org/indigo/) from Big Switch Networks, with modifications made by Broadcom to map to OF-DPA API.

### Installation Guide
