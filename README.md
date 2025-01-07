
# Digital VLSI SoC Design and Planning

> 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM (Advanced Physical Design using OpenLANE/Sky130)






##  Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK 

Task 1
  1. To run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
  2. Calculate the Flop ratio.
       
```       
Flop Ratio = Number of D Flip Flops / Total Number of Cells
```
```
Percentage of DFF's = Flop Ratio * 100
```
#### Implementation 

Commands to invoke the OpenLANE flow and perform synthesis
 
 ```# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit


 ```

Screenshots of the above commands ran
<img width="1465" alt="1 final" src="https://github.com/user-attachments/assets/595bc2e8-9a27-49a0-850f-5ea08c970c2f" />



<img width="1465" alt="2 final" src="https://github.com/user-attachments/assets/26613b8b-d750-4f29-ab2f-1590bc5f2fdd" />


Screenshots of synthesis statistics report file with required values to calculate flop ratio.

<img width="1465" alt="3 final" src="https://github.com/user-attachments/assets/ed08651e-17e1-4c52-8e9c-32627359a24d" />

Calculation of Flop Ratio and DFF% from synthesis statistics report file

```
Flop Ratio = 1613/14876 = 0.108429685
```
```
Percentage of DFF's = 0.108429685 * 100 = 10.84296854%

```

 # Section 2 - Good floorplan vs bad floorplan and introduction to library cells 

 Implementation
Section 2 tasks:-

1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.
  
```
Area of die in microns = Die width in microns * Die height in microns
```
#### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshot of floorplan run

<img width="1465" alt="4 final" src="https://github.com/user-attachments/assets/432701e9-3134-4649-9889-cfd52238311c" />

#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

<img width="1465" alt="5 final" src="https://github.com/user-attachments/assets/c31ceb8c-8f82-4392-82f8-780e5444d2e8" />

According to floorplan def

```
1000 Unit Distance = 1 Micron
Die width in unit distance = 660685 - 0 = 660685
Die height in unit distance = 671405 - 0 = 671405
Distance in microns = Value in Unit Distance/1000
Die width in microns = 660685/1000 = 660.685 Microns
Die height in microns = 671405/1000 = 671.405 Microns
Area of die in microns = 660.685 * 671.405 = 443587.212425 Square Microns
```


Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-38/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

<img width="1465" alt="6 final" src="https://github.com/user-attachments/assets/603cd029-e63a-4d69-876d-422d6bcb13d6" />


Equidistant placement of ports

<img width="1465" alt="7 final" src="https://github.com/user-attachments/assets/d2155335-4077-46a6-a370-470f794075ff" />


Port layer as set through config.tcl

<img width="1465" alt="8 final" src="https://github.com/user-attachments/assets/8e3e7613-7fe6-42c4-b407-67fb368829bb" />

<img width="1465" alt="9 final" src="https://github.com/user-attachments/assets/a89ab319-6678-440b-a715-5fbe611ed6c6" />


Decap Cells and Tap Cells

<img width="1465" alt="10 final" src="https://github.com/user-attachments/assets/d66a5d99-d10a-48f4-8a4f-9e34b0a502c6" />

Diogonally equidistant Tap cells
<img width="1465" alt="11 final" src="https://github.com/user-attachments/assets/83e407f9-5f1e-4688-80fe-ada10cdec442" />

Unplaced standard cells at the origin

<img width="1465" alt="12 final" src="https://github.com/user-attachments/assets/ab78850d-8fae-4448-8188-f960b0182a7d" />

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run
<img width="1465" alt="13 final" src="https://github.com/user-attachments/assets/d6f5475b-188a-4049-9846-87e4af0fad0a" />

<img width="1465" alt="14 final" src="https://github.com/user-attachments/assets/31265e2e-daae-4d02-b0f5-a05924829f0e" />

#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

<img width="1465" alt="15 final" src="https://github.com/user-attachments/assets/8c0d8214-ba7f-48e8-90ce-7d67c38a2928" />

Standard cells legally placed 

<img width="1465" alt="16 final" src="https://github.com/user-attachments/assets/7bdbbff4-998c-4299-8c2a-b8c1b43870f3" />


Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
## Section 3 - Design library cell using Magic Layout and ngspice characterization 


### Implementation

* Section 3 tasks:-
1. Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.





#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

<img width="1465" alt="17 final" src="https://github.com/user-attachments/assets/3e30d304-653b-4d7c-a5b3-d14665cff889" />


#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

