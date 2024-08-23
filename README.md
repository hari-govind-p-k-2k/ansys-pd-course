**Reference**

Git Hub link : [https://github.com/efabless/openlane](url)

Youtube video : [https://www.youtube.com/watch?v=EczW2IWdnOM&list=PLUg3wIOWD8yoZCg9XpFSgEgljx6MSdm9L&index=1](url)


**Day 1**

Output to be found out : 

Flop ratio = 1613/14876 = 0.0108429 = 10.8429 %

_Snapshot :_

![image](https://github.com/user-attachments/assets/d7540334-892e-4265-85b0-06fa1138d81f)



Items done on Lab based on Day 1 material 

  1. Went through **flow.tcl, tech lef** and other inputs
  2. Opened OpenLane, went through the contents of designs dir (**config.tcl**, **sky130A_sky130_fd_sc_hd_config.tcl**) and setup the design data (prep -design picorv32a)
  3. Reviewed the contents of the generated runs dir inside picorv32a design dir (**merged.lef** that has design+tech lef, **config.tcl** which has all the settings used on top of the ones provided in design dir)
  4. Ran synthesis (run_synthesis)
  5. Reviewed the above Git Hub Link and video while synthesis progressed
  6. After synthesis completed successfully, opened the results dir and reviewed **picorv32a.syntheseis.v** file.
  7. Also reviewed reports (**1-yosys_4.stat.rpt** from which we found the flop ratio, **2-opensta.timing.rpt**,i.e, the timing file)


**Day 2**

1**.Chip Floor Planning Considerations**

**a. Utilization Factor and Aspect Ratio**

Core : Inner area of the chip
Die : Peripheral region of chip

Important functioning logic to be placed in the core. All logic cells considered rectangular boxes based on dimension of cell, this is to calculate area of netlist.

Utilization Factor = Area occupied by Netlist/Total area of core
Aspect Ratio = height of core / width of core (if aspect ratio is 1, chip is square else rectangle)


**b. Concept of pre-placed cells**

Complex combinational logic cut up into smaller blocks. As we know the logic,we know where we have seperated the logic and what the connectivty between the blocks are. These blocks are then implemented seperately. These blocks can also be resued in same design if required. Each block is considered an IP or module. Som examples are memory, MUX, Comparator etc. The placement of these modules fixed before other cells, hence these are called pre-placed cells. The placement of pre-placed are fixed and cannot be moved by automated place & route tools.

**c. De-coupling capacitors**

Once pre-placed cells are placed in core, it is necessary to ensure that pre-placed cells are surrounded by decoupling capacitors(Decaps). Decaps ensure there is sufficient voltage for the logic.

if decaps are not present, voltage drop due to the wires from supply can cause the voltage at logic to be in the undefined region. This leads to missing of activity of logic. So to ensure that, voltage at logic is within the noise margin (for 1, between Voh and Vih; for 0, between Vil and Vol) decaps are used

Surround Decaps to logic ensure that voltage is supplied instantenously to the logic without any delay.

**d. Power Planning**



