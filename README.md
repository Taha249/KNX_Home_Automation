# KNX_Home_Automation
This repo is dedicated to the KNX system including ACS experience in configuration, commissioning, integration, and troubleshooting.

## 📚Table of contents


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

![abbreviations](Images/Abbreviations.jpg)

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

<img src="Images/Connectiong several lines.png" width="500" height="400">

