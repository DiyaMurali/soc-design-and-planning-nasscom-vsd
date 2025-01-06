
# VSD - Digital VLSI SoC Design and Planning

2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM




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

![Screenshot 2024-09-23 175829](https://github.com/user-attachments/assets/50f45b7a-925f-4494-9fde-8aa065e4b67d)



![IMG-20240923-WA0013](https://github.com/user-attachments/assets/947472b4-1c15-47b1-971c-6f065d3f2de6)

Screenshots of synthesis statistics report file with required values to calculate flop ratio.

![WhatsApp Image 2024-09-29 at 16 39 02_9aa1acda](https://github.com/user-attachments/assets/885fdad3-4fea-49d4-be81-951881e6ec29)



![WhatsApp Image 2024-09-29 at 16 39 02_7e77b4cf](https://github.com/user-attachments/assets/473bba60-39b2-4f04-bd74-4c40472623ec)

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
  
  ```math
Area\ of die in microns = Die width in microns * Die height in microns
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

![Screenshot 2024-09-23 180522](https://github.com/user-attachments/assets/035e3cff-be3f-4bba-868b-2ebfab04c399)

#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

![WhatsApp Image 2024-09-29 at 16 39 05_9222792f](https://github.com/user-attachments/assets/aee6c325-29ac-4f44-bf2c-98ec70c60863)

According to floorplan def

```math
1000 Unit Distance = 1 Micron
```

```math
Die width in unit distance = 660685 - 0 = 660685
```

```math
Die height in unit distance = 671405 - 0 = 671405
```

```math
Distance in microns = Value in Unit Distance/1000
```

```math
Die width in microns = 660685/1000 = 660.685 Microns
```

```math
Die height in microns = 671405/1000 = 671.405 Microns
```

```math
Area of die in microns = 660.685 * 671.405 = 443587.212425 Square Microns
```


Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/23-09_12-27/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![WhatsApp Image 2024-09-29 at 16 39 25_91c53eb6](https://github.com/user-attachments/assets/eb683466-fd15-4fb5-b9a9-477e7e3acd37)


Equidistant placement of ports

![WhatsApp Image 2024-10-02 at 19 48 20_8711e371](https://github.com/user-attachments/assets/29fa07e3-5ca1-41c0-8169-217e8d8fa141)

Port layer as set through config.tcl

![WhatsApp Image 2024-09-29 at 16 39 57_cafba4d5](https://github.com/user-attachments/assets/92b238cb-cb07-4eb9-abe8-c5c3df2c832e)

![WhatsApp Image 2024-10-02 at 19 56 50_d3eb2cd7](https://github.com/user-attachments/assets/c2d53889-bd53-4e0d-99f4-25e063e009f6)

Decap Cells and Tap Cells

![WhatsApp Image 2024-09-29 at 16 39 57_be8f7a4a](https://github.com/user-attachments/assets/ff2ef567-f7dd-4008-adb2-de67d72cff64)

Diogonally equidistant Tap cells
![WhatsApp Image 2024-09-29 at 16 39 57_f90bfbae](https://github.com/user-attachments/assets/fc3d8ffa-bd61-41a0-888e-0043c109d44f)

Unplaced standard cells at the origin

![WhatsApp Image 2024-09-29 at 16 39 57_394a3396](https://github.com/user-attachments/assets/f6b64d58-ca1d-4238-bfb7-6ef16d051c73)

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run
![image](https://github.com/user-attachments/assets/1e485942-40e4-4e35-ad1b-452fe6198e49)

![WhatsApp Image 2024-09-29 at 16 38 57_1f1a60d3](https://github.com/user-attachments/assets/78d1f928-5860-4b8a-9285-c1c99a23b9b5)

#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/23-09_12-27/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![WhatsApp Image 2024-09-29 at 16 39 57_8c88e123](https://github.com/user-attachments/assets/91cfb575-6623-4757-af53-54ea87b9d613)

Standard cells legally placed 

![WhatsApp Image 2024-09-29 at 16 39 57_1f5d5090](https://github.com/user-attachments/assets/8e2c9ed2-3a4b-4537-b8ba-b5b4fda253e0)

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

![Screenshot from 2024-09-29 16-44-15](https://github.com/user-attachments/assets/976404d0-2213-4131-8de4-53a1db53b24e)

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Screenshot from 2024-09-29 16-44-23](https://github.com/user-attachments/assets/1802f534-018f-472c-b3ab-5a1cdfb4e92b)

NMOS and PMOS identified

![Screenshot from 2024-09-29 23-01-52](https://github.com/user-attachments/assets/691029de-64f7-402e-9bc0-5d75652f87da)

![Screenshot from 2024-09-29 21-56-31](https://github.com/user-attachments/assets/1690cf74-d4ec-4287-8294-e834ee2b2a9c)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot from 2024-09-29 23-16-09 (1)](https://github.com/user-attachments/assets/20eab7c0-275d-4516-95cb-412cb42c54a9)

PMOS source connectivity to VDD (here VPWR) verified

![Screenshot from 2024-09-29 23-16-41](https://github.com/user-attachments/assets/b6935c8e-19d8-42d3-818a-5973b4e7ce2e)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot from 2024-09-29 23-16-30](https://github.com/user-attachments/assets/bf9474a7-f896-41b4-a652-3e16391aa9fd)

Deleting necessary layout part to see DRC error

![Screenshot from 2024-09-29 23-22-24](https://github.com/user-attachments/assets/e7156e5f-6145-4ff0-a188-3699a23af76f)

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

![Screenshot from 2024-09-29 23-20-42](https://github.com/user-attachments/assets/04d021de-6abe-4f69-b01c-307e3d64a6fa)

Screenshot of created spice file

![Screenshot from 2024-09-29 23-24-09](https://github.com/user-attachments/assets/461c1af0-6e90-4142-b173-93dbc9090632)

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Screenshot from 2024-09-29 23-27-51](https://github.com/user-attachments/assets/46679748-204b-4576-b02b-51f10450408c)

Final edited spice file ready for ngspice simulation

![Screenshot from 2024-09-29 23-34-40](https://github.com/user-attachments/assets/963b9eb4-8c09-4ec5-8b27-2952af39d2e1)

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

![Screenshot from 2024-09-29 23-36-57](https://github.com/user-attachments/assets/c67caf19-2d2f-4e5a-a0ba-126457fc09cc)

![Screenshot from 2024-09-29 23-37-26](https://github.com/user-attachments/assets/9ce5d8c9-5891-45a8-baf5-678b78828f1b)

Screenshot of generated plot

![Screenshot from 2024-09-29 23-38-03](https://github.com/user-attachments/assets/9b58d19a-19f2-485a-b8bd-62e7d95721bc)

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

![Screenshot from 2024-09-29 23-42-38](https://github.com/user-attachments/assets/76b8523e-a726-4edf-8fbd-34d373298cd1)

![Screenshot from 2024-09-29 23-42-44](https://github.com/user-attachments/assets/6629e331-30ab-4807-9d4c-b2a37c039721)


80% Screenshots

![Screenshot from 2024-09-29 23-43-57](https://github.com/user-attachments/assets/fc5b66e7-2c38-4be6-8d81-12b8b24ff8e9)

![Screenshot from 2024-09-29 23-43-49](https://github.com/user-attachments/assets/a5841cb8-1244-4805-9c9d-979dde29ee05)

```
Rise transition time = 2.29505 - 2.19545 = 0.0996 ns = 99.6 ps
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

![Screenshot from 2024-10-02 21-58-28 1](https://github.com/user-attachments/assets/49ca4b44-e322-4d3f-a0bd-7be164d92156)

![Screenshot from 2024-10-02 21-58-32 1](https://github.com/user-attachments/assets/2de43fd6-7beb-4361-94f3-94ec3a61c302)

80% Screenshots

![Screenshot from 2024-10-02 21-59-25 1](https://github.com/user-attachments/assets/835a9e0d-6252-4379-bbc7-b4107e71a125)

![Screenshot from 2024-10-02 21-59-29 1](https://github.com/user-attachments/assets/8090ac3e-a0e7-4801-ab4b-2109a959d9fa)

```
Fall transition time = 4.09559 - 4.0531 = 0.04249 ns = 42.49 ps
```

Rise Cell Delay Calculation

```
Rise Cell Delay = Time taken for output to rise to 50% - Time taken for input to fall to 50%
```
```
50% of 3.3 V = 1.65 V
```

50% Screenshots

![WhatsApp Image 2024-10-02 at 22 13 49_2de01a5d](https://github.com/user-attachments/assets/3c3f1b7d-ef7d-4fec-9509-49eae308a346)

![WhatsApp Image 2024-10-02 at 22 13 49_7acceb53](https://github.com/user-attachments/assets/ea5c8c72-80fe-42ac-9833-69af1284119b)

```
Rise Cell Delay = 2.21174 - 2.15011 = 0.06163 ns = 61.63 ps
```

Fall Cell Delay Calculation

```
Fall Cell Delay = Time taken for output to fall to 50% - Time taken for input to rise to 50%
```
```
50% of 3.3 V = 1.65 V
```

50% Screenshots

![WhatsApp Image 2024-10-02 at 22 13 49_61f8fb41](https://github.com/user-attachments/assets/20b9f4f4-914d-4c5a-8b19-df8c4c8e8183)

![WhatsApp Image 2024-10-02 at 22 13 49_7131e0b9](https://github.com/user-attachments/assets/cab04013-228a-4fb4-9f8f-b952c089fc67)

```
Fall Cell Delay = 4.07821 - 4.05 = 0.02821 ns = 28.21 ps
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

Screenshots of commands run

![Screenshot from 2024-09-29 23-49-57](https://github.com/user-attachments/assets/7b13623f-a585-427e-bcad-d92e84921b8d)

![Screenshot from 2024-09-29 23-50-06](https://github.com/user-attachments/assets/79e6698d-9edb-4d5b-9858-a65319d2f523)

Screenshot of .magicrc file

![Screenshot from 2024-09-29 23-50-28](https://github.com/user-attachments/assets/660fecab-a6d0-47ea-9135-795274c7785e)

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

![image](https://github.com/user-attachments/assets/e53003b9-0196-4743-a69d-b22ab4cb815a)

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

![WhatsApp Image 2024-10-02 at 22 26 37_97dd7e0c](https://github.com/user-attachments/assets/c83a30c6-3188-4b2a-a7ec-146761ac6f42)


New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-09-30 00-15-50](https://github.com/user-attachments/assets/1eec6cb7-7eb4-47ef-ac09-c7bee9c869a0)

![Screenshot from 2024-09-30 00-23-00](https://github.com/user-attachments/assets/7f19e46c-6051-41ed-84f2-3570346e12c2)



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

![Screenshot from 2024-09-30 00-25-10](https://github.com/user-attachments/assets/15d66e88-58b2-4016-9d65-39bf846981d1)

![Screenshot from 2024-09-30 00-46-55](https://github.com/user-attachments/assets/f1bb5420-cf49-4e5a-a650-dcc64e65528d)

**Incorrectly implemented difftap.2 simple rule correction**

Screenshot of difftap rules

![image](https://github.com/user-attachments/assets/0e9c00e5-c310-402e-89a9-3dd2018a7e14)


Incorrectly implemented difftap.2 rule no drc violation even though spacing < 0.42u

![WhatsApp Image 2024-10-02 at 22 36 20_7731d86c](https://github.com/user-attachments/assets/6e08229c-a654-49be-90f5-5ca3f1e5046f)

New commands inserted in sky130A.tech file to update drc

![WhatsApp Image 2024-10-02 at 22 36 18_a723a90d](https://github.com/user-attachments/assets/3a975ab9-e72e-4d6c-b8d1-0cd3d9a7e819)


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

![WhatsApp Image 2024-10-02 at 22 36 19_94d342a2](https://github.com/user-attachments/assets/5ab324b3-a3d3-4804-bfb0-77c63879c513)

**Incorrectly implemented nwell.4 complex rule correction**

Screenshot of nwell rules

![image](https://github.com/user-attachments/assets/b2b5fdf1-4736-4a84-9866-62863bf1ff9c)

Incorrectly implemented nwell.4 rule no drc violation even though no tap present in nwell

![Screenshot from 2024-09-30 01-12-20](https://github.com/user-attachments/assets/5fd41b1e-0f83-422a-a2a7-90e4e61d1ce1)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-09-30 01-04-06](https://github.com/user-attachments/assets/7b1b025b-456a-4749-beb9-d9704d486bdb)

![Screenshot from 2024-09-30 01-04-28](https://github.com/user-attachments/assets/4a2cca17-2d79-4329-99a7-b8d60602e968)

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

![Screenshot from 2024-09-30 01-12-20](https://github.com/user-attachments/assets/3897d886-f14d-4870-9a56-f15195fa8460)

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

![Screenshot from 2024-10-01 00-25-00](https://github.com/user-attachments/assets/e9b710a7-2255-4f3b-8a63-9595d13339d3)

Commands for tkcon window to set grid as tracks of locali layer

```tcl
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of commands run

![Screenshot from 2024-10-01 00-26-14](https://github.com/user-attachments/assets/b513ad4e-a874-4e0b-85b7-00739e3ceb9a)

Condition 1 verified

![Screenshot from 2024-10-01 17-19-21](https://github.com/user-attachments/assets/78c93533-e0fc-4961-906c-c541be279f57)

Condition 2 verified

```math
Horizontal track pitch = 0.46 um
```

![Screenshot from 2024-10-01 17-27-51](https://github.com/user-attachments/assets/8bc0218a-14e6-4e9f-b776-daf01ade4c59)

```math
Width of standard cell = 1.38 um = 0.46 * 3
```

Condition 3 verified

```math
Vertical track pitch = 0.34 um
```
![Screenshot from 2024-10-01 17-27-31](https://github.com/user-attachments/assets/1d1b5e22-690d-41bc-af03-2d7c7e313c49)

```math
Height of standard cell = 2.72 um = 0.34 * 8
```

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

![Screenshot from 2024-10-01 17-30-26](https://github.com/user-attachments/assets/b2ed6f61-16ec-4fd3-a0f2-1fd0ec160337)

#### 3. Generate lef from the layout.

Command for tkcon window to write lef

```tcl
# lef command
lef write
```

Screenshot of command run

![Screenshot from 2024-10-01 17-30-39](https://github.com/user-attachments/assets/a8adcfa5-0f09-4f15-a3ee-ac9c398530f2)

Screenshot of newly created lef file

![Screenshot from 2024-10-01 17-31-41](https://github.com/user-attachments/assets/2229d548-383e-478c-a128-8d97fe052c6b)

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

![Screenshot from 2024-10-01 17-37-05](https://github.com/user-attachments/assets/b6741912-924c-4b63-ad2a-8f6e45890549)

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

![Screenshot from 2024-10-01 17-45-12](https://github.com/user-attachments/assets/edec9e17-e29e-4fa4-9e1a-f0d201dbd7ba)

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

Screenshots of commands run

![Screenshot from 2024-10-01 18-21-52](https://github.com/user-attachments/assets/78028ee7-6ed5-4bf0-b266-f03b56b78884)

#### 7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Noting down current design values generated before modifying parameters to improve timing

![Screenshot from 2024-10-01 18-21-08](https://github.com/user-attachments/assets/84f6c0d2-54a8-4f17-bfec-b50d57dca767)

![Screenshot from 2024-10-01 18-21-02](https://github.com/user-attachments/assets/b12d9660-f667-4f14-908e-82a4b36845c1)

Commands to view and change parameters to improve timing and run synthesis

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-09_19-19 -overwrite

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

![Screenshot from 2024-10-01 18-16-10](https://github.com/user-attachments/assets/1ab634a4-eed4-4440-a4bc-aea13cdebfec)

Screenshots of commands run

![Screenshot from 2024-10-01 17-53-50](https://github.com/user-attachments/assets/7d99a2f0-1a1a-4ab1-bc8b-8edfdf5ccc9e)

![Screenshot from 2024-10-01 18-26-03](https://github.com/user-attachments/assets/f7eb4275-ad10-4709-b992-d09e1b73cd87)


Comparing to previously noted run values area has increased and worst negative slack has become 0

![Screenshot from 2024-10-01 18-27-05](https://github.com/user-attachments/assets/0ff437e6-f7e9-4a9c-80dc-f9f3e089ee7f)

![Screenshot from 2024-10-01 18-26-59](https://github.com/user-attachments/assets/6c95c131-5969-47c2-94ca-1dbd6d8cd107)

#### 8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command

```tcl
# Now we can run floorplan
run_floorplan
```

Screenshots of command run

![Screenshot from 2024-10-01 18-28-23](https://github.com/user-attachments/assets/f6fabcd0-03dc-4dff-b24a-e86ca2ac7acc)

![Screenshot from 2024-10-01 18-28-00](https://github.com/user-attachments/assets/62eb9b8c-7562-4889-88d1-0b43534f5bf4)

Since we are facing unexpected un-explainable error while using `run_floorplan` command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on `Floorplan Commands` section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```tcl
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or
```

Screenshots of commands run

![Screenshot from 2024-10-01 18-29-49](https://github.com/user-attachments/assets/fd0b7e65-4686-4191-9faf-4836d0fd06b6)

![Screenshot from 2024-10-01 18-30-01](https://github.com/user-attachments/assets/3e5aba77-c592-4557-92dc-f0546bc33239)

![Screenshot from 2024-10-01 18-30-04](https://github.com/user-attachments/assets/14836a58-715d-429b-ad26-17a6a77accb0)

Now that floorplan is done we can do placement using following command

```tcl
# Now we are ready to run placement
run_placement
```

Screenshots of command run

![Screenshot from 2024-10-01 18-31-18](https://github.com/user-attachments/assets/e623be3d-2b3c-4f1f-8d1f-1e6b069f6b26)

![Screenshot from 2024-10-01 18-31-27](https://github.com/user-attachments/assets/85d3b9a6-869c-47ac-a192-f2c75b3bcc51)

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-09_19-19/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of placement def in magic

![Screenshot from 2024-10-01 18-33-10](https://github.com/user-attachments/assets/f17f171a-c0f4-4899-a78a-86bf77c0bb66)

Screenshot of custom inverter inserted in placement def with proper abutment

![Screenshot from 2024-10-01 18-33-36](https://github.com/user-attachments/assets/b0e85b0b-07a3-4612-9aeb-960c33117322)

Command for tkcon window to view internal layers of cells

```tcl
# Command to view internal connectivity layers
expand
```

Abutment of power pins with other cell from library clearly visible

![Screenshot from 2024-10-01 18-34-10](https://github.com/user-attachments/assets/c56417ac-7839-4834-9dc8-96a1e274d409)

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

![WhatsApp Image 2024-10-03 at 16 11 30_75e90ae0](https://github.com/user-attachments/assets/ee7a3b0c-776c-4b9e-a9f8-6512047873a1)

Newly created `pre_sta.conf` for STA analysis in `openlane` directory

![WhatsApp Image 2024-10-03 at 16 11 30_c34f9892](https://github.com/user-attachments/assets/81915ac9-a166-4af2-bfbc-53e746296f6a)

Newly created `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src` directory based on the file `openlane/scripts/base.sdc`

![WhatsApp Image 2024-10-03 at 15 48 58_59b11f79](https://github.com/user-attachments/assets/a9f68fd0-6fa0-417f-ac7b-3389e44335f4)

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

![WhatsApp Image 2024-10-03 at 16 11 30_887c6655](https://github.com/user-attachments/assets/239b8248-91ce-4043-9762-d75378fffea4)

![WhatsApp Image 2024-10-03 at 16 11 35_7f4ff229](https://github.com/user-attachments/assets/dbe7eadc-f960-45e1-b1e7-a14207ace847)

![WhatsApp Image 2024-10-03 at 16 11 35_e3c0b46e](https://github.com/user-attachments/assets/d6d64c97-5610-418f-8638-73609e3c20d3)

![WhatsApp Image 2024-10-03 at 16 11 35_c85c5457](https://github.com/user-attachments/assets/172d7066-854b-444e-a18b-8024858ee986)


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

![WhatsApp Image 2024-10-03 at 16 11 35_eeabbcbc](https://github.com/user-attachments/assets/57174759-afc6-4028-9ced-97214ea9787e)

![WhatsApp Image 2024-10-03 at 16 26 48_7a7edc27](https://github.com/user-attachments/assets/8d4e499a-f3a3-4580-a9a3-c6090e5061e8)

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

![WhatsApp Image 2024-10-03 at 16 26 48_bc213844](https://github.com/user-attachments/assets/473a06cd-4a90-4716-8572-a4e8a9b96ac1)

![WhatsApp Image 2024-10-03 at 16 26 48_9da211e6](https://github.com/user-attachments/assets/442dd400-18bd-4708-9a99-0a203a7fb057)

![WhatsApp Image 2024-10-03 at 16 26 48_981b622b](https://github.com/user-attachments/assets/eedfb28f-e17d-45d1-9a6f-65919d363656)

![WhatsApp Image 2024-10-03 at 16 26 48_6ebb809f](https://github.com/user-attachments/assets/ecd33bac-3f0b-4792-8318-519261f5abb6)

#### 10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts

![WhatsApp Image 2024-10-03 at 16 26 48_83280348](https://github.com/user-attachments/assets/5882d0f5-c49e-4716-a874-dbe8737e8bc5)

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

![WhatsApp Image 2024-10-03 at 16 26 48_aa40614d](https://github.com/user-attachments/assets/2e4ab95d-bab1-41ae-817c-b8f8349680e7)

![WhatsApp Image 2024-10-03 at 16 26 51_35f25ac7](https://github.com/user-attachments/assets/942a7a25-e8a8-41a7-a03c-e1646a15a7a6)

![WhatsApp Image 2024-10-03 at 16 26 51_b243c12d](https://github.com/user-attachments/assets/e801a249-f977-43eb-92c9-b87c9f239102)

![WhatsApp Image 2024-10-03 at 16 26 51_37ebd960](https://github.com/user-attachments/assets/98e57d7f-420b-408d-ae6d-9ffd764b33a1)

OR gate of drive strength 2 is driving 4 fanouts

![WhatsApp Image 2024-10-03 at 16 26 57_0b002b8b](https://github.com/user-attachments/assets/368f4878-68b0-413f-9da4-565731c117f5)

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

![WhatsApp Image 2024-10-03 at 16 26 57_7aae6f8d](https://github.com/user-attachments/assets/106fb7fd-43a1-4eab-b5f2-6a318802ab26)

![WhatsApp Image 2024-10-03 at 16 26 57_3a633e21](https://github.com/user-attachments/assets/ad3653a2-4480-4d5a-b31e-938cf37055f4)

OR gate of drive strength 2 driving OA gate has more delay

![WhatsApp Image 2024-10-03 at 16 26 57_0490a6b8](https://github.com/user-attachments/assets/01033e92-d360-49a5-9527-9296b2aeba0d)

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

![WhatsApp Image 2024-10-03 at 16 26 57_41f29edf](https://github.com/user-attachments/assets/c29a7cc3-9de2-4568-aaeb-ea32877d6169)

![WhatsApp Image 2024-10-03 at 16 26 57_16b62538](https://github.com/user-attachments/assets/f13fe561-f4a7-423f-97ac-0e5ed51ad349)

OR gate of drive strength 2 driving OA gate has more delay

![WhatsApp Image 2024-10-03 at 16 26 59_32ab2d04](https://github.com/user-attachments/assets/c0d1acbb-5f9e-4ea3-b3d1-3dcbb5bda39a)
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

![WhatsApp Image 2024-10-03 at 16 26 59_34260d01](https://github.com/user-attachments/assets/6171a943-178a-4013-ad0e-295a2363ccaa)

![WhatsApp Image 2024-10-03 at 16 26 59_694fecb0](https://github.com/user-attachments/assets/6861b520-c051-4229-aff1-e0a9f154bf48)

Commands to verify instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

```tcl
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```

Screenshot of replaced instance

![WhatsApp Image 2024-10-03 at 16 26 59_b253374a](https://github.com/user-attachments/assets/9417e560-e43b-40bd-98be-e528902d786f)

*We started ECO fixes at wns -23.9000 and now we stand at wns -22.6173 we reduced around 1.2827 ns of violation*

#### 11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use `write_verilog` and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist

```bash
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_10-30/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

Screenshot of commands run

![WhatsApp Image 2024-10-03 at 16 26 59_9c9f0ebc](https://github.com/user-attachments/assets/da1e3ae9-c726-425a-8012-13c1a4370b5c)

Commands to write verilog

```tcl
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_10-30/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

Screenshot of commands run

![WhatsApp Image 2024-10-03 at 16 26 59_41155701](https://github.com/user-attachments/assets/8957a224-ae10-4e08-8c77-25ff0bab8373)

Verified that the netlist is overwritten by checking that instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

![Screenshot from 2024-10-01 19-44-19](https://github.com/user-attachments/assets/c2fba02c-f481-4104-a8fb-f9b766e4e713)

Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-09_19-19 -overwrite

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

Screenshots of commands run

![Screenshot from 2024-10-01 19-47-25](https://github.com/user-attachments/assets/77930291-f905-48f0-80fe-bec87c82a366)

![Screenshot from 2024-10-01 19-48-24](https://github.com/user-attachments/assets/cc8d817d-1e43-482d-895f-8d6dcf2e2d9d)

![Screenshot from 2024-10-01 19-49-22](https://github.com/user-attachments/assets/c6b56404-8b50-4235-887c-1020e8c72585)

![Screenshot from 2024-10-01 19-49-58](https://github.com/user-attachments/assets/02a582c1-5467-454a-aaa0-f8d3cca13a40)

![Screenshot from 2024-10-01 19-50-13](https://github.com/user-attachments/assets/9bc5b8b9-684e-4a3a-8294-089911e9a85e)

![Screenshot from 2024-10-01 19-50-31](https://github.com/user-attachments/assets/40e98078-3e27-4db7-9d40-c63401bf3c73)

![Screenshot from 2024-10-01 19-53-41 - 1](https://github.com/user-attachments/assets/4ae4ac9f-a66d-421e-95a6-57db49a919e4)

#### 12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-09_19-19/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-09_19-19/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-09_19-19/results/synthesis/picorv32a.synthesis_cts.v

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

Screenshots of commands run and timing report generated

![Screenshot from 2024-10-01 19-58-41](https://github.com/user-attachments/assets/5e130f2e-59a5-40ff-b51e-907819d9feff)

![Screenshot from 2024-10-01 19-58-48](https://github.com/user-attachments/assets/605939c4-8cdf-4d75-8cab-61f2c296e245)

![Screenshot from 2024-10-01 19-58-59](https://github.com/user-attachments/assets/14a5841a-f37e-4598-b55c-45ddaa72aad6)

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
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/24-09_19-19/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-09_19-19/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-09_19-19/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-09_19-19/results/synthesis/picorv32a.synthesis_cts.v

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

Screenshots of commands run and timing report generated

![Screenshot from 2024-10-01 20-03-17](https://github.com/user-attachments/assets/fc7a09b6-5dae-4a52-8683-a59a0b720985)

![Screenshot from 2024-10-01 20-10-38](https://github.com/user-attachments/assets/074db746-939b-444f-8eb6-f8bdae92fe9f)

![Screenshot from 2024-10-01 20-10-55](https://github.com/user-attachments/assets/a551d5b0-e3a4-4170-aff3-b77537e49df8)

![Screenshot from 2024-10-01 20-10-55](https://github.com/user-attachments/assets/f30df6bc-9313-40cc-9660-694b5073595a)

![Screenshot from 2024-10-01 20-10-59](https://github.com/user-attachments/assets/7c1442d8-116e-4a4c-adb9-7e6a8a5765ed)

## Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA (25/03/2024 - 26/03/2024)



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

Screenshots of power distribution network run

![WhatsApp Image 2024-10-03 at 17 22 38_b14b7a46](https://github.com/user-attachments/assets/28e855ad-5030-482a-bee6-8dcb216baf27)

![WhatsApp Image 2024-10-03 at 17 22 54_7deec917](https://github.com/user-attachments/assets/0c44b425-2801-4557-8285-03f4bf7644a3)

![WhatsApp Image 2024-10-03 at 17 22 55_3ebd028d](https://github.com/user-attachments/assets/18b61353-303c-415d-b589-a17af7bc7f22)

![WhatsApp Image 2024-10-03 at 17 22 57_36971d23](https://github.com/user-attachments/assets/0fe3a81f-8ceb-45fa-907c-1e4210f10773)

![WhatsApp Image 2024-10-03 at 17 23 04_19fdcd4a](https://github.com/user-attachments/assets/8df0645b-49c1-44e5-970b-284263580f9d)

![WhatsApp Image 2024-10-03 at 17 23 04_6e17a928](https://github.com/user-attachments/assets/7739360a-da83-4fd2-9c76-40d164be4d79)

![WhatsApp Image 2024-10-03 at 17 23 04_e632bd85](https://github.com/user-attachments/assets/fc64bf06-2f70-4aa6-9820-323af35462bd)

![WhatsApp Image 2024-10-03 at 17 23 07_ff46047f](https://github.com/user-attachments/assets/d3fd25c1-79d2-4cf4-bd90-37b09384a82d)

Commands to load PDN def in magic in another terminal

```bash
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_11-37/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

Screenshot of commands ran 

![WhatsApp Image 2024-10-03 at 17 23 14_465b8aa5](https://github.com/user-attachments/assets/afaa1126-fe56-458c-a992-6e4f48eb56f3)

Screenshots of PDN def

![WhatsApp Image 2024-10-03 at 17 23 14_0562f394](https://github.com/user-attachments/assets/7cee742f-ba32-44fc-8ca5-ee5769f03256)

![WhatsApp Image 2024-10-03 at 17 23 14_d0cd72a0](https://github.com/user-attachments/assets/15a2e506-2780-4c87-bd4b-3fe780a65cda)

![WhatsApp Image 2024-10-03 at 17 23 14_5b7a27f3](https://github.com/user-attachments/assets/7058a16b-3ae5-4c15-9734-906cb7a819da)

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

Screenshots of routing run

![WhatsApp Image 2024-10-03 at 17 23 14_55db3b43](https://github.com/user-attachments/assets/5074689f-904d-4912-a12f-4560883d370c)

Commands to load routed def in magic in another terminal

```bash
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_11-37/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

Screenshots of routed def

![WhatsApp Image 2024-10-03 at 18 06 13_06c24d3e](https://github.com/user-attachments/assets/b5b6423d-750b-49f9-adeb-0a73ddc739e8)

![WhatsApp Image 2024-10-03 at 18 06 13_0b0d6888](https://github.com/user-attachments/assets/e0e11a83-bd82-46d9-94da-eda4ed719771)

![WhatsApp Image 2024-10-03 at 18 06 15_10c78fa7](https://github.com/user-attachments/assets/3cc6d07c-5faa-4730-8b51-8d159f695215)

![WhatsApp Image 2024-10-03 at 18 06 18_fef149c7](https://github.com/user-attachments/assets/a5b6880a-b17f-4105-82e4-488ffcdfb694)

![WhatsApp Image 2024-10-03 at 18 06 18_780bcad5](https://github.com/user-attachments/assets/1e2fe344-79e9-4508-bff4-ca064b58a9f5)


Screenshot of fast route guide present in `openlane/designs/picorv32a/runs/03-10_11-37/tmp/routing` directory

![WhatsApp Image 2024-10-03 at 18 06 18_48aa995e](https://github.com/user-attachments/assets/67df4402-5a1d-48bf-bccd-876768f3c509)

#### 3. Post-Route parasitic extraction using SPEF extractor.

Commands for SPEF extraction using external tool

```bash
# Change directory
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/scripts/spef_extractor


# Command extract spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_11-37/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_11-37/results/routing/picorv32a.def
```
Spef file is present in location `Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-10_11-37/results/routing `

Screenshot of spef file

![WhatsApp Image 2024-10-03 at 20 09 22_ef10dc78](https://github.com/user-attachments/assets/eff2ec0d-3633-46ce-a24a-343561dc4f29)


#### 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/03-10_11-37/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/03-10_11-37/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/03-10_11-37/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/03-10_11-37/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of commands run and timing report generated

![WhatsApp Image 2024-10-03 at 20 09 24_aff1b312](https://github.com/user-attachments/assets/78b9dcf2-6810-4c81-a716-f8047b16daec)

![WhatsApp Image 2024-10-03 at 20 09 24_292ee6e2](https://github.com/user-attachments/assets/07378088-066b-4bd2-a5c4-344875ea6945)

![WhatsApp Image 2024-10-03 at 20 09 24_cd06715e](https://github.com/user-attachments/assets/45a93aa7-035a-48da-afe9-8bd98b5a24a0)

![WhatsApp Image 2024-10-03 at 20 09 24_f0b15510](https://github.com/user-attachments/assets/7871c525-d9a0-46e6-b482-99eaaa5d2e55)






   
 








      
           
   
 


