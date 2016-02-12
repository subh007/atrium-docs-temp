### Distribution VM
To get started with Atrium Release 2016/A for the **ONOS based fabric or router**, download the distribution VM (**Atrium_ONOS_2016_A.ova**) from here: size ~ 2GB

[[https://github.com/onfsdn/atrium-docs/wiki]]

**login: admin**

**password: atriumonos**

NOTE: This distribution VM is NOT meant for development. Its sole purpose is to have a working system up and running for test/deployment as painlessly as possible. 

The VM can be run on any desktop/laptop or server with virtualization software (VirtualBox, Parallels, VMWare Fusion, VMWare Player etc.). We recommend using VirtualBox for non-server uses. For running on a server, see the subsection below.

Get a recent version of VirtualBox to import and run the VM. We recommend the following:

* Using 2 cores and at least 4GB of RAM.

* For networking, you can "Disable" the 2nd Network Adapter. We only need the 1st network adapter for the fabric. However we will need the 2nd adapter in the Atrium router use case, so you may want to keep it.

* You could choose the primary networking interface (Adapter 1) for the VM to be NATted or "bridged". If you NAT, you would need to create the following port-forwarding rules. 
[[https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/pics/rules.png]]

    + The first rule allows you to ssh into your VM, with a command from a Linux or MAC terminal like this: 
        - `$ ssh -X -p 7022 admin@localhost`
    + The second and third rules allows you to connect external switches to the controller running within the VM (the guest-machine) using the IP address of the host-machine (in the example its 10.1.8.48) on the host-ports 6633 or 6653.
    + The final rule allows a browser to access ONOS to display the ONOS GUI.


* If you chose to bridge (with DHCP) instead of NAT, then login to the VM to see what IP address was assigned by your DHCP server (on the eth0 interface). Then use ssh to get in to the VM from a terminal on the host machine:
    + `$ ssh -X admin@<assigned-ip-addr>`

Once in, try to ping the outside world as a sanity check (`ping www.cnn.com`).


**Running the Distribution VM on a Serve**r
The Atrium_ONOS_2016_A.ova file is simply a tar file containing the disk image (vmdk file) and some configuration (ovf file). Most server virtualization software can directly run the vmdk file. However, most people prefer to run qcow2 format in servers. First untar the ova file

`$ tar xvf Atrium_ONOS_2016_A.ova`

Use the following command to convert the vmdk file to qcow2. You can then use your server's virtualization software to create a VM using the qcow2 image.

    $ qemu-img convert -f vmdk Atrium_ONOS_2016_A-disk1.vmdk -O qcow2 Atrium_ONOS_2016_A-disk1.qcow2


### Installation 

Check out the rest of the installation guide for [configuring and running ONOS](https://github.com/onfsdn/atrium-docs/wiki/Configuring-ONOS-Router-16A) and then either using a [NoviFlow or Accton hardware switch](https://github.com/onfsdn/atrium-docs/wiki/Hardware-Install-ONOS-Router-16A) as the router dataplane, or using [a software switch to emulate  hardware](https://github.com/onfsdn/atrium-docs/wiki/Software-Install-ONOS-Router-16A). 