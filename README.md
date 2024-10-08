# KNX_Home_Automation
This repo is dedicated to the KNX system including ACS experience in configuration, commissioning, integration, and troubleshooting.

## 📚Table of contents

- [Introduction to KNX](#inroduction-to-knx)

- <details>
  <summary>KNX TP Topology</summary>

  - [Topology-Overall view](#topology--overall-view)
  - [Topology line](#topology-line)
  - [Topology area](#topology-area)
  - [Topology network](#topology-network)
  - [Individual address](#individual-address)
  - [Coupler-Gate function](#coupler-gate-function)
  - [Coupler-Block diagram](#coupler-block-diagram)
  - [Coupler-field of application](#coupler-fields-of-application)
  - [Connection of several line](#connection-of-several-lines)
  - [Internal line diagram](#internal-line-diagram)
  - [Line-crossing diagram](#line-crossing-telegram)
  - [Area-crossing diagram](#area-crossing-telegram)
  - [Coupling unit: Routing center](#coupling-unit-routing-center)
  - [Internal and external interfaces](#internal-and-external-interfaces)
  - [Backbone-/ LC classical structure](#backbone--lc-classical-structure)
  - [Taking intro account higher telegram rates:IP network](#taking-into-account-higher-telegram-rates-ip-network)
  - [LC replaced by KNXnet/IP](#lc-replaced-by-knxnetip)

  <details>

- <details>
  <summary>KNX RF</summary>
  
  - [The RF transmission medium](#the-rf-transmission-medium)
  - [Cobination of transmission media](#cobination-of-transmission-mdeia)

  </details>

- [KNX Bus device](#knx-bus-device)

- <details>
  <summary>KNX TP Installation</summary>

   - [Safety Low Voltage Network](#safety-low-voltage-networks)
   - [Types of Bus cables](#types-of-bus-cables)
   - [Power supply unit](#power-supply-unit)
   - [Power supply for two lines](#power-supply-for-two-lines)
   - [Two power supply units in one line](#two-power-supply-units-in-one-line)
   - [Distributed power supply](#distributed-power-supply)
   - [Power supply unit with data rail](#power-supply-unit-with-data-rail)
   - [Power supply unit for two lines with data rail](#power-supply-unit-for-two-lines-with-data-rail)

  </details>

- <details>
  <summary>KNX project design ETS5:Basics</summary>

   - [Installation of ETS](#installation-of-ets)
   - [Licences](#licences)
   - [Starting ETS5](#starting-ets5)
    
   -  <details>
      <summary>Dashboards tabs</summary>

      - [Overview tab](#overview-tab)
       
      - [Project Properties](#project-properties)
      - [Bus tab](#bus-tab)
      - [Catalogs tab](#catalogs-tab)
      - [Settings tab](#settings-tab)
       
      </details>

   - [Project design views](#project-design-views)
   - [Panels](#panels)
   
   -  <details>
      <summary>ETS windows</summary>

      - [Buildings window](#buildings-window)
      - [Group addresses window](#group-addresses-window)
      - [Topology window](#topology-window)
      - [Devices window](#devices-window)
      - [Configuring group addresses](#configuring-group-addresses)
      - [Assignment individual addresses](#assignment-of-group-addresses)
      - [Sending group address](#sending-group-address)
      
      </details>

    - <details>
      <summary>ETS Commissioning</summary>

       - [Starting the commissioning](#starting-the-commissioning)
       - [Configuraing the interface](#configuring-the-interface)
       - [Downloading](#downloading)
       - [Assigning individual addresses](#assigning-individual-addresses)
       - [Resetting devices](#resetting-devices)

      </details>

    - <details>
      <summary>ETS Diagnostics</summary>

       - [Diagnostics and fault location](#diagnostics-and-fault-location)
       - [Diagnostics:Individual addresses](#diagnostics-individual-addresses)
       - [Devices in programming mode](#devices-in-programming-mode)
       - [Diagnostics:line scan update flag](#diagnostics-line-scan-update-flags)
       - [Saving telegrams](#saving-telegrams)

      </details>




### __Inroduction to KNX__

KNX is an open standard for home and building automation that provides seamless integration and interoperability for smart systems.

#### __KNX TP Topology__

##### __Topology- Overall view__

![Overall view](<Images/Topology Overall view.png>)

Max topological size of a KNX TP installation:
 
 Primary line -> refers to the upper segment of a line or backbone coupler
      
      - Line coupler -> main line
      - IP router -> IP side

Secondary line -> refers to the lower segment of a line or backbone coupler

     - Line coupler -> line itself
     - IP router -> TP side

The main abbreviations are used:

<img src="Images/Abbreviations.jpg" width="700" height="500">

##### __Topology Line__

![topology line](<Images/Topology line.png>)

Max. 256 devices can be mounted in one line.

Each DVC acn exchange information with any other device and each line requires its own ps/ch.

The no. of devices per line depends on the ps/ch selected and the power required for each device.

##### __Topology Area__

![topology area](<Images/Topology Area.png>)

If more devices need to be mounted than one can fit into one line, then up to 15 lines can be connected to the main line via line couplers LC.

The max number of DVC in the main line decreases due to the energy consumption by the number of mounted LCs.

LR may not be used in main lines.

It is possible to topologically address max 256 bus devices on the main line including the mounted LR.

##### __Topology Network__

![topology network](<Images/Topology Network.png>)

If more devices have to be mounted, then the TP installation can be extended by mounting BCs to the backbone line.

The max. number of DVC on the backbone line decreases due to the energy consumption by the no. of mounted BCs.

The backbone lines requires its own ps/ch and also may not be used a LRs.

##### __Individual Address__

![individual address](<Images/Individual address.png>)

The individual address identify the bus device and describes its location within the topology.

     - A -> addresses the areas 1...15
     - A -> addresses the DVC on BL 0
     - A -> only 4 bits -> half byte
     - L -> addresses the lines 1...15
     - L -> addresses the main line of respective area 0
     - L -> only 4 bits
     - B -> addresses the DVCs on the line defined by L 0...255
     - B -> addresses the coupler in respective line 0
     - B -> 8 bits -> 1 byte

The individual address of an unloaded bus device is 15.15.255.

##### __Coupler-Gate Function__

<img src="Images/Copuler-gate function.png" width="400" height="250">

If a coupler is used in the installation and the correct individual addresses are assigned to the couplers, a filter table will auto created in ETS file for the respective couplers.

The filter table contains the active line-crossing group addresses.

The coupler routes aal received droup addresses if these are listed in its filter table.

The LR used in installation passes on all telegrams has no filter table.

##### __Coupler-Block Diagram__

<img src="Images/Coupler-Block diagram.png" width="400" height="250">

The LC can be programmed both from the primary line as well as the secondary line.

The couplers are supplied from the primary line and only have one controller. This make the coupler can report secondary line power-down.

##### __Coupler-Fields of Application__

<img src="Images/Coupler-fileds of aplications.png" width="400" height="250">

BC, LC and LR are identical devices -> their tasks depend on the location and individual address.

     - BC -> connection between BL and main line.
     - LC -> connection between main line and secondary L
     - LR -> for extended a line by a line segment with up to 64 additional DVC and 1000 m cable length.

BC and LC only forward line-crossing telegrams.

LR does not have filter table so it passes all telegrams.

##### __Connection of several lines__

<img src="Images/Connectiong several lines.png" width="600" height="400">

In an installation consisting of several lines, each line or each line segment must have its own ps/ch.

##### __Internal line diagram__

![internal line](<Images/Internal line telegram.png>)

By press PB P1, sends a telegram with group address of 5/2/66.

Despite all DVCs get the telegram when it is transmitted, only the actuators of L11,L12,and L13 with the same group addresses execute the command.

##### __Line-crossing telegram__

![line-crossing telegram](<Images/line crossing telegram.png>)

If S1 is not connected in the same line as the lamp it has to control, it is necessary to transmit its telegrams via the main line.

All DVCs on this line listen to the telegram from S1, but only the actuator controlling the lights L11 & L21 execute the command.

##### __Area-crossing telegram__

![area-crossing telegram](<Images/area crossing telegram.png>)

If S1 is mounted in another area, it can still address all bus devices via backbone line.

If 0/2/11 is assigned to S1, the telegram is routed to line 1 via BC1 and BC2 and LC1.

##### __Coupling unit: Routing center__

![coupling unit](<Images/Coupling unit-routingdiagram.png>)

The telegram transmitted by sending device contains a routing center, of which the initial value is 6.

Each line -/ BC or LR decrements the routing center and passes the telegram as long as the value has not reached 0

##### __Internal and External interfaces__

![I&E interfaces](<Images/internal and external interfaces.png>)

KNX is open to be linked to any other system. the BL can be connected via a gateway unit.

Media couplers connect different types of KNX media -> TP or RF

##### __Backbone-/ LC classical structure__

![LC classical structure](<Images/Backbone-line coupler classical structure.png>)

##### __Taking into account higher telegram rates: IP Network__

![IP network](<Images/taking into account higher telegram rates- IP network.png>)

Replacement of LCs by KNXnet/IP routers is the increased telegram load, which can occur when the user makes use of visualization SW and devices with higher no. of channels.

##### __LC replaced by KNXnet/IP__

![LC replaced by IP](<Images/line coupler replaced by KNXnet-router.png>)

Case 1 -> The KNXnet/Ip router can be used as a LC as well as a BC. If the KNXnet/IP router replaces LC, all main lines and basically also the BLs are replaced by Ethernet.

Case 2 -> If BCs are replaced by KNXnet/IP routers, the normal LCs can remain, as only the backbone line is replaced by Ethernet.


#### __KNX RF__

KNX RF allows the wireless transmission of telegrams by means of the medium radio frequency.

##### __The RF transmission medium__

The range of KNX RF components is indicated as free-filed range. Due to max transmission power and limitations of energy availabe in case of battery-operated, the free feild range is limited to 100 m.

KNX RF uses 868 MHz frequency band.

##### __Cobination of transmission mdeia__

![topology RF](<Images/Topology of RF.png>)

#### __KNX Bus device__

![bus device](<Images/KNX bus device.png>)

A functioning bus device (dimming/shutter actuator) consists of 3 interconnecting parts:

     - BCU -> Bus Coupling Unit
     - AM -> Application Module
     - AP -> Application Program


#### __KNX TP Installation__

##### __Safety Low Voltage Networks__

![SELV](Images/SELV.png)

Permitted voltage range:

     Alternating current -> <= 50V
     Direct current -> <= 120V

⚠️ A SELV network may not be earthed!

##### __Types of bus cables__

![bus cables](<Images/bus cable.png>)

     - YCYM 2x2x0.8 -> outdoors -> test voltage: 4Kv
     - J-Y(st) 2x2x0.8 -> outdoors -> test voltage: 2.5Kv

The requirements for instance include a loop resistance of 75 ohm and a loop capacitance of 100 nF per 1000 m.

##### __Power Supply unit__

![PS unit](<Images/PS unit.png>)

Bus devices require a min of 21 VDC for safe operation.

##### __Power supply for two lines__

![PS for 2 lines](<Images/PS for two lines.png>)

##### __Two power supply units in one line__

![2 PS units in one line](<Images/two PS units in one line.png>)

##### __Distributed power supply__

![DPS](<Images/Distributed power supply.png>)

##### __Power supply unit with data rail__

![ps unit with datavrail](<Images/PS unit with data rail.png>)

##### __Power supply unit for two lines with data rail__

![ps unit for 2 lines with data rail](<Images/PS unit for 2 lines with data rail.png>)


#### __KNX project design ETS5:Basics__

##### __Installation of ETS__

You can only obtain ETS from from KNX assosiation via internet in the KNX online shop : https://www.knx.org/knx-en/for-professionals/index.php

##### __Licences__

![licence](Images/ETS_licence.png)

There are 4 versions availabel:

1- Demo -> Full functionality max 5 devices per project.

2- Lite -> Full functionality max 20 devices per project.

3- Professional -> Full functionality, no restrictions as regards no. of devices.

4- Supplemntary -> Additional licence for professional on a second PC.

##### __Starting ETS5__

![ETS starting](Images/ETS_starting1.png)

1- A bar with tabs for different functions of dashboard.

2- The workspace with the functions selsected by means of the tabs.

3- A status bar with info of ETS, to licences and apps.

##### __Dashboards Tabs__

###### __Overview tab__

![overview tab](<Images/ETS_overview tab.png>)

Window area ->  you can select the project that is to be
edited or you can create a new project.

Area ->  up-to-date news on KNX Association is
displayed provided that ETS is connected to the internet.

###### __Project properties__

![project details](<Images/ETS_project details.png>)

shows how you can access the properties of a project by clicking on a project. You can also import and export projects by means of this window. 

###### __Bus tab__

1- __Connections__

<p>

<img src="Images/ETS_tab bus connections interfaces.png" width="" height="">
<img src="Images/ETS_tab bus connections options.png" width="" height="">


</p>

Interfaces shows current interfaces and configured it by adding, importing, and exporting and also shows discovered interfaces.

Connection options acn use direct IP connection if avilable, use project bus connections if defined and disconnect  connection after usage.

2- __Monitor__

<p>

<img src="Images/ETS_tab bus monitor group monitor.png" width="" height="">
<img src="Images/ETS_tab bus monitors bus monitor.png" width="" height="">


</p>

3- __Diagnostics__


<img src="Images/ETS_tab bus diagnostics unload device.png" 
width="" height="">

This function will unload the app and address of the device in programming mode.

<img src="Images/ETS_tab bus diagnostics device info.png" width="" height="">

In this tab we can read device information from searching the individual address.


<img src="Images/ETS_tab bus diagnostics IA programming mode.png" width="" height="">

<img src="Images/ETS_tab bus diagnostics individual address check.png" width="" height="">

Check if an address is reachable and locate the device from putting the individual address.


<img src="Images/ETS_tab bus diagnostics line scan.png" width="" height="">

In this tab we can search ot list all existing addresses in a single line.

###### __Catalogs Tab__

![catalogs tab](<Images/ETS_catalogs tap.png>)

Under the tab catalogs the product data of the manufacturers are administrated. After the installation ETS does not dispose yet of product data of the manufacturers. In order to be able to insert product data into an ETS project, product data of the manufacturers have to be imported before into the ETS catalog (product store).

###### __Settings Tab__

![settings tab](<Images/ETS_settings tap.png>)

The settings are subdivided in eight different sections, which you can select by clicking on the terms in the left part of the window:

Presentation ->influence the behaviour and the appearance of ETS.

Language -> elect the language of ETS

Online Catalog ->the settings for the ETS App Online KNX Product Catalog are made.

Data Storage ->set the paths for the storage of the product data.

Troubleshooting -> You can set amongst others how detailed your work with ETS will be documented in the log files which can be helpful later on for the error diagnostics by KNX Association.

Import/Export -> Settings for the extent of exported files.

Shortcuts -> Here you can see all shortcuts and you can adapt them to your habits.

Label Printer -> Settings for the ETS App Label Printer.


##### __Project design views__

![design views](<Images/ETS_project design view.png>)

     1. Menu bar
     2. Toolbar
     3. Panels
     4. Navigation bar
     5. Status bar

##### __Panels__

![panels](Images/ETS_panels.png)

     1. Title bar->select which panel should be displayed.
     2. Toolbar->available tools
     3. Tree view
     4. List view->have up to three index cards which you can select via the tabs below.

##### __ETS windows__

###### __buildings window__

![buildings window](<Images/ETS_buildings window.png>)

The Buildings view is used to structure the KNX
projects according to the actual building structure and to insert the KNX devices.

###### __Group addresses window__

![GAW](<Images/ETS_group addresses window.png>)

This view is required together with the Building view to link the group addresses to the corresponding group objects.

###### __Topology window__

![topology window](<Images/ETS_topology window.png>)

###### __Devices window__

![devices window](<Images/ETS_devices window.png>)

###### __Configuring group addresses__

![configuring GA](<Images/ETS_configuring group addresses.png>)

###### __Assignment of group addresses__

![assignment of GA](<Images/ETS_Assignment of group addresses.png>)

###### __Sending group address__

![sending GA](<Images/ETS_sending group addresses.png>)


#### __ETS Commissioning__

The commissioning can be carried out via
✓ a USB interface
✓ or a network connection
of the commissioning PC. The required settings are explained in more detail under “Bus access” and “Configuring the interface”.

##### __Starting the commissioning__

![start comm](<Images/ETS_starting the commissioning.png>)

You access the embedded commissioning dialog via the Buildings View, Topology View, Group Addresses View or Device View.

##### __Configuring the interface__

![KNXnet IP routing](<Images/ETS_KNXnet IP routing.png>)

With the communication type “KNXnet/IP Routing”, pure KNX IP devices can be addressed. These are devices that only have an IP connection. The communication type can also be used
to carry out a diagnosis of the IP network connection.

##### __Downloading__

![downloading](<Images/ETS_download full and partial.png>)

Download all -> Download of the individual address and the application program (press the programming button)

Download partial -> Download of the not yet downloaded parts (programmed flag not set)

##### __Assigning individual addresses__

![assigning](<Images/ETS_assigning individual address.png>)

##### __Resetting devices__

![resitting](<Images/ETS_resetting devices.png>)


#### __ETS Diagnostics__

##### __Diagnostics and fault location__

![project check](<Images/ETS_Dianostics project check.png>)

##### __Diagnostics: Individual Addresses__

![individual address](<Images/ETS_Diagnostics individual address check.png>)

##### __Devices in Programming Mode__

![PM](<Images/ETS_Diagnostics devices in programming mode.png>)

##### __Diagnostics: Line Scan update flags__

![flag](<Images/ETS_Diagnostics select update flag.png>)


##### __Saving telegrams__

![saving telegrams](<Images/ETS_Diagnostics saving telegrams.png>)

It is possible to save recorded telegrams via the button “Save” in the toolbar of the Bus Monitor or Group Monitor window.


### __KNX Multitouch Pro (MTN6215-5910)__

<video width="600" controls>
  
 <source src="" type="">

</video>