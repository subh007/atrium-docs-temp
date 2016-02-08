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