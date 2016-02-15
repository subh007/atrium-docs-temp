### Topology

![Demo_1](https://www.dropbox.com/s/38j0i8e3zuojil6/OTI_Demo_1.png?dl=1)

![Demo_2](https://www.dropbox.com/s/oii5gw88qjchzcb/OTI_Demo_2.png?dl=1)

#### Details for AS 100

Controller    : OpenDaylight

DP OF Switch  : OpenVswtich

Driver        : ovs-2TP

Peer AS       : 300, 500

Control Plane router deployment script : https://www.dropbox.com/s/rxzry9qokwit3gx/router-deploy.py?dl=1

Configuration File :

sdnip.json     :   https://www.dropbox.com/s/xtczoibnvilih6u/sdnip.json?dl=1
addresses.json :   https://www.dropbox.com/s/cm8rr30237eg0c8/address.json?dl=1

Data plane switch (configuration) :



#### Details for AS 200

Controller : ONOS
OF Switch  : OpenVswtich
Driver     : SoftRouter
Peer AS    : 200, 500
Control Plane router deployment script :
Configuration File :
          sdnip.json        :
          addresses.json    : https://www.dropbox.com/s/c4rua23oy5vfhp8/addresses.json?dl=1
          routerconfig.json :
Data plane switch (configuration) :

#### Details for AS 300

Controller    : ONOS
DP OF Switch  : Accton
Driver        : OFDPA
Peer AS       : 00, 200
Control Plane router deployment script :
Configuration File :
          sdnip.json        :
          addresses.json    :
          routerconfig.json :
Data plane switch (configuration) :


#### Details for AS 400 and 500
Configuration file for the Spirent Traffic Generator
