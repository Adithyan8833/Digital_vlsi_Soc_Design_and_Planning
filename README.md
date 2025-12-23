
# Digital VLSI SoC Design and Planning

![Workshop](https://img.shields.io/badge/Workshop-RTL%20to%20GDS-black)
![Flow](https://img.shields.io/badge/Flow-OpenLANE-blue)
![OS](https://img.shields.io/badge/OS-Linux-orange)
![HDL](https://img.shields.io/badge/HDL-Verilog-red)
![EDA](https://img.shields.io/badge/EDA-OpenROAD%20%7C%20Magic-green)
![STA](https://img.shields.io/badge/Timing-OpenSTA-purple)

````
Two-Week Digital VLSI SoC Design and Planning Workshop covering the complete RTL-to-GDSII flow, organized by VSD in collaboration with
````


## DAY 1  OPENLANE
### Synthesis of pikorv32a
Synthesis of picorv32a means converting its RTL (Verilog) code into a gate-level netlist using standard cells, so the design can move to physical design steps like floorplanning and routing.
### Steps
#### 1. Open OpenLane environment
    docker

#### 2. Start interactive mode
    ./flow.tcl -interactive
#### 3. Importing all packages
    package require openlane 0.9
#### 4. Prep the Design (picorv32a)
     prep -design picorv32a
#### 5. Run Synthesis
    run_synthesis
## Screenshots
<img width="1920" height="1012" alt="1" src="https://github.com/user-attachments/assets/f9170eb5-dddd-46e7-9faf-e34ba29c3fa0" />
<img width="1920" height="1200" alt="2" src="https://github.com/user-attachments/assets/49a72209-0b1c-40b2-9339-4b1dffa219d6" />
<img width="1920" height="1200" alt="3" src="https://github.com/user-attachments/assets/df0a061b-1906-4d3e-9389-e68406b9d3af" />
<img width="1920" height="1012" alt="4" src="https://github.com/user-attachments/assets/732216ab-22ff-4395-9cdd-b2bc2023a47e" />

## Calculation of D Flip-Flop (DFF) Count and Flop Ratio(from synthesis report)

The synthesis report provides important information about the composition of the synthesized design. Two key parameters extracted from the report are:
- Total number of standard cells in the design
- Total number of D Flip-Flops (DFFs)
- Total number of cells = 14876  
- Total number of D Flip-Flops (DFFs) = 1613
  
- **Flop Ratio**
  
     1613 / 14876 = 0.108429685

- **Percentage of DFFs**

     0.108429685 × 100 = 10.84296854 %

A reasonable flop ratio indicates that the design is neither overly sequential nor overly combinational, which helps in achieving better timing and area optimization.


## DAY 2 — Floorplan
Floorplanning is the step where the chip’s size is defined and major blocks, I/O pins, and power/ground structures are placed to prepare the design for placement and routing.

Before starting the floorplanning step, ensure that synthesis has been completed.  
If not, run the synthesis process first.

#### Run Floorplan
    run_floorplan

## Screenshots
<img width="1920" height="1200" alt="1" src="https://github.com/user-attachments/assets/ae321748-eb7d-4f2f-b803-e790d80e7dae" />
<img width="1920" height="1200" alt="4" src="https://github.com/user-attachments/assets/129d5f05-c526-4ff8-ad9e-58733a85416e" />

### Calculate Die Area (from floorplan.def)

<img width="1920" height="1012" alt="5" src="https://github.com/user-attachments/assets/aa3e7baa-945a-4026-b0d3-decbf39a6415" />


From the `floorplan.def` file, note the two coordinates under the `DIEAREA` section:

Given 1000 units = 1 micron:

- Die width  = (660685 − 0) / 1000 = 660.685 μm  
- Die height = (671405 − 0) / 1000 = 671.405 μm  

Die Area = 660.685 × 671.405 = **443587.212425 μm²**

### Load Floorplan DEF in Magic
#### Commands
    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/
    
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
& = Run Magic in background so terminal remains usable.

### Screenshots of floorplan def in magic
<img width="1920" height="1012" alt="6" src="https://github.com/user-attachments/assets/ca3353c4-8278-4167-9113-02a784adea67" />

### Centrally alligned floorplan def in magic
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 19-24-32" src="https://github.com/user-attachments/assets/6fa7f538-a5c4-4cfb-9cd4-87fae41969a4" />

#### ports
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 19-33-42" src="https://github.com/user-attachments/assets/2b8d3f65-04f7-4a4c-acb6-be458834cb99" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 19-34-40" src="https://github.com/user-attachments/assets/b11ba797-8094-482e-9602-403ab8df14c6" />

### Congestion Aware Placement for 'picorv32a'
Run congestion-aware placement for the `picorv32a` design using OpenLANE:

<img width="1920" height="1012" alt="Screenshot from 2025-12-11 19-54-30" src="https://github.com/user-attachments/assets/5f215fdc-2ff0-4b1d-84ac-11c982bec008" />

### Load Placement DEF in Magic

#### commands

    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-12_12-23/results/placement/
    
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
### screenshots
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 20-04-15" src="https://github.com/user-attachments/assets/78ebe1fa-771b-47f8-8f76-c91b4b2858a3" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 20-05-32" src="https://github.com/user-attachments/assets/d11431f8-70ef-45d7-b482-2354c88f4030" />

## DAY 3 — Design of a Library Cell Using Magic Layout and NGSPICE Characterization
### 1. Clone the Custom Inverter Standard Cell Design
#### Commands
    cd Desktop/work/tools/openlane_working_dir/openlane
    
    git clone https://github.com/nickson-jose/vsdstdcelldesign
    
    cd vsdstdcelldesign
    
    # Command to open custom inverter layout in magic
    magic -T sky130A.tech sky130_inv.mag &

### Screenshots

#### Command run
<img width="1920" height="1012" alt="2" src="https://github.com/user-attachments/assets/c831f5ef-c267-4853-84da-e0441bc0e9b6" />

#### Magic layout 

<img width="1920" height="1012" alt="3" src="https://github.com/user-attachments/assets/3fa550fb-667f-4253-936a-51e8a62723ea" />

#### pmos and nmos

<img width="1920" height="1012" alt="4" src="https://github.com/user-attachments/assets/5ebe0628-6a39-4c31-9998-450d8f1b5833" />

<img width="1920" height="1012" alt="5" src="https://github.com/user-attachments/assets/56514e0f-8a6c-4fa1-bffa-4759c7790dce" />

###  Spice extraction of inverter in magic.

#### COmmands
    pwd
    extract all
    ext2spice cthresh 0 rthresh 0
    ext2spice

#### Screenshots

<img width="1920" height="1012" alt="6" src="https://github.com/user-attachments/assets/cc86847a-fe44-40f8-936b-71a5850157f8" />

<img width="1920" height="1012" alt="7" src="https://github.com/user-attachments/assets/a944f357-322d-4bf7-8990-9acf99fa49e7" />

<img width="1920" height="1012" alt="8" src="https://github.com/user-attachments/assets/622230c1-e89e-4418-9ea8-fa787bba9219" />

<img width="1920" height="1012" alt="9" src="https://github.com/user-attachments/assets/a4144f0c-8d31-438e-bd05-1f425c3e28f5" />

#### Screenshot of created spice file

<img width="1920" height="1012" alt="10" src="https://github.com/user-attachments/assets/bce66260-7b88-45ab-bc96-648c57dc61d2" />

#### Measuring unit distance in layout grid

<img width="1920" height="1012" alt="11" src="https://github.com/user-attachments/assets/dc5b0b61-cc30-403c-ba41-3fd9c8fdb8dc" />

#### Final edited spice file
<img width="1920" height="1012" alt="1" src="https://github.com/user-attachments/assets/fa0014b0-1e1d-4f1c-83aa-04cec4e79ad4" />



###  Post-layout ngspice simulations.

#### To install ngspice if the system dosent have
    sudo apt install ngspice

#### Commands for ngspice simulation
    ngspice sky130_inv.spice
#### Load the Plot in NGSPICE
    plot y vs time a

#### Screenshots of ngspice run

<img width="1920" height="1012" alt="12" src="https://github.com/user-attachments/assets/6045f225-51c6-4af9-8479-48bb9ceeed72" />

<img width="1920" height="1012" alt="13" src="https://github.com/user-attachments/assets/5620b5bc-ace5-4cbf-8501-01026c1ccb31" />

#### Screenshot of generated plot

<img width="1920" height="1012" alt="14" src="https://github.com/user-attachments/assets/d8fb248a-16bd-4dd2-bdbe-81eced603939" />

### Identify and Fix DRC Issues in the Old Magic Tech File (SkyWater Process)
For detailed DRC and layout rules, refer to the SkyWater Sky130 Periphery Rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

#### Commands to Download and View the Corrupted SkyWater Magic Tech File
    cd
    wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
    tar xfz drc_tests.tgz
    cd drc_tests
    ls -al
    gvim .magicrc
    magic -d XR &
#### Screenshots of commands

<img width="1920" height="1012" alt="2" src="https://github.com/user-attachments/assets/ffb41ad4-8a8f-44fd-9a53-96fdb074d528" />
<img width="1920" height="1200" alt="3" src="https://github.com/user-attachments/assets/a6cdaa08-be87-4a13-84a5-31d6a007dec3" />

#### Screenshot of .magicrc file

<img width="1920" height="1200" alt="4" src="https://github.com/user-attachments/assets/72c3526f-26a3-4cde-b5ce-c75a76042e1c" />

### Correction of Incorrectly Implemented `poly.9` Simple Rule
#### Screenshot of poly rules
<img width="1920" height="1200" alt="5" src="https://github.com/user-attachments/assets/1c460a93-f905-4062-bc01-b11e210bcebe" />

#### Screenshot of poly9
<img width="1920" height="1012" alt="7" src="https://github.com/user-attachments/assets/17b5ff8e-a405-496a-8e9e-6c52394c56de" />

<img width="1920" height="1200" alt="6" src="https://github.com/user-attachments/assets/6c0989bd-94d3-47cd-a19c-1b3575f08fd6" />

#### New commands inserted in sky130A.tech file 
<img width="1920" height="1012" alt="8" src="https://github.com/user-attachments/assets/a13356e4-1baa-49c4-8979-d6f02b2a7295" />
<img width="1920" height="1012" alt="9" src="https://github.com/user-attachments/assets/a3ceac69-967f-40de-bbbc-aa2bf74c6eeb" />
<img width="1920" height="1012" alt="10" src="https://github.com/user-attachments/assets/14734916-94f8-4202-9a31-d50f276593c2" />

#### Commands to Run in tkcon Window

    # Load the updated Magic tech file
    tech load sky130A.tech

    # Re-run DRC to check for updated errors
    drc check

    # Select the error region and view DRC messages
    drc why

<img width="1920" height="1012" alt="11" src="https://github.com/user-attachments/assets/5bfd875e-bb36-4b98-b569-cfe819fcf5a5" />

#### Screenshot of difftap rules
<img width="1090" height="549" alt="Screenshot 2025-12-13 173017" src="https://github.com/user-attachments/assets/fb8e9068-e994-48b6-a0dc-ab139900e7c6" />

### Incorrectly implemented nwell.4 complex rule correction
#### Screenshot of nwell rules
<img width="1069" height="520" alt="image" src="https://github.com/user-attachments/assets/fcb70999-ebc9-458e-86f0-a5372870aafc" />

### Incorrect Implementation of `nwell.4` Rule
<img width="1920" height="1012" alt="13" src="https://github.com/user-attachments/assets/597c9798-2955-4e59-b1b8-e5ee010c9c73" />

#### New commands inserted in sky130A.tech file to update drc
<img width="1920" height="1012" alt="14" src="https://github.com/user-attachments/assets/404896fe-83ec-4309-a5c1-e52b19b73f01" />

#### Commands to run in tkcon window
    tech load sky130A.tech
    drc style drc(full)
    drc check
    drc why

<img width="1920" height="1012" alt="15" src="https://github.com/user-attachments/assets/2e8ba57b-4f63-46cc-8e56-12aa1d78a39b" />

## Day 4 — Pre-Layout Timing Analysis and Importance of a Good Clock Tree
Pre-Layout Timing Analysis checks whether the design meets timing requirements before physical layout by estimating delays, while a good clock tree ensures the clock reaches all registers with minimal skew and delay, enabling reliable and high-speed operation.

### Fix Minor DRC Errors and Final Verification
#### Commands to open the custom inverter layout
    cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
    magic -T sky130A.tech sky130_inv.mag &

#### Screenshot of tracks.info of sky130_fd_sc_hd
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-13-45" src="https://github.com/user-attachments/assets/0b603512-4657-4117-9df4-754645e353ec" />


#### Commands for tkcon window to set grid as tracks of locali layer
    grid 0.46um 0.34um 0.23um 0.17um
#### Screenshot of commands run
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-17-47" src="https://github.com/user-attachments/assets/6b7f5e3a-8ca9-4935-bb6c-70cf0d832c53" />


#### Condition 1 verified
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-18-54" src="https://github.com/user-attachments/assets/2a2cf3ab-b38b-42e2-b808-29f433047d7a" />




#### Condition 2 verified
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-21-26" src="https://github.com/user-attachments/assets/644161d4-7d39-498c-a64e-34d1e8ebc6fa" />

#### Condition 3 verified

<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-22-38" src="https://github.com/user-attachments/assets/b603ba7b-91be-4ddd-a2b2-f3d04ae535f8" />


#### Save the finalized layout with custom name and open it.
    # Command to save as
    save sky130_vsdinv.mag
#### Command to open the newly saved layout
    magic -T sky130A.tech sky130_vsdinv.mag &
#### Screenshot of newly saved layout


#### Generate lef from the layout.
    lef write
#### screenshot
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-30-49" src="https://github.com/user-attachments/assets/4089b64a-a9db-461d-9958-bfc52537b40a" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-31-28" src="https://github.com/user-attachments/assets/2d4590d9-595b-4a5b-b8f2-e7257442fc15" />


#### Screenshot of newly created lef file
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 23-50-03" src="https://github.com/user-attachments/assets/5f2b1fa1-dcbe-4f32-bc35-4e7c6c420bb1" />



### Copy LEF and LIB Files to `picorv32a` Design Source Directory

Copy the newly generated LEF file and the required library (`.lib`) files into the `src` directory of the `picorv32a` design.

```bash
# Copy the LEF file
cp sky130_vsdinv.lef \
~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Verify LEF file copy
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy required LIB files
cp libs/sky130_fd_sc_hd__* \
~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Verify LIB files copy
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
````
#### Screenshot of commands run

<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-40-19" src="https://github.com/user-attachments/assets/3df54c03-9bae-4358-9a93-175693a8ad48" />


#### Edited config.tcl 

<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-47-10" src="https://github.com/user-attachments/assets/f374718f-a0f1-4c84-a79c-8ed74319550d" />


#### Run openlane flow synthesis with newly inserted custom inverter cell.
before run_syntesis add 
````
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

````
#### Screenshots of commands run
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-50-03" src="https://github.com/user-attachments/assets/6e43fb30-8420-4f86-9cb6-3731c68b4b95" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 18-54-29" src="https://github.com/user-attachments/assets/0bcdc3c4-6660-4dd0-8be9-7baf124722ab" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-08-29" src="https://github.com/user-attachments/assets/8b83ad0e-574c-4ffa-a476-06b572e29cdf" />





#### Commands to view and change parameters to improve timing and run synthes
    $::env(SYNTH_STRATEGY)
    set ::env(SYNTH_STRATEGY) "DELAY 3"
    echo $::env(SYNTH_BUFFERING)
    echo $::env(SYNTH_SIZING)
    set ::env(SYNTH_SIZING) 1
    echo $::env(SYNTH_DRIVING_CELL)
    run_synthesis
#### Screenshot of merged.lef
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-13-51" src="https://github.com/user-attachments/assets/6bde8db3-5df3-49a2-bbe8-78dd6e6f6988" />

#### run_floorplan

#### screenshot
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-18-49" src="https://github.com/user-attachments/assets/183d2762-466a-4afa-8ba8-7dcbb9f6b469" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-19-59" src="https://github.com/user-attachments/assets/07ed82ad-68b6-4806-967a-219a9173b357" />


#### Commands to load placement def in magic in another terminal
    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-12_14-07/results/placement/
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

#### screenshot
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-29-20" src="https://github.com/user-attachments/assets/5ecfbae2-41a8-44fc-affe-155f8679c62d" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-24-55" src="https://github.com/user-attachments/assets/90aafffd-0ca4-4286-a1c2-12590ca53fb7" />


#### screenshot of internal layers
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 19-30-06" src="https://github.com/user-attachments/assets/6e535a8f-b0f8-4c0c-b579-53c99710ca66" />

#### pre_sta.conf
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 23-13-28" src="https://github.com/user-attachments/assets/ad97c26a-db24-4e41-ab55-6547135146ed" />


#### my_base.sdc
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 23-14-33" src="https://github.com/user-attachments/assets/ab9737f9-bed0-4df0-90dd-2deac9da4a08" />


#### Commands to run STA in another terminal
    cd Desktop/work/tools/openlane_working_dir/openlane
    sta pre_sta.conf

#### screenshots


<img width="1920" height="1012" alt="Screenshot from 2025-12-13 23-14-44" src="https://github.com/user-attachments/assets/a52f3926-ce06-4306-b9d9-91863fb0e981" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-13 23-14-50" src="https://github.com/user-attachments/assets/282ceb10-4e8e-4ca6-a3c8-310f8be4cdc8" />

## Day 5 — Final Steps for RTL2GDS Using TritonRoute and OpenSTA
This section covers detailed routing using TritonRoute and post-route timing analysis using OpenSTA to complete the RTL-to-GDSII flow.
### 1. Power Distribution Network (PDN) Generation and Exploration
#### Commands
#### Running oprnlane
    cd Desktop/work/tools/openlane_working_dir/openlane
    docker
    ./flow.tcl -interactive
    package require openlane 0.9
    prep -design picorv32a
#### include newly added lef
    set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
    add_lefs -src $lefs
    # Command to set new value for SYNTH_STRATEGY
    set ::env(SYNTH_STRATEGY) "DELAY 3"
    # Command to set new value for SYNTH_SIZING
    set ::env(SYNTH_SIZING) 1
#### Run synthesis and floorplan
    # 1ts
    run_synthesis
    #2nd
    init_floorplan
    place_io
    tap_decap_or
    #3rd
    run_placement
#### Run CTS
    run_cts
#### Do power distribution network
    gen_pdn
#### Screenshots of power distribution network run
<img width="1920" height="1012" alt="1" src="https://github.com/user-attachments/assets/575d59e8-41b3-49ec-973b-1f4a3a3cfbe7" />
<img width="1920" height="1012" alt="2" src="https://github.com/user-attachments/assets/55549ae7-b76c-426d-998f-3916f97f74d9" />

#### Commands to load PDN def in magic
    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-12_18-36/tmp/floorplan/
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
#### Screenshots of PDN def

<img width="1920" height="1012" alt="3" src="https://github.com/user-attachments/assets/bc0a6cb7-c0fd-4d50-9622-f33f83345c57" />
<img width="1920" height="1012" alt="4" src="https://github.com/user-attachments/assets/422ad695-49e8-42e8-95dc-3e1069356d9d" />
<img width="1920" height="1012" alt="5" src="https://github.com/user-attachments/assets/9e01f71d-9f42-489f-848d-9928759843a9" />
<img width="1920" height="1012" alt="6" src="https://github.com/user-attachments/assets/5434c213-cf09-47f6-adee-f1524f748aa3" />

### Perform Detailed Routing Using TritonRoute and Explore the Routed Layout
#### Commands
    # Check value of 'CURRENT_DEF'
    echo $::env(CURRENT_DEF)
    
    # Check value of 'ROUTING_STRATEGY'
    echo $::env(ROUTING_STRATEGY)
    
    # Command for detailed route using TritonRoute
    run_routing

#### Screenshots
<img width="1920" height="1012" alt="7" src="https://github.com/user-attachments/assets/5ad57138-a9bd-424a-8e8a-d35bac85d39e" />
<img width="1920" height="1012" alt="8" src="https://github.com/user-attachments/assets/5de0ac9d-683a-4a67-8902-a60585d4f83c" />
<img width="1920" height="1012" alt="9" src="https://github.com/user-attachments/assets/c61380fb-c603-4f72-a6cd-7263f3c211b6" />
<img width="1920" height="1012" alt="10" src="https://github.com/user-attachments/assets/50e47357-66de-4636-9813-c5c5a68b8815" />


#### Commands to load routed def in magic
    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-12_18-36/results/routing/
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

#### Screenshots of routed def
<img width="1920" height="1012" alt="11" src="https://github.com/user-attachments/assets/714c5bc5-29a9-4f07-8786-d0fe1aac7f97" />
<img width="1920" height="1012" alt="12" src="https://github.com/user-attachments/assets/514d46d8-e7cd-4bbd-8ae0-39c34d3ad8aa" />
<img width="1920" height="1012" alt="13" src="https://github.com/user-attachments/assets/910dfa49-44e0-4f36-8e2e-509b05def33d" />

#### Screenshot of FastRoute Guide
<img width="1920" height="1012" alt="14" src="https://github.com/user-attachments/assets/1f80f633-7e1b-4d98-8b41-3624c08f92ec" />

### Post-Route parasitic extraction using SPEF extractor
#### Commands
    cd Desktop/work/tools/SPEF_EXTRACTOR
    python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-12_18-36/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-12_18-36/results/routing/picorv32a.def

### Post-Route OpenSTA Timing Analysis
#### Commands
    # Command to run OpenROAD tool
    openroad

    # Reading lef file
    read_lef /openLANE_flow/designs/picorv32a/runs/13-12_18-36/tmp/merged.lef

    # Reading def file
    read_def /openLANE_flow/designs/picorv32a/runs/13-12_18-36/results/routing/picorv32a.def

    # Creating an OpenROAD database to work with
    write_db pico_route.db

    # Loading the created database in OpenROAD
    read_db pico_route.db

    # Read netlist post CTS
    read_verilog /openLANE_flow/designs/picorv32a/runs/13-12_18-36/results/synthesis/picorv32a.synthesis_preroute.v

    # Read library for design
    read_liberty $::env(LIB_SYNTH_COMPLETE)

    # Link design and library
    link_design picorv32a

    # Read in the custom sdc we created
    read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

    # Setting all cloks as propagated clocks
    set_propagated_clock [all_clocks]

    # Read SPEF
    read_spef /openLANE_flow/designs/picorv32a/runs/13-12_18-36/results/routing/picorv32a.spef

    # Generating custom timing report
    report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

    # Exit to OpenLANE flow
    exit

#### Screenshots

<img width="1920" height="1012" alt="15" src="https://github.com/user-attachments/assets/e2fc401d-13c6-4445-ba21-8c8a29564194" />
<img width="1920" height="1012" alt="16" src="https://github.com/user-attachments/assets/7ce3e34d-3582-4b85-a144-15c81c98b594" />
<img width="1920" height="1012" alt="17" src="https://github.com/user-attachments/assets/f682ed3f-030d-4cb3-9c57-bc3bbeafaef2" />
<img width="1920" height="1012" alt="18" src="https://github.com/user-attachments/assets/093bde72-d3ba-43ab-ae46-414aa55d8543" />

# END























