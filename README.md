# Advanced-Physical-Design-Using-Skylane-130-nm
# Contents
- Day 1 
   - Introduction
   - RISC-V
   - ABIs
   - OpenLANE
   - Getting started with the platform
- Day 2
   - pre placed cells
   - floorplanning rules
   - timing characterizations
- Day 3
   - magic tool
   - placement of the inverter
   - 16 step cmos mask process
   - drc checkers
- Day 4
   - openSTA
   - clock tree synthesis 
   - openroad
   - 
# DAY 1 
## Introduction
This course gives the basic overview of designing an IP(Intellectual Property) using the opensource tool OpenLANE from skywater foundaries in collaboration with google.
The IPs which are being devolped will be of the scale of 130 nm. The designing rules are defined by the PDKs (Process Design Kit) which is not generally opensourced due to the integrity and also due to security concerns. We will work with a design reference namely picorv32a which is based on RISC-V ISA.The computers can only understand binary digits (that is 1s or 0s) to convert a program into that language we need an flow which takes in ISA converts it RTL then this is converted into layout. All these above steps require an interface and each interface needs to be designed such that the communication between each step is smooth.


### Opening my first interactive window
```linux
./flow.tcl -interactive 
```
![opening my  interactive window](https://user-images.githubusercontent.com/58397908/114047038-ff4a8900-98a6-11eb-8797-41705a527344.jpg)

## RISC-V
RISC-V(stands for Reduced Instruction Set Computer Five) is an instruction set architecture which is open sourced for mainly learning and understanding of a computer architecture was devolped at University of California, Berkley. This ISA generally comes under little endian memory addressing mode or system which basically means Most Significant Bit is stored last and the Least Significant Bit is stored first. The memory in generally addresses a byte of data and there are only 32 regesters in RISC-V architecture meaning we can store the data in these regesters while running other commands such that it is efficent. The core(picorv32a) is a pre designed SoC which is generally used as a reference.

## ABI
ABI stands for Application Binary Interface which is generally also called as System Call Interface is generally one of the interfaces which helps us run our commands on the hardware.

## OpenLANE
OpenLANE is an open source tool which basically runs all the program from RTL to GDSII. This tools helps us decide the ASIC Flow: RTL to GDSII(format of final layout)
Processes are 
1. Synthesis (RTL in)
RTL (HDL) gets converted into a circuit or digital design from a standard cell library.
In openlane we have used 
  - OpenSTA : For post-synthesis analysis which is generally dumped onto the verilog file
  - Yosys : It is used for mapping the RTL netlist
  - abc : It generally uses PDK specification to map the cells accordingly 
2. Floor/Power Planning
Chip-Floor-Planning: Partition the chip die between different system building blocks and place the I/O Pads.
Marco-Planning: Generally the macros have to be fixed before we place any IPs
3. Placement : Genererally done in two steps first it places them globally and then correctly places them to avoid any overlapping
  - Ioplacer - It places the macros and connects I/O ports to the pins(pads)
5. Clock-Tree Synthesis
  - TritonCTS is the tool used for this step, It only creates a network of clock which is connected in routing step
6. Routing : The most time consuming step as a result can be said as one of the most important step. We route the IPs and macros as well as connect them to their respective pins without violating any rules which can for example be routes which coinciede but have to be in differnet layers.
7. Sign Off (GDSII out) : This step basically chhecks for any DRC errors or any such related errors.

### The figure depiects the location where we have placed the temporary files
![tem files](https://user-images.githubusercontent.com/58397908/114047080-07a2c400-98a7-11eb-9590-155454da816f.jpg)

### After the synthesis step we create a folder which stores all the data from RTL to GDSII for that specifications
![creation of runs file](https://user-images.githubusercontent.com/58397908/114047097-0c677800-98a7-11eb-879d-30ee18b7c764.jpg)

## The sysnthesis report which is generated after the run the commands
```linux
package require openlane 0.9
prep -design picorv32a -tag foder_name (creates in runs section)
run_synthesis
```
![synthesis report](https://user-images.githubusercontent.com/58397908/114047129-11c4c280-98a7-11eb-8443-0678f9bc13f8.jpg)


# Day 2

## Floorplan
1. Core - The general idea is to encapsulate it with a die so that we get a protection. Intial netlist uses less space so that we can place other cells for optimisation. 
Dimensions of the chip mostly depends on the dimensions of the standard cells. (Width and Height generally determines the dimensions of the chip) We determine its ultilization factor from the following equations
```
Utilization Factor = (Area occuipied by Netlist)/(Total Area of the Core)
```
Ideally we go for a (50 - 60) % so that it is easier for the tools to work.
The other factor is 
```Aspect ratio (If it other than 1 it signifies rectangle) = height/width
```
2. Pre placed cells
They can be macros or ips and alo are a part of the top level netlist. The location placing or arrangements of the cells are done only once because of their functionality which does not change, As a result they are placed before routing and placement of other cells

3. Decoupling capacitors are generally used incase the power supply for the gate falls under the undefined region in noise margin level which may cause it fall under both logic 1 or 0. It is a capacitor which is typically filled with a lot charge which will result in less loss of voltage/current.

4. Power planning
If supply is being provided from one point we face two issues namely
  - Ground bounce (discharging)
  - Voltage droop (charging)
Solution is to use multiple vdd vss lines creating power meshes which blocks of suc errors and allows the core to run smoothly.

We need to take care of below conditions to ensure that the floorplan does not have any errors :
- Giving shapes and sizes for the gates
- Binding the netlist with physical cells
- Libraries general hold the data for width delay and condtion of the cell which generally comes in different sizes (Bigger cells mean it runs faster)
- Depending on the I/O ports, Cells are placed to avoid any delays. Problems are generally solved by the technique called as optimize placement which basically estimates the length and capacitance on the basis of this information we try to insert repeaters to maintain the signal integrity.

```linux 
run_floorplan

shows the value or the name asks for it
echo $::env(CURRENT_DEF) shows the which def file it is holding

we can make changes by using
set ::env(CLOCK_PERIOD) _value_

magic -T //loaction of magic T tech file// read lef //location of merged lef file// read def //location def
```
### After running the command if we use the magic we can view this file which depiects the picture a floorplan with components placed globally
![placement](https://user-images.githubusercontent.com/58397908/114306373-71b2a780-9af9-11eb-8be8-bc8b2abe64dc.jpg) 

### Magic Floorplan View
![magic floorplan](https://user-images.githubusercontent.com/58397908/114306031-25b33300-9af8-11eb-98b0-c714d30aa249.jpg)

### Ioplacer showing the data about the designs with respect to the config.tcl file , default file (Importantly the sky130a_fd_sc_hd file)
![ioplacer pic1](https://user-images.githubusercontent.com/58397908/114306102-601cd000-9af8-11eb-930f-48d08c4233d3.jpg)

### Pins are non-symmetrical as shown in the figure which can be preset depending upon the choice
![pins are non symmetrical](https://user-images.githubusercontent.com/58397908/114306407-93139380-9af9-11eb-8c6e-850250a226ef.jpg)


the picture shows the placement done before the placing of ip
![floorplan die to core ](https://user-images.githubusercontent.com/58397908/114306029-251a9c80-9af8-11eb-89a6-90e1058eb9ab.jpg)

## Need for library characterization
It is important because it general holds all the data of a particular netlist which can be a cell or an IP.
Depending on it we can determine the following conditions :-
  - Logic Synthesis
  - Floorplanning
  - Placement
  - Clock Tree Synthesis
  - Routing
  - STA


time(out_*_thr) - time(in_*_thr)


![transient response](https://user-images.githubusercontent.com/58397908/114306286-1a143c00-9af9-11eb-96ab-c257b3f75baf.jpg)

the transient response with respect to voltage and time (spice simulations)

![calculating time delay](https://user-images.githubusercontent.com/58397908/114306316-3912ce00-9af9-11eb-81ce-c68be03a8333.jpg)

calculating time delay 


![cell rise delay](https://user-images.githubusercontent.com/58397908/114306319-3c0dbe80-9af9-11eb-8918-5062ff7682ad.jpg)

calculatinf rise delay



![ngspice simulation](https://user-images.githubusercontent.com/58397908/114306642-074e3700-9afa-11eb-9336-de153c90da29.jpg)




# Day 3
## Cell design flow for an ASIC
We generallly design a standard cell which can be placed in a library we take the inputs (from foundries) and accordingly we get PDKs(process design kits) which contains DRC & LVS rules, SPICE models files which gives us data on threshold voltage, capacitance etc and library, user-defined specs which specifies the cell height that is the gap between the power lines and ground lines , cell width and so on.
(L = minimum feature size => lambda = L/2)

## Design steps
Circuit design which is followed by Layout design (art of layout design is eulers path and stick diagram which results in pmos network graph and nmos network), characterization and lastly outputs.
We can also make custom layouts but it requires alot of specifications which may cause issues to the maker as it is not specified by them.
CDL (circuit description language), GDSII(layout file),LEF(defines layout), extracted space netlist(.clr)(gives characterization flow)
## Eight key steps are followed in characterization process :- 
  - Reading-in the models
  - Reading the extracted spice netlist
  - Recognizing the behavoiur of the circuits 
  - Reading the sub circuits data and netlist
  - Attaching the power sources
  - Stimulus response checking
  - And lastly necessary simulation command

## SPICE Simulations
The below code is written for an inverter
We analyse the data with respect to time and voltage
These characteristics can be called as timing constrainsts a few below are examples of how they are cateogrised :-
  - Slew low rise threshold
  - Slew high rise threshold
  - Slew low fall threshold
  - Slew high fall threshold
  - In rise thr
  - In fall thr
  - Out rise thr
  - Out fall thr
```
** MODEL Descriptions **
** NETLIST Descriptions **
name_transistor drain gate source type w l
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u
cload out 0 10f
"dynamic simulation"
vin in 0 0 pulse 0 2.5 0(shift) 10p(rise) 10p(fall) 1n(pulse width) 2n(complete cycle length)
vdd vdd 0 2.5
vin in 0 2.5
** SIMULATION Commands**
.op
.dc Vin 0 2.5 0.05
.LIB "tsmc_025um_model.mod" CMOS_MODELS
.end
```

SPICE decks for CMOS inverter
component connectivity
source of pmos is connected to vdd
source of nmos is connected to capacitor


component values
pmos and nmos w/l 0.375micron/0.25micron
pmos should be wider than nmos
cload is approx 10fF
i/p gate and supply volt is approx 2.5V depending on channel length

nodes
identifying and naming nodes

case 1 specs (wn=wp=0.325,lp=ln=0.25,wn/ln=wp/lp =1.5)
case 2 specs (wn<wp=0.9375u,lp=ln,wn/ln=1.5<wp/lp=3.75)
cmos is robust as a result it is widely used in gate designs
vin 0 o/p 1
vin 1 o/p 0
1. Switching Threshold Vm
device current leakage at this stage is high
vin = vout (tan 45*)
case 1 is approx 0.98
case 2 is approx 1.2
we calculate rise delay fall delay
vgs = vds
idsp=-idsn

16 mask CMOS process
selecting a substrate
ptype,high resistivity(5~50omhs),doping lvl(10^15 cm^-3),orientation(100)
doping should be less than well doping

creating active region for transistors
grow a SiO2 40nm
Si3N4 80nm
1um photoresist
then we use photolithography and block the regions for wells
other layers are etched off
placed in oxidation furance SiO2 is grown
Bird's break, LOCOS
ethc off the Si3N4  using P2OH3

n well(pmos) and p well(nmos)
photoresist
pwell B is used where it is diffused by ion implentation (200keV) which penitaters thro oxide layer
nwell P is used where is it difused (400keV)
P heavier than B
driving  furnance 1100C 
diffuses the n and p well 
twin tubes

formation of gate
which controls threshold voltage
factors effecting gate 
doping concern
oxide concentration
B is used on p well side on surface  (60KeV)
Ar(as low as possible) we control the threshold voltage
HF is used to etch off the SiO2 and it is regrown
polysilicon layer is grown and then dope it with more impurities
excess polysilicon is removed (low resistance gate)

lightly doped drain (LDD) formation
2 reasons why we need n- and p-
hot electron effect 
high energy carrier break Si-Si bonds
3.2eV barrier b/w Si conduction band
SiO2 conduction band
short channel effect
drain penetrates channel which makes the gate to control the drain very hard
P is used on p well n- implant
B is used on n well p- implant
side wall spacers are createrd(Si3N4) Plasma anistropic etching 

source and drain formation
thin layer of oxide for avoiding ion going into the substrate helps in randomize the direction of ions 
As(75kev) 
n+ n- p
p+ p- n
high temperature analing

steps for generating contacts
Titanium for conductivity
sputtering means hitting the a metal(Ti) with the Agron(Ar) forms a layer 
Titanium react with Si and forms TiSi(low resistance), TiN(low comm)
RCA cleaning Deionsed water, 5 parts, NH4OH,H2O2 1 part
TiN local interconnect

Higher level metal formation
thick layer of SiO2 doping it with P(acts a protection Na ions) and B (reduces the temperature)
TiN
blanket tungsten(W) excess is washed
Aluminium is used for higher level
thickness of the interrconnect is increased as we go up
Si2N4 acts as a protection layer for the entire chip

drain gate source substrate



requirements
i/o msut line on intersection hor
width odd multiple hor pitch
height odd multiple of ver pitch


![placement after cell](https://user-images.githubusercontent.com/58397908/114306365-6c555d00-9af9-11eb-87aa-8e808a25157e.jpg)
![preplaced cell](https://user-images.githubusercontent.com/58397908/114306367-6eb7b700-9af9-11eb-9296-e7d6dc5491d2.jpg)
![pmos](https://user-images.githubusercontent.com/58397908/114306384-80995a00-9af9-11eb-8c51-c73ac6c5091b.jpg)
![ip added](https://user-images.githubusercontent.com/58397908/114306532-bdfde780-9af9-11eb-8594-5102ca37f98c.jpg)

# Day 4
## clock tree synthesis 

## open STA
## openroad

![clock slew both](https://user-images.githubusercontent.com/58397908/114308800-a7a85980-9b02-11eb-8af0-6fa2213a9835.jpg)

we can see both slew rates that is hold and setup for -0.12 slack 

![positive slack](https://user-images.githubusercontent.com/58397908/114306603-e554b480-9af9-11eb-8eee-b8418019bc2e.jpg)

we have got a positive slack which is the result of all the conversion of buffers

![post slacl](https://user-images.githubusercontent.com/58397908/114306611-ed145900-9af9-11eb-8f70-09d158b87b1b.jpg)

conversion of slack after post synthesis can be seen

![synthesis after opensta](https://user-images.githubusercontent.com/58397908/114306617-f30a3a00-9af9-11eb-829b-ab5f41ec08e7.jpg)

this is slack after the conversion of all buffers in opensta for post synthesis

![synthesis report](https://user-images.githubusercontent.com/58397908/114306619-f4d3fd80-9af9-11eb-9e6a-0afc4b63b867.jpg)

general synthesis report

![decreased slack](https://user-images.githubusercontent.com/58397908/114306653-103f0880-9afa-11eb-8ce2-c65581c0162f.jpg)

slack after few conversions

![012 thing](https://user-images.githubusercontent.com/58397908/114308846-d58d9e00-9b02-11eb-998b-1f690c980e0d.jpg)

the pre preped file

