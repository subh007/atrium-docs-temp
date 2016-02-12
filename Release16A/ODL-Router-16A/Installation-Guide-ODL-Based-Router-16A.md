### Distribution VM
To get started with Atrium Release 2015/A, download the distribution VM (Atrium_ODL_2016_A.ova) from here: size ~ 2GB

[link for dropbox](link for dropbox)

login: admin

password: bgprouter


NOTE: This distribution VM is NOT meant for development. Its sole purpose is to have a working system up and running for test/deployment as painlessly as possible. A developer guide using mechanisms other than this VM will be available shortly after the release.

## Installation Steps
Once you have the VM up and running, the following steps will help you to bring up the system.

admin@atrium:~/atrium-odl$ ./distribution-karaf/target/assembly/bin/karaf
opendaylight-user@root>feature:install odl-atrium-all

Configuration file

Known issue

You have two choices:

A) You can bring up the Atrium Router completely in software,  Head over to the "Running Manual Tests" section on the Test Infrastructure page.

B) Or you could bring up the Atrium Router in hardware, working with one of the seven OpenFlow switches we have certified to work for Project Atrium. Follow the directions below:

### Bring up the Atrium Router completely in software
 You can bring up the Atrium Router completely in software, completely self-contained in this VM. In addition, you will get a complete test infrastructure (other routers to peer with, hosts to ping from, etc.) that you can play with (via the router-test.py script). Note that when using this setup, we emulate  hardware-pipelines using software switches.

 Configuration file

### Bring up the Atrium Router completely in hardware

Required change in configuration file
