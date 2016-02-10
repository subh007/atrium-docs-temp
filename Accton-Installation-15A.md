### Prerequisites
We are assuming that you have successfully configured and launched ONOS and Quagga. This section will help you install Atrium's open-source switch software stack - ONL/ONIE/OFDPA/Indigo - on the Accton 5710 and connect it to the control plane stack that you have already installed.

Questions about the Accton 5710 can be directed to Jeff Catlin (jeff_catlin@accton.com). Questions regarding the use of the hardware and the open-source switch software stack as part of Atrium can be directed to the atrium_eng@groups.opensourcesdn.org mailing list. Generic questions on ONL, ONIE and Indigo should be directed to their own mailing lists.

### Configuration
While there are a number of ways to get Open Network Linux (ONL) on your white-box switch, we will follow the procedure outlined below to install the software, and assign a static-IP to the management port, and bring up OFDPA and Indigo.

1. Attach a serial terminal to the switch on the console port (Baud-rate: 115200, dataBits:8, parity:none, stop-bits:1, flow-control:none)

2. Attach the switch's management port to your management network. Power up the switch, and watch your serial terminal - you should find that ONIE comes pre-installed in this switch model (if it does not then contact the switch manufacturer). Note that ONIE comes up in installer mode, and gets an IP address for the management port from the DHCP server in your management network.

[[https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/preboot.jpg]]