<img width="1465" alt="18 final" src="https://github.com/user-attachments/assets/1cb3128f-5e5d-43c8-8946-bee7187a12df" />

NMOS and PMOS identified

<img width="1465" alt="19 final" src="https://github.com/user-attachments/assets/7414cefc-60bf-4f2a-8a2b-ac36685f4689" />


<img width="1465" alt="20 final" src="https://github.com/user-attachments/assets/2cf282ab-d38e-4004-bbee-e630efc4db7e" />

Output Y connectivity to PMOS and NMOS drain verified

<img width="1465" alt="21 final" src="https://github.com/user-attachments/assets/fe9b93fe-373c-493c-8046-70dc4a771bbc" />

PMOS source connectivity to VDD (here VPWR) verified

<img width="1465" alt="22 final" src="https://github.com/user-attachments/assets/634d9cb3-334f-4456-8128-51803e68083c" />
NMOS source connectivity to VSS (here VGND) verified

<img width="1465" alt="23 final" src="https://github.com/user-attachments/assets/eb94ad3e-674c-4ca0-b3e9-da07b1e52f92" />

Deleting necessary layout part to see DRC error

<img width="1465" alt="24 final" src="https://github.com/user-attachments/assets/b1095e15-624f-487f-89e5-15a9eb862370" />

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

<img width="1011" alt="25 final" src="https://github.com/user-attachments/assets/d83012d2-9d96-4740-b047-1d267ba06089" />

Screenshot of created spice file

<img width="1011" alt="26 final" src="https://github.com/user-attachments/assets/fdecd097-ac8a-4f1e-ad65-827873f07349" />


#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid
<img width="1009" alt="27 final" src="https://github.com/user-attachments/assets/79a63870-4c55-4359-b3f3-dfa633b67cab" />

Final edited spice file ready for ngspice simulation

<img width="1002" alt="28 final" src="https://github.com/user-attachments/assets/866f90b8-9bfe-43ea-844f-45d85229418c" />

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

<img width="1239" alt="29 final" src="https://github.com/user-attachments/assets/698daf9e-b2b8-49f6-8106-4c7ffd80f726" />


<img width="1232" alt="30 final" src="https://github.com/user-attachments/assets/58fad7f5-793d-4c51-ae5f-dc47500a994c" />


Screenshot of generated plot

<img width="1234" alt="31 final" src="https://github.com/user-attachments/assets/9398670b-f6f3-4bde-bb66-c3a50d009e4e" />
Rise transition time calculation


```
Rise transition time = Time taken for o/p to rise to 80% - Time taken for o/p to rise to 20%
```
```
20% of the O/P = 660mv
```
```
80% of the O/P = 2.64mv
```
20% Screenshots

<img width="1233" alt="32 final" src="https://github.com/user-attachments/assets/ba98412b-17f5-4786-80d3-a42da5d94f48" />

<img width="1236" alt="33 final" src="https://github.com/user-attachments/assets/c44c5e54-fa1a-4329-be37-ab512b3a4658" />



80% Screenshots

<img width="1234" alt="34 final" src="https://github.com/user-attachments/assets/61ca92d4-62a1-4cc8-af86-7ec33830527d" />


<img width="1232" alt="35 final" src="https://github.com/user-attachments/assets/f973873d-7987-4bd7-b617-3d16a9a7cd4d" />


```
Rise transition time = 2.2463 - 2.18242 = 0.06388 ns = 63.88 ps
```

Fall transition time calculation

```
Fall transition time = Time taken for output to fall to 20% - Time taken for output to fall to 80%
```
```
20% of output = 660 mV
```
```
80% of output = 2.64 V
```

20% Screenshots

<img width="1235" alt="36 final" src="https://github.com/user-attachments/assets/416d5309-16b4-4a05-a459-987235465e91" />


<img width="1230" alt="37 final" src="https://github.com/user-attachments/assets/4355c204-9431-410e-801e-8f861b836536" />


80% Screenshots

<img width="1214" alt="38 final" src="https://github.com/user-attachments/assets/d75b98bb-0bbe-430f-8856-45e18f9ce277" />


<img width="1227" alt="39 final" src="https://github.com/user-attachments/assets/28184bdf-e670-4fe7-b183-22b9cab09285" />
```
Fall transition time = 4.09555 - 4.0536 = 0.04195 ns = 41.95 ps
```

Rise Cell Delay Calculation

