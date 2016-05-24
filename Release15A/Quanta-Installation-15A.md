Prerequisites
We are assuming that you have successfully configured and launched ONOS and Quagga. This section will help you install Atrium's open-source switch software stack - ONL/ONIE/OFDPA/Indigo - on the Quanta LY2 and connect it to the control plane stack that you have already installed.

We currently do not have any support for questions on Quanta LY2 hardware. It can be purchased from: http://www.colfaxdirect.com/store/pc/viewPrd.asp?idproduct=2170&idcategory=0

Questions regarding the use of the hardware and the open-source switch software stack as part of Atrium can be directed to the atrium_eng@groups.opensourcesdn.org mailing list. Generic questions on ONL, ONIE and Indigo should be directed to their own mailing lists.

Configuration
The configuration steps to follow for the LY2 are exactly the same as those for the Accton 5710.

Launching OFDPA and Indigo
The launch process is also identical to the Accton. 

After launching if you check:

 root@onl-powerpc:~# ps aux | grep ofdpa

root     11198  5.2 11.0 293692 229628 ?       Sl   17:01   0:40 /usr/sbin/ofdpa-powerpc-quanta-ly2-r0
root     20520  1.3  0.2   8336  4328 pts/0    S    17:13   0:00 brcm-indigo-ofdpa-ofagent --dpid=0x000000154d0a0c24 -t 10.1.9.140:6633
root     20580  0.0  0.0   3776   864 pts/0    S+   17:13   0:00 grep ofdpa
 You will find that a different OFDPA binary is being used for the LY2, but the same ofagent works on both.
