### Features:
Atrium SDN based Router
* choice of seven different hardware OpenFlow switches
* supports  BGP routing protocol for IPv4
* supports VLANs
* supports multi-table OpenFlow 1.3.4 protocol

Open SDN Distribution VM includes:
* A snapshot of Open Network Operating System (ONOS) SDN controller verified to work with all seven hardware OpenFlow switches.
* A BGP peering application that runs on ONOS and includes the Quagga BGP stack.
* A collection of OpenFlow v1.3 device drivers in ONOS, meant for programming 7 switches with 5 different hardware pipelines.
* A new "Flow Objectives API" in ONOS that allows applications to be written in a pipeline agnostic way.
* A set of configuration scripts for deploying and testing the Atrium router.
* Capability to run the router completely in software for testing, while emulating hardware pipelines with software switches (OVS, CPqD) in Mininet network emulation environment.
* Capability to setup test topologies entirely in software  that include traditional routers and SDN based Atrium routers.
* (Experimental) Automated test suite using the TestON framework for regression testing of the Atrium SDN router.

Open-source Switch software stack for Bare-Metal/White-Box switches certified to work for Atrium SDN router. The stack includes:
* ONL - Open Network Linux (from the Open Compute Project)
* ONIE - Open Network Install Environment (from the Open Compute Project)
* OFDPA - OpenFlow Datapath Abstration v2.0 - a hardware abstraction layer from Broadcom meant for OpenFlow compatibilty
* Indigo OpenFlow client from Project Floodlight
* Accton 5710 (OCP compliant bare-metal switch) - certified to work with Atrium open-source switch software stack for Atrium SDN router.
* Quanta LY2 bare metal switch - certified to work with Atrium open-source switch software stack for Atrium SDN router.
* A set of utility scripts for testing the hardware tables, and instructions for installation and usage.

Hardware switches from following vendors certified for Atrium SDN router
* Centec v350
* Corsa 6410
* Netronome FlowNIC
* NoviFlow 1152
* Pica8 P-3295

Documentation for installation, configuration, and operation for test and deployment
 

### Known Issues:
* Lack of performance testing and optimizations for performance
* Functionality tests are incomplete both for hardware-switch based tests and emulated-hardware tests.
* Lacks support for originating route advertisements i.e the router can only be used in transit
* Lacks support for runtime configuration
* The OFDPA based stack may not recover correctly from switch or controller restarts