### Prerequisites
We are assuming that you have already configured ONOS. We will continue the same running example that we started there.

Generic questions on Quagga should be directed to the Quagga mailing lists. Questions specific to the use of Quagga in Atrium, should be directed to the atrium_eng@groups.opensourcesdn.org mailing list. The same applies for all the other software projects that are part of Atrium.

### Configuring Quagga BGP & Connecting to ONOS
Quagga comes with a rich (Cisco-like) CLI. However it is not enough to simply configure Quagga BGP. We also need to configure Zebra, as well as the Linux-host on which Quagga runs. In addition we need to hook up Quagga to ONOS in the control plane - this hookup happens on two different fronts: 1) where BGP communication from the data-plane hardware switch is delivered to ONOS, which in-turn delivers those packets to Quagga (and the reverse communication); and 2) where the ONOS lightweight BGP implementation peers with Quagga BGP using iBGP.

Sounds complicated! But part of Project Atrium's goal is to make life easier for the end user. And so, we have created a script, that you could modify in a couple of places, and launch with one command, that would take care of all the pieces above. If you are really interested in understanding the underlying plumbing, refer to the System Architecture.

The script resides at the top level of the directory struture in the distribution VM: router-deploy.py

Here is what it looks like:
![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/bgp.jpg)

The highlighted boxes in the script above show what you need to change for your network:

* Your Atrium Router speaks BGP and so it must have an AS number. Set your AS number in the first box.
* The second box is where you configure the same interface addresses (mac/vlan/ip) for the Atrium router, as you had done when configuring ONOS. Note that there is no need to configure "port" information here. Also note that if you had configured the same mac address for all ports (eg. the RouterMAC case for Pica8 switches), you would need to do the same here.
* The third box is where you configure the peer's (also called neighbor) IP address and AS number. Don't change the last line in this box - it refers to the ONOS iBGP peer.
You may have noticed that step#2 could be done using Linux commands, and steps 1 and 3 can be done using the Quagga CLI. Also, you may have noticed that we did not give the ability to advertise routes (also known as 'networks' in BGP terminology). This is a known issue with the Atrium router implementation, which will be addressed in the next release (actually the code exists in ONOS but it has not been well tested, and so it was left out of the release). For this release, the Atrium router can only be used in transit, ie. it can receive and readvertise routes, but it cannot originate route advertisements itself.

### Launching Quagga
Just like we had shown with the "Launching ONOS" section, we can do this simply by running the command in bash (over ssh) if we just want to run it for test. For deployment, we prefer to use tmux and launch quagga in a seperate tmux window/pane.

In this example, I will create a new "pane" to launch Quagga, in the same tmux "window" where I had launched ONOS.

In bash, listing the current tmux session, should show you the one that was created for launching ONOS.

`$ tmux ls `
`0: 1 windows (created Thu Jun 25 17:39:27 2015) [109x36]`

Attach to the session:

`$ tmux at -t 0`

That should bring up the window where ONOS was launched.

Enter the keys Ctrl-b followed by "  (yes that is the double quotation mark). This would split the window vertically into two panes.

In the lower pane, start the script you just edited in the background

`$ sudo ./router-deploy.py &`

As a sanity check, you should see something like this, if you check for the following

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/mini.jpg)

Here "bgp1" represents the linux-host (container) that runs Quagga. We can enter this host, with the command

`$ ./mininet/util/m bgp1`

Then we can telnet into the Quagga BGP process with 

`$ telnet localhost 2605`
`password: sdnip`

Now we are in the Quagga CLI. To see the status of BGP peering, try

`> show ip bgp summary`

![](https://github.com/onfsdn/atrium-docs/blob/master/15A/pics/pane.jpg)

The screenshot above shows that the iBGP peering session between ONOS (1.1.1.1) and Quagga is up. It also shows that the BGP peering session with the neighbors (peers) is not up. This is of course due to the fact that we haven't connected the dataplane switch to the controller.

You are now ready to connect the switch of your choice to the controller.

