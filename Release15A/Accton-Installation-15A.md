### Prerequisites
We are assuming that you have successfully configured and launched ONOS and Quagga. This section will help you install Atrium's open-source switch software stack - ONL/ONIE/OFDPA/Indigo - on the Accton 5710 and connect it to the control plane stack that you have already installed.

Questions about the Accton 5710 can be directed to Jeff Catlin (jeff_catlin@accton.com). Questions regarding the use of the hardware and the open-source switch software stack as part of Atrium can be directed to the atrium_eng@groups.opensourcesdn.org mailing list. Generic questions on ONL, ONIE and Indigo should be directed to their own mailing lists.

### Configuration
While there are a number of ways to get Open Network Linux (ONL) on your white-box switch, we will follow the procedure outlined below to install the software, and assign a static-IP to the management port, and bring up OFDPA and Indigo.

1) Attach a serial terminal to the switch on the console port (Baud-rate: 115200, dataBits:8, parity:none, stop-bits:1, flow-control:none)

2) Attach the switch's management port to your management network. Power up the switch, and watch your serial terminal - you should find that ONIE comes pre-installed in this switch model (if it does not then contact the switch manufacturer). Note that ONIE comes up in installer mode, and gets an IP address for the management port from the DHCP server in your management network.

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/preboot.jpg]]

3) Ensure that with the IP address assigned by your DHCP server (in the example above I got 10.1.9.153), you can reach the outside world (say ping 8.8.8.8). If you cannot, then there is likely an issue with your management network connectivity or DHCP server configuration. 

4) Now we will install a special version of ONL that comes with OFDPA and Indigo prepackaged for you :) Reboot the switch. As it boots up, on your serial console you should see the a countdown that says "Hit any key to stop autoboot: "  <<-- go ahead and hit it. This should get you to uBoot.

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/uboot.jpg]]

5) Enter the command: `run onie_rescue`

This should get you back into ONIE in rescue mode

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/onie.jpg]]

6) (now in ONIE) install_url http://opennetlinux.org/binaries/2015.06.11.15.18.59.ab0b340d4177839aabb79c52af2416cd45527633/onl-ab0b340-powerpc-all.2015.06.05.22.07.installer

This will download ONL and install it in your switch, and reboot it, and load stuff -- let it go all the way to the end, when you should see the ONL prompt


[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/onl.jpg]]

7) login: root  with password: onl

8) OPTIONAL: you could change the password if you want to:  passwd root

9) Notice that the management interface no longer has an IP address assigned to it. In fact, it has been renamed from "eth0" to "ma1"

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/noip.jpg]]

10) populate /etc/network/interfaces with the static IP address you want

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/netif.jpg]]

11) To persist this configuration across reboots, run the following commands

    # persist /etc/shadow; persist /etc/passwd; persist /etc/network/interfaces
    # savepersist /persist/rootfs
    # cpio -tv < /mnt/flash/persist/rootfs 
    # sync
    # reboot
The cpio command is just a sanity check to see if the right files got persisted. You should see files like shown in the screenshot below:

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/persist.jpg]]


12) Ensure that once the switch reboots and comes back to ONL, you have the right network interfaces, and can ping the outside world. Your network interface configuration should now persist through reboots. 

### Launching OFDPA and Indigo
To launch OFDPA use the following script:

    root@onl-powerpc:~# /etc/init.d/ofdpa start

To launch Indigo, enter the following command (change the dpid to whatever you have configured ONOS to expect, and change the IP address to whatever is your controller IP):

    root@onl-powerpc:~# brcm-indigo-ofdpa-ofagent --dpid=0x000000154d0a0c24 -t 10.1.9.140:6633

After launching if you check: 

    root@onl-powerpc:~# ps aux | grep ofdpa
    root      1298 33.5 16.4 420616 327708 ?       Sl   07:33   0:19 /usr/sbin/ofdpa-powerpc-accton-as5710-54x-r0
    root      1661  0.3  0.2   8336  4332 pts/0    S    07:33   0:00 brcm-indigo-ofdpa-ofagent --dpid=0x000000154d0a0c24 -t 10.1.9.140:6633
    root      2006  0.0  0.0   3780   864 pts/0    S+   07:34   0:00 grep ofdpa

Note that both ofdpa and indigo are up and running!