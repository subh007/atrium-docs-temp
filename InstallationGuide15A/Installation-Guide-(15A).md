### Distribution VM

To get started with Atrium Release 2015/A, download the distribution VM (Atrium_2015_A.ova) from here: size ~ 2GB

[Atrium 2015/A](https://dl.orangedox.com/TfyGqd73qtcm3lhuaZ/Atrium_2015_A.ova)

**login: admin**

**password: bgprouter**

 
NOTE: This distribution VM is NOT meant for development. Its sole purpose is to have a working system up and running for test/deployment as painlessly as possible. A developer guide using mechanisms other than this VM will be available shortly after the release.

The VM can be run on any desktop/laptop or server with virtualization software (VirtualBox, Parallels, VMWare Fusion, VMWare Player etc.). We recommend using VirtualBox for non-server uses. For running on a server, see the subsection below.

Get a recent version of VirtualBox to import and run the VM. We recommend the following:

1) using 2 cores and at least 4GB of RAM.

2) For networking, you can "Disable" the 2nd Network Adapter. We only need the 1st network adapter for this release.

3) You could choose the primary networking interface (Adapter 1) for the VM to be NATted or "bridged". If you choose to NAT, you would need to create the following port-forwarding rules. The first rule allows you to ssh into your VM, with a command from a Linux or MAC terminal like this: 

`$ ssh -X -p 3022 admin@localhost`

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/natted.png)

The second rule allows you to connect an external switch to the controller running within the VM (the guest-machine) using the IP address of the host-machine (in the example its 10.1.9.140) on the host-port 6633.

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/port-fwd.png)

If you chose to bridge (with DHCP) instead of NAT, then login to the VM to see what IP address was assigned by your DHCP server (on the eth0 interface). Then use ssh to get in to the VM from a terminal:

`$ ssh -X admin@<assigned-ip-addr>`

You can login to the VM with the following credentials --> login: admin, password: bgprouter

Once in, try to ping the outside world as a sanity check (ping www.cnn.com).

### Running the Distribution VM on a Server
The Atrium_2015_A.ova file is simply a tar file containing the disk image (vmdk file) and some configuration (ovf file). Most server virtualization software can directly run the vmdk file. However, most people prefer to run qcow2 format in servers. First untar the ova file

`$ tar xvf Atrium_2015_A.ova`

Use the following command to convert the vmdk file to qcow2. You can then use your server's virtualization software to create a VM using the qcow2 image.

`$ qemu-img convert -f vmdk Atrium_2015_A-disk1.vmdk -O qcow2 Atrium_2015_A-disk1.qcow2`

### Running the Distribution VM on the Switch
While it should be possible to run the controller and other software that is part of the distribution VM directly on the switch CPU in a linux based switch OS, it is not recommended. This VM has not been optimized for such an installation, and it has not been tested in such a configuration. 

### Installation Steps
Once you have the VM up and running, the following steps will help you to bring up the system.

You have two choices:

A) You can bring up the Atrium Router completely in software, and completely self-contained in this VM. In addition, you will get a complete test infrastructure (other routers to peer with, hosts to ping from, etc.) that you can play with (via the router-test.py script). Note that when using this setup, we emulate  hardware-pipelines using software switches. Head over to the "Running Manual Tests" section on the Test Infrastruture page.

B) Or you could bring up the Atrium Router in hardware, working with one of the seven OpenFlow switches we have certified to work for Project Atrium. Follow the directions below:

Basically you need to configure the controller/app, bring up Quagga and connect it to ONOS (via the router-deploy.py script), and then configure the switch you are working with to connect it to the controller - 3 easy steps! The following pages will help you do just that:

Configure and run ONOS
Configure and run Quagga 
Configure and connect your Switch
Accton 5710
Centec v350
Corsa 6410
Netronome
NoviFlow 1132
Pica8 P-3295
Quanta LY2
