### Installation Guide

There is actually nothing to install if you want to work with software switches - everything is included with the distribution VM. We use the [CPqD OF 1.3 Software switch](https://github.com/CPqD/ofsoftswitch13) in the [Mininet](http://mininet.org/) environment to create the Atrium router dataplane, the Quagga instance that is part of Atrium, as well as external routers (also Quagga) and hosts.

Importantly, we use the software switch to emulate the hardware switch, by replicating the same fowarding-table-pipeline  you will use in hardware. For example NoviFlow hardware and the emulation switch both use a 2-table pipeline for the Atrium router. Similarly, the Accton switch and the emulation switch (CPqD) both use the pipeline defined in the [OF-DPA spec](https://github.com/Broadcom-Switch/of-dpa/tree/master/OF-DPA-2.0). This emulation is done with the help of ONOS, where we use "drivers" to talk to different OpenFlow switches.

For the hardware Accton switch using OF-DPA, we use the regular "ofdpa" driver

     <driver name="ofdpa" extends="default"
                manufacturer="Broadcom Corp." hwVersion="OF-DPA.*" swVersion="OF-DPA.*">

For the CPqD switch emulating OF-DPA for the **router**, we use the "ofdpa-cpqd-vlan" driver

     <driver name="ofdpa-cpqd-vlan" extends="default"
                manufacturer="ONF"
                hwVersion="OF1.3 Software Switch from CPqD" swVersion="for Group Chaining">
      
For the hardware NoviFlow switch, as well as the software-switch emulating the NoviFlow hardware, we use the "softrouter" driver.

       <driver name="softrouter" extends="default"
                manufacturer="Various" hwVersion="various" swVersion="0.0.0">


  
Note that when using software switches, ONOS needs to be told which driver to use. This is done via the "devices" configuration in the ~/Applications/config/network-cfg.json file in the Atrium-ONOS distribution VM

### Quick Start

To quickly get started with software switches, follow the recipe below to launch ONOS

    admin@atrium16A:~$ cell atrium_router
    admin@atrium16A:~$ cp Applications/config/network-cfg.json.router.mn Applications/config/network-cfg.json
    admin@atrium16A:~$ ok clean

From a different shell, start the script to launch Mininet with the software-switch, quagga-instances and end-hosts. 


    admin@atrium16A:~$ sudo ./router-test.py

This will bring up the setup shown below

![](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/topoR.png)

There are a few things to note here:
* The picture shows the expected way to deploy the Atrium router with the router-deploy.py script. But the script we just launched (router_test.py) creates everything you see in the picture within the Atrium  Distribution VM.
* The router-test.py script launches the "Dataplane Switch" using the CPqD software switch. Use the "driver" configuration in network-cfg.json to set "softrouter" for NoviFlow emulation, or "ofdpa-cpqd-vlan" for Accton emulation.
* The router-test.py creates 3 physical (dataplane) interfaces on the CPqD switch
    * port 1 is configured 192.168.10.101 and connected to an external router "peer 1" on vlan 100.
    * port 2 is configured 192.168.20.101 and connected to another external router "peer 2" on an untagged interface.
    * **port 3 is a dataplane interface that is used for a different purpose** -- connecting to the Quagga host that "speaks" the control protocols BGP, OSPF etc. See [here](https://github.com/onfsdn/atrium-docs/wiki/Hardware-Install-ONOS-Router-16A#special-requirements-for-hardware-switches) for details.
    * the switch also a management interface over which it connects to ONOS using OpenFlow 1.3

