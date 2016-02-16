### Features:
**Atrium ODL Based Router 1st Release**
* Supports  BGP routing protocol for IPv4
* Supports VLANs
* Supports multi-table OpenFlow 1.3.4 protocol
* Flow Objectives API & DIDM device drivers
* Tested to work with NoviFlow hardware switches
* Configuration scripts for deploying and testing the Atrium router.

**Atrium ONOS Based Router 2nd Release **
* Continued support of BGP routing protocol
* Experimental support for Quagga IGP stacks, due to Quagga FPM protocol implementation in ONOS
* Improved stability at high-flow counts due to OF and BGP separation
* Improved resilience - data plane communication does not stop upon loss of controller connection
* Support for untagged and tagged interfaces
* Support for static routes
* Tested to work with NoviFlow switches
* Tested to work with Accton/EdgeCore switches using OF-DPA/ONL/ONIE/Indigo switch software stack (included in VM)
* Configuration scripts for deploying and testing the Atrium router.

**Atrium Leaf-Spine Fabric 1st Release**
* Supports subnet configuration on leaf-switches
* Supports L2 switching within subnets on leaf-switches
* Support L3 ECMP routing between subnets via segment-routing on leaf-spine switches
* Tested to work with  Accton switches using OF-DPA/ONL/ONIE/Indigo switch software stack (included in VM)
* Configuration scripts for deploying and testing the Atrium router.
 
 
