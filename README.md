

# Digital VLSI SoC Design and Planning

## DAY 1  OPENLANE
### Synthesis of pikorv32a
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

## Calculation of Flop Ratio and DFF % (from synthesis report)

- **Flop Ratio**
1613 / 14876 = 0.108429685

- **Percentage of DFFs**
0.108429685 × 100 = 10.84296854 %

## DAY 2 — Floorplan

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

    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/
    
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
### screenshots
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 20-04-15" src="https://github.com/user-attachments/assets/78ebe1fa-771b-47f8-8f76-c91b4b2858a3" />
<img width="1920" height="1012" alt="Screenshot from 2025-12-11 20-05-32" src="https://github.com/user-attachments/assets/d11431f8-70ef-45d7-b482-2354c88f4030" />

## DAY 3 — ## Design of a Library Cell Using Magic Layout and NGSPICE Characterization
### ### 1. Clone the Custom Inverter Standard Cell Design
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
