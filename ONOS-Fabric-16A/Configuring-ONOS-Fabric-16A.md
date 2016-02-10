There are 3 steps in configuring and running ONOS from the distribution VM

1. Use the correct "cell"
1. Create the config in the `Applications/config/network-cfg.json` file
1. Use the correct launch script

#### Using the correct Cell

A cell is simply a collection of environment variables used by onos scripts when launching onos. We use these to launch onos for different purposes - test or deployment, fabric or router etc.

There are 3 cells provided with the distribution VM:
* atrium_fabric
* atrium_fabric_no_learn  
* atrium_router

If you wish to try the fabric with hardware switches, the correct cell is "atrium_fabric_no_learn". 

    admin@atrium16A:~$ cell atrium_fabric_no_learn  <<---- enter the correct cell
    ONOS_CELL=atrium_fabric_no_learn
    OCI=127.0.0.1
    OC1=127.0.0.1
    OCN=127.0.0.1
    ONOS_APPS=drivers,openflow-base,lldpprovider,netcfghostprovider,segmentrouting
    ONOS_GROUP=admin
    ONOS_NIC=127.0.0.*
    ONOS_SCENARIOS=/home/admin/onos/tools/test/scenarios
    ONOS_USER=admin
    ONOS_WEB_PASS=rocks
    ONOS_WEB_USER=onos

In this mode, the the fabric control application does not learn the addresses of the end-hosts (rack-servers). The end-hosts addresses are either configured manually or programmed by an data-center orchestrator (like OpenStack). Currently the fabric lacks the support to automatically learn end-host addresses due to Issue # <>.

If you wish to try the fabric with software switches, you could either use the "atrium_fabric" or "atrium_fabric_no_learn" cells. The difference is that different apps are launched depending on the chosen cell.

    admin@atrium16A:~$ cell atrium_fabric
    ONOS_CELL=atrium_fabric
    OCI=127.0.0.1
    OC1=127.0.0.1
    OCN=127.0.0.1
    ONOS_APPS=drivers,openflow,segmentrouting
    ONOS_GROUP=admin
    ONOS_NIC=127.0.0.*
    ONOS_SCENARIOS=/home/admin/onos/tools/test/scenarios
    ONOS_USER=admin
    ONOS_WEB_PASS=rocks
    ONOS_WEB_USER=onos