```
Rise Cell Delay = Time taken for output to rise to 50% - Time taken for input to fall to 50%
```
```
50% of 3.3 V = 1.65 V
```

50% Screenshots

<img width="1226" alt="40 final" src="https://github.com/user-attachments/assets/a850c178-d361-46d1-82ee-7e2c92c06fdc" />


<img width="1230" alt="41 final" src="https://github.com/user-attachments/assets/9a40207f-f944-44b3-94e7-3999c1550cbe" />

```
Rise Cell Delay = 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
```

Fall Cell Delay Calculation

```
Fall Cell Delay = Time taken for output to fall to 50% - Time taken for input to rise to 50%
```
```
50% of 3.3 V = 1.65 V
```

50% Screenshots

<img width="1228" alt="42 final" src="https://github.com/user-attachments/assets/b6bdbb43-811f-4d04-9821-2b35cb5736a7" />


<img width="1227" alt="43 final" src="https://github.com/user-attachments/assets/d07dbfd9-c852-4d3f-96c5-2ee11073f485" />


```
Fall Cell Delay = 4.07807 - 4.05 = 0.02807 ns = 28.07 ps
```

#### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```


Screenshot of .magicrc file

<img width="1168" alt="44 final" src="https://github.com/user-attachments/assets/709d0160-d43c-4fd1-a73c-2b5497ab87df" />

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

<img width="1170" alt="45 final" src="https://github.com/user-attachments/assets/e19192f7-1547-4a7e-bc41-9e87c155f717" />


Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

<img width="1138" alt="46 final" src="https://github.com/user-attachments/assets/a7a9deeb-ae96-48ba-be25-271d14a14969" />

New commands inserted in sky130A.tech file to update drc

<img width="1131" alt="47 final" src="https://github.com/user-attachments/assets/b0c5fe67-fb3c-4da8-8e2f-3cc686827849" />


<img width="1133" alt="48 final" src="https://github.com/user-attachments/assets/922bc383-2152-4d79-a0ae-ebb958c575cb" />




Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

<img width="1137" alt="49 final" src="https://github.com/user-attachments/assets/f4141319-e243-4ee2-a3b4-c32b66deb950" />



**Incorrectly implemented difftap.2 simple rule correction**

Screenshot of difftap rules

<img width="1166" alt="50 final" src="https://github.com/user-attachments/assets/e75380fc-ec33-49ea-b1ef-e2189051426d" />



Incorrectly implemented difftap.2 rule no drc violation even though spacing < 0.42u

<img width="1140" alt="51 final" src="https://github.com/user-attachments/assets/9995bb35-48a9-47fa-9a75-20966ecf8861" />

New commands inserted in sky130A.tech file to update drc
<img width="1136" alt="52 final" src="https://github.com/user-attachments/assets/ec823c5f-a199-48ee-ba02-9f67077c22da" />


Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

<img width="1136" alt="53 final" src="https://github.com/user-attachments/assets/d6d9e5b4-13e6-4ac8-baaa-20e67940eaf0" />


**Incorrectly implemented nwell.4 complex rule correction**

Screenshot of nwell rules

<img width="1170" alt="54 final" src="https://github.com/user-attachments/assets/4f7d430c-3fc3-4aa8-88e5-8fae57d0ed55" />


Incorrectly implemented nwell.4 rule no drc violation even though no tap present in nwell

<img width="1134" alt="55 final" src="https://github.com/user-attachments/assets/00f7f3e7-a387-4ece-be25-a7ff17e34248" />

New commands inserted in sky130A.tech file to update drc

<img width="1143" alt="56 final" src="https://github.com/user-attachments/assets/1fdc60d9-515d-407a-8303-c671d3870423" />


<img width="1130" alt="57" src="https://github.com/user-attachments/assets/b5cdfe93-bd55-4a5d-98a3-973e4efdd781" />


Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

<img width="1130" alt="58" src="https://github.com/user-attachments/assets/503ab8d7-ddcc-40de-ab58-88a3869fb3eb" />

## Section 4 - Pre-layout timing analysis and importance of good clock tree 



### Implementation

* Section 4 tasks:-
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.
2. Save the finalized layout with custom name and open it.
3. Generate lef from the layout.
4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.
5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.
6. Run openlane flow synthesis with newly inserted custom inverter cell.
7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.
9. Do Post-Synthesis timing analysis with OpenSTA tool.
10. Make timing ECO fixes to remove all violations.
11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.
12. Post-CTS OpenROAD timing analysis.
13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.


