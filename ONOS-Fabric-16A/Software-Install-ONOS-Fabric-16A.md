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

In this example communication between h1 and h2, and between h3 and h4 is being L2 switched (bridged) in the OF-DPA pipeline, emulation the communication within a rack of servers. Communication between racks (h1-h4 or h3-h2 for example) is routed.

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
