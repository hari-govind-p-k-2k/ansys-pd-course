# **Reference**

Git Hub link : [https://github.com/efabless/openlane](url)

Youtube video : [https://www.youtube.com/watch?v=EczW2IWdnOM&list=PLUg3wIOWD8yoZCg9XpFSgEgljx6MSdm9L&index=1](url)

OpenLane Flow:

![image](https://github.com/user-attachments/assets/109da779-720b-4dec-8524-6f5c3d66a11b)



# **Day 1**

Output to be found out : 

## Flop ratio = 1613/14876 = 0.0108429 = 10.8429 %

_Snapshot :_

![image](https://github.com/user-attachments/assets/d7540334-892e-4265-85b0-06fa1138d81f)



Items done on Lab based on Day 1 material 

  1. Went through **flow.tcl, tech lef** and other inputs

  ![image](https://github.com/user-attachments/assets/0f67c776-cb24-4c9b-bde0-313988736b93)

  2. Opened OpenLane, went through the contents of designs dir (**config.tcl**, **sky130A_sky130_fd_sc_hd_config.tcl**) and setup the design data (prep -design picorv32a)
     
     ![image](https://github.com/user-attachments/assets/4f627763-67f2-435e-acb3-30556b1da4b8)

     ![image](https://github.com/user-attachments/assets/7e02b30b-fcbf-4d4f-93e4-2a44c7ffe02e)

     ![image](https://github.com/user-attachments/assets/6a92d38a-ffe3-4b63-a55d-efd18a432b69)


  3. Reviewed the contents of the generated runs dir inside picorv32a design dir (**merged.lef** inside tmp dir that has design+tech lef, **config.tcl** which has all the settings used on top of the ones provided in design dir)
     ![image](https://github.com/user-attachments/assets/360d4501-f2d4-400c-9198-9b9c1861a508)

  
  4. Ran synthesis (run_synthesis)
  
     ![image](https://github.com/user-attachments/assets/55dc677b-d51f-4a44-9768-a26b934199be)
  
     
  6. Reviewed the above Git Hub Link and video while synthesis progressed


     
  7. After synthesis completed successfully, opened the results dir and reviewed **picorv32a.syntheseis.v** file.

  ![image](https://github.com/user-attachments/assets/739394be-224c-4a9d-ad75-03276b1dd21f)

     
  8. Also reviewed reports (**1-yosys_4.stat.rpt** from which we found the flop ratio, **2-opensta.timing.rpt**,i.e, the timing file)

  ![image](https://github.com/user-attachments/assets/14b20f02-ea5b-4fc9-af5f-b2796af33c00)



    
# **Day 2**

# 1**.Chip Floor Planning Considerations**

## **a. Utilization Factor and Aspect Ratio**

Core : Inner area of the chip
Die : Peripheral region of chip

Important functioning logic to be placed in the core. All logic cells considered rectangular boxes based on dimension of cell, this is to calculate area of netlist.

Utilization Factor = Area occupied by Netlist/Total area of core
Aspect Ratio = height of core / width of core (if aspect ratio is 1, chip is square else rectangle)


## **b. Concept of pre-placed cells**


Complex combinational logic cut up into smaller blocks. As we know the logic,we know where we have seperated the logic and what the connectivty between the blocks are. These blocks are then implemented seperately. These blocks can also be resued in same design if required. Each block is considered an IP or module. Som examples are memory, MUX, Comparator etc. The placement of these modules fixed before other cells, hence these are called pre-placed cells. The placement of pre-placed are fixed and cannot be moved by automated place & route tools.


## **c. De-coupling capacitors**


Once pre-placed cells are placed in core, it is necessary to ensure that pre-placed cells are surrounded by decoupling capacitors(Decaps). Decaps ensure there is sufficient voltage for the logic.

if decaps are not present, voltage drop due to the wires from supply can cause the voltage at logic to be in the undefined region. This leads to missing of activity of logic. So to ensure that, voltage at logic is within the noise margin (for 1, between Voh and Vih; for 0, between Vil and Vol) decaps are used

Surround Decaps to logic ensure that voltage is supplied instantenously to the logic without any delay. The decaps charge when logic is inactive and discharge when logic is active


## **d. Power Planning**


Using a single supply can lead to voltage droop and ground bounce especially in logic far from the supply. This will occur even if decaps are present decaps are also close to logic and thus can charge only upto a certain limit. Hence, we need to make a mesh/grid of the supply so that multiple tapin points are available for logic close to it so that the mesh/grod is robust and stable.


## **e. Pin placement and logic cell placement placement blockage**

The area between core and die is reserved for pin placements. As a general rule, input pins are placed on the left or top of the die while output pins are placed at the right or bottom side of the die. Pins placement also factors in the location of IPs are they are pre-placed. 

Clocks pins are larger than signal pins as they are connected to large number of logic and thus need less resistance.

Once pins are placed, the rest of the area between core and die are blocked off for cell logic placement so that tool does not place any logic cells in that region. It is reserved for pins of the placed logic.

With this, floorplan is done and design is ready for placement and routing


## **f. Floorplan using OpenLane**

After synthesis, we do floorplanning. Before actual step, review the README.md file inside working dir to find switches to control various flow parameters

![image](https://github.com/user-attachments/assets/58432398-2a15-4645-85d2-2bb7c4f2a623)

![image](https://github.com/user-attachments/assets/8ca9e7c9-2b6a-41f0-b56f-399b8efb5706)

![image](https://github.com/user-attachments/assets/b983225b-f271-4f01-adf8-735a4aa305b6)


Review the **floorplan.tcl, config.tcl** and **design specific config.tcl**(from design dir) 
![image](https://github.com/user-attachments/assets/a2be438e-aa82-4579-9add-e8787292a510)

![image](https://github.com/user-attachments/assets/d2eb5b55-fb37-4d52-9dd5-3f61aab0ff52)

![image](https://github.com/user-attachments/assets/80a21e82-d8eb-46a7-86f2-f02f697ea72a)

Run the floorplan (run_floorplan)

![image](https://github.com/user-attachments/assets/cf254d90-6fd3-438d-bdb4-4aa676073821)

![image](https://github.com/user-attachments/assets/70e67974-d195-450c-a120-f200536da60f)

Review the floorplan output DEF file

![image](https://github.com/user-attachments/assets/14342487-378c-4ed9-b07e-8056a48a10f6)

![image](https://github.com/user-attachments/assets/ab3f51f3-3e41-4c42-8172-1484e5a0569d)

Found DIE AREA from DEF file (DB unit 1000 so to get actual die area in micron, divide the reported DIEAREA value by 1000)

![image](https://github.com/user-attachments/assets/14ee98cb-e654-44ca-9f2f-128bc01471c2)


### Die Area from DEF file = (660685/1000) * (671405/1000) = 660.685 * 671.405 = 443587.21 sq microns

## Using Magic tool to view the created the DEF 

Following command used to opening magic:
![image](https://github.com/user-attachments/assets/93451f5e-6f07-481d-b6e4-38d531df7b13)


Keys used in magic:
Key  | Description
| :---: | :---
S | Select the instance or object
V | center the design
Z | Zooms in
arrows | Move around in the design
left click and the right click other location | Creates a box for reference to zoom in
'what' in tkcon | query attributes of object selected


Layout window:

![image](https://github.com/user-attachments/assets/b9a25eb1-5782-4c9c-88f5-c7527cf78636)

As seen all the instances in lower left. Tap cells diagonally equidistant and all pins random but equidistant as FP_IO_MODE is set to 1.

Querying attributes of a decap:

![image](https://github.com/user-attachments/assets/42fbfa22-03a0-4c18-9cc7-f04edf854da8)


## Placement using OpenLane

Till now, we only placed the Macros/ IPs along with IO ports. We did not place the standard cells. The idea is to take library information of cells in the design to implement the netlist and come up with an optimal placement which could possibly pass timing. The instances are placed according to the netlist on the design instances close to the pins are placed near the IO pads.

If there are hard paths then placement is performed and then based on wireload estimations cap is calculated and if slew threshold is not met buffers are added to regenerate the signals and help with signal integrity.

To do this, we do placement. Placement is of two types:

1. Global placement : Coarse placement and no legalizations considered
2. Detailed placement: legalizations are done (for example, where instances are placed according to standard cell rows.)

To run placement, the command is run_placement
![image](https://github.com/user-attachments/assets/21400460-3e88-4a05-afaa-63643b8d20e5)

Placed layout window of magic:

![image](https://github.com/user-attachments/assets/ff711ec6-dbea-405f-b6ea-e10571a7a0c7)


## Cell Design Flow

Library is where the information related to standard cells, macros/IPs, decap cells are present.	Bigger buff means higher drive strength & higher threshold voltage which in turn determines the speed of the cell. Bigger threshold voltage cell will take more time to switch. High Vt (hVt) low Vt (lVt) is based on the threshold voltage. Same size cells can have different Vts.

The Cell design flow is divided into 3 parts: 
1. Inputs (PDK, DRC& LVS rules, spice models, library & user-defined specifications)
2. Design steps
3. Outputs

Tech file contains all the foundry related rules.



The Cell height is defined as the distance between Vdd & Vss line. The cell designers need to make sure that the cell height is exactly fit into the Vdd & Vss distance. Cell width depends on the timing information.	Metal layers to connect a cell is defined by the user.	Pin location requirements also have to be taken into account by the cell designer

Design steps are divided into three: 
1. Ckt design
2. Layout design
3. Characterization

**Ckt Design**: Depending on the drive strength, the NMOS & PMOS needs to be sized.

**Layout Design: - characterization**: After the PMOS & NMOS network graph is obtained, make an euler’s path. Euler’s path is a path that is traced only once. Place the transistors based on the euler’s path and draw a stick diagram. Poly is vertically placed for each gate input that used in the circuit. The remaining connections are done based on the circuit. 

**Output**: CDL, GDSII file, LEF, Extracted parasitics (.cir)  - Timing, power .libs, noise, function

**characterization flow**
Inputs available: subcircuit models, ckt connections, models for NMOS & PMOS

Flow: 
1. Read in the models
2. Read the extracted spice netlist
3. Recognize the behavior of the buffer
4. Read the subcircuit of the inverter
5. Attach the necessary power sources
6. Apply the stimulus
7. Necessary output/load capacitances (max to min range)
8. Provide the necessary simulation commands (.tran, .dc etc.,) 

Next step is feed in all the 1-8 inputs as a characterization file in Guna which gives the output models – timing, noise, power .libs, function

## **4.	Timing Characterization**

### **Timing threshold definitions**

1.	slew_low_rise_thr – closer to low voltage and rising, typically 20% above

2.	slew_high_rise_thr – closer to the high voltage and rising, typically 20% to the high voltage

3.	slew_low_fall_thr – like above but falling

4.	slew_high_fall_thr - like above but falling

5.	in_rise_thr – 50% (typical) of the input voltage rise step

6.	in_fall_thr – 50% of the input voltage fall step

7.	out_rise_thr – like that for output voltage

8.	out_fall_thr -  like that for output voltage

9.	To calculate the delay, 50% input to output can be used

### **b.	Propagation Delay & transition time**

1.	Delay is out_*_thr – in_*_thr

2.	Wrong threshold points taken can produce wrong delay values, in some cases negative numbers which is simply not correct

3.	Even with proper choice of threshold, it is possible to see negative delays. For e.g., a long line with big capacitance and then a buffer can lead to negative delay

4.	Transition time: input slew = slew_high_rise_thr - slew_low_rise_thr & output slew = slew_high_fall_thr - slew_low_fall_thr



# Day 3