#### 1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

Conditions to be verified before moving forward with custom designed cell layout:
* Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
* Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
* Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.

Commands to open the custom inverter layout

```bash
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of tracks.info of sky130_fd_sc_hd

<img width="1139" alt="59" src="https://github.com/user-attachments/assets/72f916a5-9f27-47c0-836d-aae72b770e3a" />

Commands for tkcon window to set grid as tracks of locali layer

```tcl
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of commands run

<img width="1136" alt="60" src="https://github.com/user-attachments/assets/33b11e2e-02c4-4749-897a-07b395725aac" />


Condition 1 verified

<img width="1138" alt="61" src="https://github.com/user-attachments/assets/fffdb238-74ec-43e2-bd3f-5eeae45d97cc" />


Condition 2 verified


**Horizontal track pitch** = 0.46 µm



<img width="1070" alt="62" src="https://github.com/user-attachments/assets/faecb647-8a93-4a30-914d-2cffad24318a" />



**Width of standard cell** = 1.38 µm = 0.46 × 3



Condition 3 verified


**Vertical track pitch** = 0.34 µm



<img width="1069" alt="63" src="https://github.com/user-attachments/assets/2a066da5-558c-4602-a81d-8567eb6342cd" />


**Height of standard cell** = 2.72 µm = 0.34 × 8



#### 2. Save the finalized layout with custom name and open it.

Command for tkcon window to save the layout with custom name

```tcl
# Command to save as
save sky130_vsdinv.mag
```

Command to open the newly saved layout

```bash
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

Screenshot of newly saved layout

<img width="1102" alt="64" src="https://github.com/user-attachments/assets/24a8313e-8cc4-443a-a382-fb65b94effa3" />

#### 3. Generate lef from the layout.

Command for tkcon window to write lef

```tcl
# lef command
lef write
```

Screenshot of command run

<img width="1072" alt="65" src="https://github.com/user-attachments/assets/b3723d5c-3cfb-4929-82d7-d9c7e60558d4" />


Screenshot of newly created lef file

<img width="1070" alt="66" src="https://github.com/user-attachments/assets/91c857a8-2af6-492a-a2e3-faec35147773" />

#### 4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

```bash
# Copy lef file
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

Screenshot of commands run

<img width="1175" alt="67" src="https://github.com/user-attachments/assets/f9ae47df-7af7-4307-9e98-1fdd472ccaff" />

#### 5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow

```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

Edited config.tcl to include the added lef and change library to ones we added in src directory

<img width="1070" alt="68" src="https://github.com/user-attachments/assets/ec39fd71-020b-41ed-a0da-605fe4ca40b4" />

#### 6. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```




#### 7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Noting down current design values generated before modifying parameters to improve timing

<img width="1173" alt="71" src="https://github.com/user-attachments/assets/9da06b9e-f37d-4142-b2e7-487eb35eadfa" />

<img width="796" alt="70" src="https://github.com/user-attachments/assets/7d03a2b0-f3dc-4f9f-b666-0f02761e5f7b" />

Commands to view and change parameters to improve timing and run synthesis

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 05-01_18-52 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) 1

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshot of merged.lef in `tmp` directory with our custom inverter as macro

<img width="1062" alt="72" src="https://github.com/user-attachments/assets/26aed511-81ee-4892-8282-d69f80d6f354" />


Screenshots of commands run

<img width="1163" alt="74" src="https://github.com/user-attachments/assets/8e5c844e-917c-4583-bb28-5945964417f3" />


Comparing to previously noted run values area has increased and worst negative slack has become 0

<img width="1171" alt="75" src="https://github.com/user-attachments/assets/4f892d96-3ae2-4577-bc94-fb42ed577c8a" />

<img width="794" alt="76" src="https://github.com/user-attachments/assets/dd475902-eda0-42fd-87c7-1eea0a1233af" />



#### 8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command

```tcl
# Now we can run floorplan
run_floorplan
```

Screenshots of command run

<img width="1170" alt="78" src="https://github.com/user-attachments/assets/fa08c829-bc41-4629-8986-08fbe6d42173" />



We will get an unexpected un-explainable error while using `run_floorplan` command after synthesis, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on `Floorplan Commands` section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```
# Following commands are all together sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement
```



Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of placement def in magic

<img width="1195" alt="79" src="https://github.com/user-attachments/assets/4c0f2b09-6a3d-4c35-95a7-02e020925b82" />


Screenshot of custom inverter inserted in placement def with proper abutment

<img width="1195" alt="80" src="https://github.com/user-attachments/assets/056bfba8-3d81-45f3-a3c3-2541fd785fe1" />


Command for tkcon window to view internal layers of cells

```tcl
# Command to view internal connectivity layers
expand
```

Abutment of power pins with other cell from library clearly visible

<img width="1202" alt="81" src="https://github.com/user-attachments/assets/9e66dc10-8a13-492c-a710-5f409ea96421" />


#### 9. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands run final screenshot

<img width="1169" alt="82" src="https://github.com/user-attachments/assets/ccf97a70-7a2f-4525-b352-6d83c5aacb35" />





Newly created `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src` directory based on the file `openlane/scripts/base.sdc`

<img width="954" alt="83" src="https://github.com/user-attachments/assets/eb5a3d9b-508f-425f-ac0c-29bf46c6731a" />


Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

<img width="1175" alt="84" src="https://github.com/user-attachments/assets/b3c4d80f-5620-4aad-ab92-edf6f90bb8a8" />


<img width="1166" alt="85" src="https://github.com/user-attachments/assets/91b2fcfb-0f09-40c4-ad9a-9b48dfd28b78" />


<img width="1171" alt="86" src="https://github.com/user-attachments/assets/fbdf8872-6609-489b-887f-28de4f1962a8" />


<img width="1164" alt="87" src="https://github.com/user-attachments/assets/f8a23897-830f-451b-86c4-4a4ce1b0ba19" />

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis 

```tcl
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 03-10_10-30 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands run final screenshot

<img width="1173" alt="88" src="https://github.com/user-attachments/assets/47821bf6-4cc7-46c6-81fc-87c4a9038d9f" />



Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

<img width="1170" alt="89" src="https://github.com/user-attachments/assets/b93e90ae-c5f2-4b2f-b238-4a8aa5a7d91d" />


<img width="1164" alt="90" src="https://github.com/user-attachments/assets/d4074905-468f-442f-b652-9aad66f976f8" />


<img width="1168" alt="91" src="https://github.com/user-attachments/assets/17bddafc-2d0c-42ed-9fd6-f748e2a59b9f" />


<img width="1166" alt="92 final" src="https://github.com/user-attachments/assets/f1444da5-de57-4984-9f74-5c0b32101a47" />



#### 10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts

<img width="1173" alt="93" src="https://github.com/user-attachments/assets/5ba4383e-e54b-4d83-b003-678d82aff420" />

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

<img width="1175" alt="94" src="https://github.com/user-attachments/assets/dc3a5ba2-9ef6-4b95-8b0f-12f2d7ec3f20" />

<img width="1175" alt="95" src="https://github.com/user-attachments/assets/617c6625-d991-49ef-a908-349ed08e23fa" />


<img width="1174" alt="96" src="https://github.com/user-attachments/assets/ebcdba90-568c-4d31-99f2-a504bdd2a743" />


<img width="1163" alt="97" src="https://github.com/user-attachments/assets/6bc8b440-767a-4970-a12d-95204685096b" />

OR gate of drive strength 2 is driving 4 fanouts

<img width="1173" alt="98" src="https://github.com/user-attachments/assets/ce0af848-6b2d-4a58-a928-f280d2620800" />


Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

<img width="1171" alt="99" src="https://github.com/user-attachments/assets/e657c578-dd0f-4ede-9d89-d469463a7d4f" />

<img width="1171" alt="101" src="https://github.com/user-attachments/assets/ab17caa9-7f6b-444c-b916-50500e714122" />


OR gate of drive strength 2 driving OA gate has more delay

<img width="1169" alt="102" src="https://github.com/user-attachments/assets/6da25da0-717b-42e8-b13f-0e58558f8eca" />


Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

<img width="1172" alt="103" src="https://github.com/user-attachments/assets/dd57b973-5b3e-4b22-bdc9-0bb38c8344dd" />

<img width="1169" alt="104" src="https://github.com/user-attachments/assets/54b3aa70-f1f0-486c-9fd5-6728eaef7282" />


OR gate of drive strength 2 driving OA gate has more delay

<img width="1167" alt="105" src="https://github.com/user-attachments/assets/b11cd931-91eb-4cdc-ad23-d79b6bdca9a5" />

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

<img width="1170" alt="106" src="https://github.com/user-attachments/assets/e220bcfb-16a1-43e5-9675-6cbcce755f65" />

<img width="1169" alt="107" src="https://github.com/user-attachments/assets/32885b66-d372-4af0-95aa-14881f73a421" />


Commands to verify instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

```tcl
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```

Screenshot of replaced instance

<img width="1177" alt="108" src="https://github.com/user-attachments/assets/0cfd6866-1fe0-4c14-8f14-b9f84face946" />


*We started ECO fixes at wns -23.9000 and now we stand at wns -22.6173 we reduced around 1.2827 ns of violation*

#### 11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use `write_verilog` and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist

```bash
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```


Commands to write verilog

```tcl
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```


Verified that the netlist is overwritten by checking that instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

<img width="957" alt="109" src="https://github.com/user-attachments/assets/43f0fe5d-8332-4c48-ad94-b578a909bffb" />

Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 05-01_18-52 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```



