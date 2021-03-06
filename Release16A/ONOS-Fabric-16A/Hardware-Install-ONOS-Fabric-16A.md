### Hardware Switches
In principle any hardware switch that supports OF-DPA should work. However, in this release only a specific build of OF-DPA (which we provide) is certified to work for this fabric. In the near future, when OF-DPA 2.0 is GA and publicly available we will release a version of the fabric for the GA code. Right now, we provide a binary that you can load on your switch -- one of [Accton/EdgeCore 5710, 5712 or 6712 models](http://www.edge-core.com/prodcat.asp?c=1). These switches come pre-installed with [ONIE](http://onie.org/) (from the OCP project).

### Software Components to Install on Hardware Switches
The following "Atrium Stack" needs to be installed on the Accton switches.

##### Broadcom's OF-DPA
For this release we use a pre-GA version of Broadcom's OF-DPA code. Details of [OF-DPA can be found here](https://www.broadcom.com/products/Switching/Software-Defined-Networking-Solutions/OF-DPA-Software). More information on the datapath pipeline can be found [here](https://github.com/Broadcom-Switch/of-dpa/tree/master/OF-DPA-2.0). Also take a deeper dive in the [System Architecture] (https://github.com/onfsdn/atrium-docs/wiki/System-Architecture-ONOS-Based-Fabric-16A) page. 

##### Open Network Linux

The OS to install on the Hardware Switch comes from the Open Compute Project. More [details on ONL can be found here](http://opennetlinux.org/).

##### Indigo OpenFlow Agent

We use the [Indigo OF Agent](http://www.projectfloodlight.org/indigo/) from Big Switch Networks, with modifications made by Broadcom to map to the OF-DPA API.

### Installation Guide

1) Connect the switch's management port to your management network (with DHCP server). Power up the switch, ONIE comes pre-installed so you should be able to reach the ONIE prompt. ONIE automatically searches for a DHCP server, and DHCP would have assigned an IP address to the management port.

2) While you can login to the switch using the management-IP assigned to it by your DHCP server, you will lose this capability in a couple of steps. So it required as part of this installation to connect to the console port via a serial line (Baud-rate: 115200, dataBits:8, parity:none, stop-bits:1, flow-control:none).

3) At the ONIE prompt, we need to get and install ONL. Depending on the switch model you are using we need to use different installers from here [[http://opennetlinux.org/binaries/2016.01.04.21.46.18cc76084b3104961117aa8434599c8c67a9a276]]
 
    For powerpc based 5710, use onl-18cc760-powerpc-all.2016.01.04.21.45.installer
    For x86 based 5712 and 6712, use the onl-18cc760-amd64-all.2016.01.04.21.36.installer

    ONIE:/ # wget <installer-url>
    ONIE:/ # sh <installer-filename>

4) The switch should automatically reboot into ONL. At this step you will find that the management IP from step 1 has been lost, which is why console access was required. Use the default _login:_**root** _password:_**onl** to get in.

5)  Open /etc/network/interfaces to configure DHCP or a static-IP for the management port (which is now called ma1).

    auto ma1
    iface ma1 inet static
        address 10.128.10.128
        netmask 255.255.0.0
        gateway 10.128.0.1
        dns-nameservers 192.168.1.1 8.8.8.8

Bring up the ma1 interface with `ifup ma1`

6) Persist your config across reboots

    # persist /etc/shadow; persist /etc/passwd; persist /etc/network/interfaces
    # savepersist /persist/rootfs
    # sync
    # reboot  

7) You should now be able to ssh into the switch.  
Copy over the OFDPA + Indigo debian package from [here](https://github.com/onfsdn/atrium-docs/tree/master/16A/ONOS/builds). The builds are also included in the ONOS based [Atrium Distribution VM](https://github.com/onfsdn/atrium-docs/wiki).

    admin@atrium16A:~$ scp ofdpa/ofdpa-i.12.1.1_12.1.1+accton1.7-1_amd64.deb root@<switch-management-ip-addr>:/mnt/flash2/

Make sure you select the _amd64.deb for the x86 switch models, or _powerpc.deb for the powerpc model. Ensure that the debian package has been saved to the /mnt/flash2/ drive, so that it is persistent through reboots.

8) Install and launch OFDPA 

    # service ofdpa stop
    # dpkg -i --force-overwrite /mnt/flash2/ofdpa-i.12.1.1_12.1.1+accton1.7-1_amd64.deb

9) Launch the OF client

    # brcm-indigo-ofdpa-ofagent --dpid=0x1 --controller=<atrium-vm-eth0-IP> 

10) Done. Consult the [User Guide](https://github.com/onfsdn/atrium-docs/wiki/User-Guide-ONOS-Based-Fabric-16A) for configuration and debugging options.