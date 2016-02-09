Prerequisites
We are assuming that you have already configured ONOS. We will continue the same running example that we started there.

Generic questions on Quagga should be directed to the Quagga mailing lists. Questions specific to the use of Quagga in Atrium, should be directed to the atrium_eng@groups.opensourcesdn.org mailing list. The same applies for all the other software projects that are part of Atrium.

Configuring Quagga BGP & Connecting to ONOS
Quagga comes with a rich (Cisco-like) CLI. However it is not enough to simply configure Quagga BGP. We also need to configure Zebra, as well as the Linux-host on which Quagga runs. In addition we need to hook up Quagga to ONOS in the control plane - this hookup happens on two different fronts: 1) where BGP communication from the data-plane hardware switch is delivered to ONOS, which in-turn delivers those packets to Quagga (and the reverse communication); and 2) where the ONOS lightweight BGP implementation peers with Quagga BGP using iBGP.

Sounds complicated! But part of Project Atrium's goal is to make life easier for the end user. And so, we have created a script, that you could modify in a couple of places, and launch with one command, that would take care of all the pieces above. If you are really interested in understanding the underlying plumbing, refer to the System Architecture.

The script resides at the top level of the directory struture in the distribution VM: router-deploy.py

Here is what it looks like: