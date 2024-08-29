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
![image](https://github.com/user-attachments/assets/9f8aff89-d320-4fb6-9c60-8433a7821a14)

Keys used in magic:
Key  | Description
| :---: | :---
S | Select the instance or object
V | center the design
Z | Zooms in
arrows | Move around in the design
left click and the right click other location | Creates a box for reference to zoom in
'what' in tkcon | query attributes of object selected







