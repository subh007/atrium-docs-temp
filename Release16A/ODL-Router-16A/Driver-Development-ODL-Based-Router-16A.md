##Developing flow objective driver for a new hardware platform

The tutorial below summarizes the steps to implement flow objective interfaces in new device drivers for OpenDaylight (ODL) controller. It is necessary to develop a new driver for each unique openflow pipeline to allow ODL applications to interoperate across multiple switch platforms. The BGP peering application is currently the only ODL application for which the drivers are being tested.

All flow objectives are implemented in the ODL DIDM project (Device Independent Driver Module) which is responsible for handling vendor specific device drivers in ODL.  Please refer to [Atrium documentation](https://groups.opensourcesdn.org/wg/Atrium/document/45) to get a high level overview of flow objectives and [DIDM wiki pages](https://wiki.opendaylight.org/view/DIDM:Main) to understand the DIDM project. Also refer [document](https://groups.opensourcesdn.org/wg/Atrium/document/23) that talks about developing a simple 2-table pipeline for the ONOS controller.

In this document the Mininet driver implementation is explained as a reference implementation. The OVS-2TP pipeline is currently implemented as part of the mininet driver.

Following are the important steps for writing any device driver:
  * Setting up the project structure
  * Driver Registration
  * Device Identification
  * Device Driver Interface Implementation (Flow Objectives)
  * Device Driver Feature installation

###Setting up project structure:

Copy the folder structure from the [vendor folder](https://github.com/openflowsdn/atrium/tree/master/didm/vendor). It includes two subfolder ( feature and impl). We require to modify bundle coordinate for these two subfolder to reflect the vendor implementation.

E.g. [impl/pom.xml](https://github.com/openflowsdn/atrium/blob/master/didm/vendor/mininet/impl/pom.xml )
```
<modelVersion>4.0.0</modelVersion>
<groupId>org.opendaylight.didm.mininet</groupId>   // (group ID)
<artifactId>mininet-impl</artifactId>                              // (artificat ID)
<version>0.2.0-SNAPSHOT</version>                           //  (version)
<packaging>bundle</packaging>
```
(Similarly for [features/pom.xml](https://github.com/openflowsdn/atrium/blob/master/didm/vendor/mininet/features/pom.xml)
```
<groupId>org.opendaylight.didm.mininet</groupId>
<artifactId>mininet-features</artifactId>
<version>0.2.0-SNAPSHOT</version>
<name>${project.artifactId}</name>
<modelVersion>4.0.0</modelVersion>
```
###Driver Registration:

For registering the driver, the DIDM writes the device info to the configurational data store. The device info constitutes of (Manufacturer name and Hardware name).

Following code snippet shows the registration of ovs driver  
E.g. [MininetModule.java](https://github.com/openflowsdn/atrium/blob/master/didm/vendor/mininet/impl/src/main/java/org/opendaylight/yang/gen/v1/urn/opendaylight/params/xml/ns/yang/didm/vendor/mininet/rev150211/MininetModule.java )

Make the appropriate changes for your driver.

```java
// Device Info for the OVS
private static final Class<? extends DeviceTypeBase> DEVICE_TYPE = MininetDeviceType.class;
    private static final List<String> MANUFACTURERS = ImmutableList.of("Nicira, Inc.");
    private static final List<String> HARDWARE = ImmutableList.of("Open vSwitch");
    private static final DeviceTypeInfo DEVICE_TYPE_INFO = new DeviceTypeInfoBuilder().setDeviceType(DEVICE_TYPE)
            .setOpenflowManufacturer(MANUFACTURER)
            .setOpenflowHardware(HARDWARE).build();

// Registering the DeviceType to the config data store
private static InstanceIdentifier<DeviceTypeInfo> registerDeviceTypeInfo(DataBroker dataBroker) {
        InstanceIdentifier<DeviceTypeInfo> path = createKeyedDeviceTypeInfoPath(DEVICE_TYPE);

        WriteTransaction writeTx = dataBroker.newWriteOnlyTransaction();
        writeTx.merge(LogicalDatastoreType.CONFIGURATION, path, DEVICE_TYPE_INFO, true);

        CheckedFuture<Void, TransactionCommitFailedException> future = writeTx.submit();
```

### Device Identification:

Device and Driver mapping is done at runtime on the basis of manufacturer ID and Hw type reported from openflowplugin. (No code modification is required for new driver development and this section is added for the sake of completeness and better understanding)

e.g.
[DeviceIdentificationManager.java](https://github.com/openflowsdn/atrium/blob/master/didm/identification/impl/src/main/java/org/opendaylight/didm/identification/impl/DeviceIdentificationManager.java )

```java
private void checkOFMatch(final InstanceIdentifier<Node> path, Node node, FlowCapableNode flowCapableNode, List<DeviceTypeInfo> dtiInfoList ){
    	 if (flowCapableNode != null) {
             String hardware = flowCapableNode.getHardware();
             String manufacturer = flowCapableNode.getManufacturer();
             String serialNumber = flowCapableNode.getSerialNumber();
             String software = flowCapableNode.getSoftware();

             LOG.debug("Node '{}' is FlowCapable (\"{}\", \"{}\", \"{}\", \"{}\")",
                     node.getId().getValue(), hardware, manufacturer, serialNumber, software);

             // TODO: is there a more efficient way to do this?
             for(DeviceTypeInfo dti: dtiInfoList) {
                 // if the manufacturer matches and there is a h/w match
                 List<String> manufacturers = dti.getOpenflowManufacturer();                 

                if (manufacturer != null && manufacturers.contains(manufacturer)) {
                     List<String> hardwareValues = dti.getOpenflowHardware();
                     if(hardwareValues != null && hardwareValues.contains(hardware)) {
                             setDeviceType(dti.getDeviceType(), path);
                             return;
                     }

```



###Device Driver Interface Implementation:

The driver implements the driver interface named [“openflow-feature”](https://github.com/openflowsdn/atrium/blob/master/didm/drivers/api/src/main/yang/openflow-feature.yang ).

openflow-feature.yang defines the interface and expose the FlowObjective APIs. This interface get consumed by the application (BGP Application) to communication the application flow objectives (filtering, forwarding  and next objective).

e.g.
[OpenFlowDeviceDriver.java](https://github.com/openflowsdn/atrium/blob/master/didm/vendor/mininet/impl/src/main/java/org/opendaylight/didm/vendor/mininet/OpenFlowDeviceDriver.java )
```java
public class OpenFlowDeviceDriver implements OpenflowFeatureService, DataChangeListener, AutoCloseable {
…
}
```

Implement the interface according to your device pipeline.

###Device Driver Feature installation:

Feature for the vendor driver is defined by the copied bundle “features”. The feature file defines the required bundle, config file and feature to enable the driver.

e.g. [features/feature.xml](https://github.com/openflowsdn/atrium/blob/master/didm/vendor/mininet/features/src/main/features/features.xml )
```
<feature name='odl-didm-mininet' version='${project.version}' description='OpenDaylight :: DIDM Mininet reference driver'>
    <feature version='${didm.version}'>odl-didm-identification</feature>
    <feature version='${didm.version}'>odl-didm-drivers-api</feature>
    <bundle>mvn:org.opendaylight.didm.mininet/mininet-impl/${project.version}</bundle>
    <configfile finalname="${config.configfile.directory}/didm-mininet.xml">mvn:org.opendaylight.didm.mininet/mininet-impl/${project.version}/xml/config</configfile>
  </feature>
```

The ODL application (BGP peering app) creates one more feature “odl-didm-all” using feature “odl-didm-mininet” to make it available at runtime. For new drivers, vendors need to add their driver feature to the [feature file](https://github.com/openflowsdn/atrium/blob/master/features/src/main/features/features.xml) to make it available to install at runtime.
