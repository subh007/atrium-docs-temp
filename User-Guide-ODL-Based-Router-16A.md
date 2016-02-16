An all-in-built virtual machine is available  [here](https://github.com/onfsdn/atrium-docs/wiki) to play around with the Opendaylight implementation of BGP Peering Router use case.  Before getting into the details, first let’s familiarize the main components in the BGP Peering Router. 
* Data Plane Switch (OpenvSwitch or different vendor switches, e.g: Pica8, Noviflow, Accton etc) 
* Opendaylight Controller (aligned to Lithium release) 
* BGP Routing Application 
* DIDM Drivers 
* Flow Objectives API
* Control Plane Switch (OpenvSwitch)  
* Quagga Soft Router 

Following diagram shows how these components fit together to form BGP Router. 
![Atrium BGP Peering Router: Main Components](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/MainComponents.jpg)

The data plane switch is the entity uses the flow table entries installed by the BGP Routing Application through SDN controller. In simplest form data plane switch with the installed flows act like a BGP Router. The data plane switches are connected to the SDN controller on the north side and other peer routers on the east west sides. 

Opendaylight SDN controller has many utility applications or plugins which are leveraged by the BGP Router application to manage the control plane information.  In its core Opendaylight SDN controller has a service abstraction layer which is called Model Driven Service Abstraction Layer (MD-SAL). MD-SAL provides a set of common infrastructure services for plugin developers. This include a configuration subsystem manage the life cycle of the plugins, a data store to store the configuration and states , a set of brokers which enable interaction with the data store as well as other plugins. More details of MD-SAL can be found [here](https://wiki.opendaylight.org/view/OpenDaylight_Controller:MD-SAL). The BGP Peering Router application is implemented as a plugin within the Opendaylight run time environment (Apache Karaf container). 

The BGP Routing functionality is split between two components – an application running within the Opendaylight runtime environment and an opensource routing software called Quagga. The split in functionality is purely to differentiate the handling of E-BGP and the I-BGP updates. In other words, the Quagga soft-router receives BGP route updates from peers through E-BGP channel and it notifies the routes to application running on Opendaylight runtime as I-BGP updates. The reason for splitting the BGP functionality between the two components is to reduce the complexity of handling E-BGP routes and associated BGP attributes, and to implement a layer to process the routes and translate them in to the flow rules required by the switches. 

The Device Identification and Driver Management (DIDM) component is one of the active projects in Opendaylight which is used to develop drivers encapsulating south bound device specific functionality. DIDM manages the drivers specific to each data plane switch connected to the controller.  The drivers are created primarily to hide the underlying complexity of the devices and to expose a uniform API to applications. In Atrium Release 2016/A the driver implementation provides a pipeline abstraction and exposes Flow Objectives API. This means applications need to be aware of only the Flow Objectives API without worrying about the Table IDs or the pipelines. 

One more important component in BGP Peering Router implementation is control plane switch. This component is primarily used to connect the Opendaylight SDN controller with the Quagga Soft-Router and establish a path for forwarding EBGP packets to and from Quagga. The BGP Application in Opendaylight has a component by name TunnelingConnectivityManager which forwards the BGP packets between control plan and data plane switch.  There is an obvious question as to why there is no direct connection between the CP and DP switches and why controller is tunneling the BGP packets. While this is the expected behavior that is intended, but controller based tunneling is used to overcome limitations of some of the DP Switch implementation which are currently not able to forward BGP packets. 

Having briefed on the internal components, next we will see how to setup the environment for Atrium BGP Peering Router. 

##Environment Setup##

Following diagram shows a reference environment to verify the Atrium BGP Peering Application. 
![User Test Environment](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/UserTestEnvironment.jpg)

The whole setup shown above, except for the Opendaylight controller, can be brought up using a script available in the VM image distributed along with the release.  There are two scripts which can be used router-test.py and router-deploy.py. These two scripts are available in the root directory of the distribution VM. These scripts uses mininet python libraries to setup the topology shown above including the specific addresses used here. 

The green boxes shown above are Linux containers which the above script brings up with Quagga soft-router within it. The connectivity/Links between the network elements (Peers , OVS, SDN controller, Hosts etc) is established using the mininet python APIs.  The difference between these two scripts is that router-test.py brings up the whole topology including the peers (except SDN controller) , where as router-deploy.py brings up only the components specific to Atrium BGP Peering Router , i.e. CP Switch and Quagga BGP Soft-Router (without DP Switch and the Peering Routers).  Note that the second option is used to test a specific vendor hardware switch where in the DP switch will be connected externally to the box running ODL, CP Switch and Quagga. 

Next we will see how to start each of these components and verify the functionality. 

## Starting Opendaylight Controller ##

Prerequisites : 
•	JDK 1.8 
•	Maven 3.3.x
Before starting Opendaylight it is recommended to do the following 

Set MAVEN_HOME environment variable . To setup the environment variable, first type on your shell prompt mvn –version which gives and output as follows 

    admin@atrium:~/controller/atrium-karaf-1.0-SNAPSHOT/bin$ mvn --version
    Apache Maven 3.3.1 (cab6659f9874fa96462afef40fcf6bc033d58c1c; 2015-03-13T13:10:27-07:00)
    Maven home: /home/admin/Applications/apache-maven-3.3.1
    Java version: 1.8.0_45, vendor: Oracle Corporation
    Java home: /usr/lib/jvm/java-8-oracle/jre
    Default locale: en_US, platform encoding: UTF-8

To set the MAVEN_HOME type the following command (in single line)
    admin@atrium:~/controller/atrium-karaf-1.0-SNAPSHOT/bin$ export MAVEN_HOME=/home/admin/Applications/apache-maven-3.3.1

If you are running this distribution VM behind a proxy (in your corporate network), please ensure that you add the proxy details in the maven settings file located at $MAVEN_HOME/conf/settings.xml. 
Without this step the karaf runtime fail to download required dependencies which are not included in the distribution and the Opedaylight karaf startup command may hang. 

Next to start Opendaylight after you have done the above steps 

1. In the Release 2016/A distribution VM for Opendaylight go to the root folder for Opendaylight 
2. Go to the bin directory 
3. Issue command ./karaf clean . Following is what you will see 

![Karaf](https://github.com/onfsdn/atrium-docs/blob/master/16A/ODL/pics/Karaf.jpg)

4. What you get after running the above command is the Karaf CLI where features can be installed on demand. What we are interested in is a feature by name ‘odl-atrium-all’. Installation of this feature will bring up the Atrium BGP Router application component inside ODL and also few utility plugins (such as ARP Handler, Tunneling Connectivty Manager, DIDM/Flow Objectives Module etc) which we will explain in subsequent sections. To install all the required features related to Atrium type the command given below. 

    opendaylight-user@root>feature:install odl-atrium-all
This will take few seconds. Please wait until the CLI prompt appears again. 

5. Once the CLI prompt appears verify if BGP Router is started by checking the log. 
     opendaylight-user@root>log:tail 
6. Look for a log message as given below which indicates all required components  started gracefully
    2016-02-16 20:48:40,158 | INFO  | config-pusher    | Bgprouter                        | 296 - org.opendaylight.atrium.bgprouter-impl - 1.0.0.SNAPSHOT | BGP Router started

Note: While installing odl-atrium-all, Opendaylight may get into a deadlock mode and fail to start on rare occasions. This is due to an inconsistency in the Opendaylight implementation as described here. In case you encounter this issue then it is recommended to logout from Karaf and restart. Other alternate option is to install features one by one (odl-l2switch-packethandler, odl-didm-all, odl-atrium-all).
