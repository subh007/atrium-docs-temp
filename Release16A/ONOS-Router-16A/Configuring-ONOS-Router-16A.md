There are 3 steps in configuring and running ONOS from the distribution VM

1. Use the correct "cell"
1. Create (or copy over) the config in the `~/Applications/config/network-cfg.json` file
1. Use the correct launch script

#### Using the correct Cell

A cell is simply a collection of environment variables used by onos scripts when launching onos. We use these to launch onos for different purposes - test or deployment, fabric or router etc.

There is a single cell provided with the distribution VM for the router: **atrium_router**  
  

    admin@atrium16A:~$ cell atrium_router   <<---- enter the correct cell
    ONOS_CELL=atrium_router
    OCI=127.0.0.1
    OC1=127.0.0.1
    OCN=127.0.0.1
    ONOS_APPS=drivers,openflow,vrouter
    ONOS_GROUP=admin
    ONOS_NIC=127.0.0.*
    ONOS_SCENARIOS=/home/admin/onos/tools/test/scenarios
    ONOS_USER=admin
    ONOS_WEB_PASS=rocks
    ONOS_WEB_USER=onos



#### Create the network-cfg.json file

This is in the ~/Applications/config/ directory of the distribution VM. There are 2 sample network configuration files that have been provided for the fabric
* network-cfg.json.router.hw
* network-cfg.json.router.mn

To configure ONOS, simply copy one of these samples to the network-cfg.json file
    
    admin@atrium16A:~$ cd Applications/config 
    admin@atrium16A:~/Applications/config$ cp network-cfg.json.router.mn network-cfg.json

To test with a hardware switch, use the "router.hw" config file. To test with a software switch  use the "router.mn" config file. Of course when using the "router.hw" config file, you will need to change the config to match your setup for switches and peers. See the [User Guide](https://github.com/onfsdn/atrium-docs/wiki/User-Guide-ONOS-Based-Router-16A) to understand how to change the config. The "router.mn" config file has been created to work with the router-test.py test script, which we describe next.

#### Use the correct launch script

It is easy to launch onos. Simply type

    admin@atrium16A:~$ ok clean

or you may also choose to launch onos within a tmux session as described [here](https://github.com/onfsdn/atrium-docs/wiki/Configure-and-run-ONOS-15A#launching-onos-for-deployment).

Now you can connect the fabric hardware-switches to ONOS (if you configured ONOS for them) as described in the [Hardware Switches](https://github.com/onfsdn/atrium-docs/wiki/Hardware-Install-ONOS-Fabric-16A) section.

Or if you wish to work with software switches, run the following script (either from a different shell from your ONOS shell, or a different window/pane in your tmux session)

    admin@atrium16A:~$ sudo ./fabric-test.py

See the [Software Switches](https://github.com/onfsdn/atrium-docs/wiki/Software-Install-ONOS-Fabric-16A) section to understand what the script does and what you can expect to see in ONOS.