#### 12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/05-01_18-52/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of timing report generated


<img width="1172" alt="110" src="https://github.com/user-attachments/assets/ab1426bb-b349-4eeb-825c-b4495e7a7931" />

<img width="1172" alt="111" src="https://github.com/user-attachments/assets/7a4482c5-6697-4119-bcc6-53aefed4fd3f" />

<img width="1172" alt="112" src="https://github.com/user-attachments/assets/871eb6b4-71bc-4b8e-b9d0-4e3bd89c8594" />



#### 13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing `CTS_CLK_BUFFER_LIST`

```tcl
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/05-01_18-52/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```

Screenshots of timing report generated


<img width="1170" alt="113" src="https://github.com/user-attachments/assets/7a01b833-b2a2-4a9b-8113-20befb2dced4" />

<img width="1174" alt="114" src="https://github.com/user-attachments/assets/9eb31e8b-ae6d-4303-a80b-c21a1fb5cd6c" />

<img width="1174" alt="115" src="https://github.com/user-attachments/assets/362ff0aa-a267-4b20-ab83-ad0b05810c78" />

## Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA



### Implementation

* Section 5 tasks:-
1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
2. Perfrom detailed routing using TritonRoute.
3. Post-Route parasitic extraction using SPEF extractor.
4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.



#### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn 
```


Commands to load PDN def in magic in another terminal

```bash
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```


Screenshots of PDN def

<img width="954" alt="116" src="https://github.com/user-attachments/assets/27444661-07bf-4317-9f0a-0b05b2ceb312" />

<img width="958" alt="117" src="https://github.com/user-attachments/assets/c2cbee38-09c6-4a0f-aa32-a96f7e95b686" />


<img width="959" alt="118" src="https://github.com/user-attachments/assets/53926d89-eb4c-4624-b2b0-c3dda0261d18" />

#### 2. Perfrom detailed routing using TritonRoute and explore the routed layout.

Command to perform routing

```tcl
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```



Commands to load routed def in magic in another terminal

```bash
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

Screenshots of routed def

<img width="952" alt="119" src="https://github.com/user-attachments/assets/44c45b40-b2dd-40e9-855e-7526291e3987" />

<img width="957" alt="120" src="https://github.com/user-attachments/assets/62ee8198-916f-4f51-ab2a-9ed3148096b5" />


<img width="958" alt="121" src="https://github.com/user-attachments/assets/5c322f7a-8f33-4281-97a0-969077de74d0" />


<img width="956" alt="122" src="https://github.com/user-attachments/assets/f80b1974-a70b-4069-98d9-7a54903df088" />

<img width="958" alt="123" src="https://github.com/user-attachments/assets/08709e88-a29c-4c24-b8e7-1159ddc79d10" />


Screenshot of fast route guide present in `openlane/designs/picorv32a/runs/05-01_18-52/tmp/routing` directory

<img width="952" alt="124" src="https://github.com/user-attachments/assets/62bb4a8a-5154-4b74-96b1-7ee6f290322d" />


#### 3. Post-Route parasitic extraction using SPEF extractor.

Commands for SPEF extraction using external tool

```bash
# Change directory
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/scripts/spef_extractor


# Command extract spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-01_18-52/results/routing/picorv32a.def
```



#### 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/05-01_18-52/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/05-01_18-52/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of commands run and timing report generated

<img width="1168" alt="125" src="https://github.com/user-attachments/assets/d091705a-6317-4d43-af3e-648fb5fb988e" />

<img width="1172" alt="126" src="https://github.com/user-attachments/assets/fcde4785-65f1-491b-af4a-a2271ff3f78f" />









   
 








      
           
   
 


