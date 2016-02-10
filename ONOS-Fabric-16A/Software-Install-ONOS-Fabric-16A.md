### Installation Guide

There is actually nothing to install if you want to work with software switches - everything is included with the distribution VM. We use the [CPqD OF 1.3 Software switch](https://github.com/CPqD/ofsoftswitch13) in the [Mininet](http://mininet.org/) environment to create a leaf-spine fabric.

Importantly, we use the software switch to emulate the hardware switch, by replicating the same fowarding-table-pipeline defined in the [OF-DPA spec](https://github.com/Broadcom-Switch/of-dpa/tree/master/OF-DPA-2.0). This emulation is done with the help of ONOS, where we use "drivers" to talk to different OpenFlow switches.

For hardware switches based on OF-DPA, we use the  

     <driver name="ofdpa" extends="default"
                manufacturer="Broadcom Corp." hwVersion="OF-DPA.*" swVersion="OF-DPA.*">

For software switches emulating OF-DPA, we use the

     <driver name="ofdpa-cpqd" extends="default"
                manufacturer="ONF"
                hwVersion="OF1.3 Software Switch from CPqD" swVersion="for Group Chaining">
        
Note that when using software switches, ONOS needs to be told which driver to use. This is done via the "devices" configuration in the ~/Applications/config/network-cfg.json file in the Atrium-ONOS distribution VM

### Quick Start

To quickly get started with software switches, follow the recipe below to launch ONOS

    admin@atrium16A:~$ cell atrium_fabric_no_learn
    admin@atrium16A:~$ cp Applications/config/network-cfg.json.fabric.nolearn.mn Applications/config/network-cfg.json
    admin@atrium16A:~$ ok clean

From a different shell, start the script to launch Mininet with the software-switches and end-hosts.

    admin@atrium16A:~$ sudo ./fabric-test.py

This is bring up a simple fabric with 2 leaves and 2 spines. Two hosts are connected to each leaf.

<pic>

Try to ping from each host to every other host using the mininet "pingall" command

	mininet> pingall
	*** Ping: testing ping reachability
	h1 -> h2 h3 h4
    h2 -> h1 h3 h4 
	h3 -> h1 h2 h4 
	h4 -> h1 h2 h3 
	*** Results: 0% dropped (12/12 received)

In this example communication between h1 and h2, and between h3 and h4 is being L2 switched (bridged) in the OF-DPA pipeline, emulation the communication within a rack of servers. Communication between racks (h1-h4 or h3-h2 for example) is routed. The best way to what flows have been programmed in the switches, is to use the "flows" command on the ONOS cli.

You can also get "in" to the network-namespace for each host or switch in this network. Use the mininet utility to do so.

	admin@atrium16A:~$ ps aux | grep mininet
	root     13717  0.0  0.0  21164  2156 pts/5    Ss+  01:30   0:00 bash --norc -is mininet:c0
	root     13723  0.0  0.0  21164  2156 pts/6    Ss+  01:30   0:00 bash --norc -is mininet:h1
	root     13727  0.0  0.0  21164  2156 pts/7    Ss+  01:30   0:00 bash --norc -is mininet:h2
	root     13729  0.0  0.0  21164  2156 pts/8    Ss+  01:30   0:00 bash --norc -is mininet:h3
	root     13731  0.0  0.0  21164  2156 pts/9    Ss+  01:30   0:00 bash --norc -is mininet:h4
	root     13733  0.0  0.0  21168  2180 pts/10   Ss+  01:30   0:00 bash --norc -is mininet:leaf1
	root     13739  0.0  0.0  21168  2180 pts/11   Ss+  01:30   0:00 bash --norc -is mininet:leaf2
	root     13744  0.0  0.0  21168  2180 pts/12   Ss+  01:30   0:00 bash --norc -is mininet:spine401
	root     13749  0.0  0.0  21168  2184 pts/13   Ss+  01:30   0:00 bash --norc -is mininet:spine402
	admin    14374  0.0  0.0  11736   932 pts/0    R+   01:33   0:00 grep --color=auto mininet

The example below shows getting in to mininet:h1 and pinging h4

	admin@atrium16A:~$ ./mininet/util/m h1
	root@atrium16A:~# ifconfig
	h1-eth0   Link encap:Ethernet  HWaddr 00:00:00:00:00:01  
    		inet addr:10.0.1.1  Bcast:10.0.1.255  Mask:255.255.255.0
          	UP BROADCAST RUNNING MULTICAST  MTU:1490  Metric:1
          	RX packets:896 errors:0 dropped:840 overruns:0 frame:0
          	TX packets:41 errors:0 dropped:0 overruns:0 carrier:0
          	collisions:0 txqueuelen:1000 
          	RX bytes:72292 (72.2 KB)  TX bytes:3330 (3.3 KB)

	lo      Link encap:Local Loopback  
    		inet addr:127.0.0.1  Mask:255.0.0.0
          	inet6 addr: ::1/128 Scope:Host
          	UP LOOPBACK RUNNING  MTU:16436  Metric:1
          	RX	packets:0 errors:0 dropped:0 overruns:0 frame:0
          	TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          	collisions:0 txqueuelen:0 
          	RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

	root@atrium16A:~# ping 10.0.2.2
	PING 10.0.2.2 (10.0.2.2) 56(84) bytes of data.
	64 bytes from 10.0.2.2: icmp_req=1 ttl=63 time=3.30 ms
	64 bytes from 10.0.2.2: icmp_req=2 ttl=63 time=3.28 ms
	64 bytes from 10.0.2.2: icmp_req=3 ttl=63 time=2.56 ms
	^C
	--- 10.0.2.2 ping statistics ---
	3 packets transmitted, 3 received, 0% packet loss, time 2002ms
	rtt min/avg/max/mdev = 2.569/3.054/3.306/0.349 ms
 