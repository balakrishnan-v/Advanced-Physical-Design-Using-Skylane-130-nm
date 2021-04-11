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
## RISC-V
RISC-V(stands for Reduced Instruction Set Computer Five) is an instruction set architecture which is open sourced for mainly learning and understanding of a computer architecture was devolped at University of California, Berkley. This ISA generally comes under little endian memory addressing mode or system which basically means Most Significant Bit is stored last and the Least Significant Bit is stored first. The memory in generally addresses a byte of data and there are only 32 regesters in RISC-V architecture meaning we can store the data in these regesters while running other commands such that it is efficent. The core(picorv32a) is a pre designed SoC which is generally used as a reference.
## ABI
ABI stands for Application Binary Interface which is generally also called as System Call Interface is generally one of the interfaces which helps us run our commands on the hardware.
## OpenLANE
OpenLANE is a tool used for 

## Getting Started with the platform

![opening my  interactive window](https://user-images.githubusercontent.com/58397908/114047038-ff4a8900-98a6-11eb-8797-41705a527344.jpg)

my first interactive window

![tem files](https://user-images.githubusercontent.com/58397908/114047080-07a2c400-98a7-11eb-9590-155454da816f.jpg)

directory where we have stored the merged file

![creation of runs file](https://user-images.githubusercontent.com/58397908/114047097-0c677800-98a7-11eb-879d-30ee18b7c764.jpg)

creating my own runs files

![synthesis report](https://user-images.githubusercontent.com/58397908/114047129-11c4c280-98a7-11eb-8443-0678f9bc13f8.jpg)

synthesis report

# Day 2
Notes
Core is present on the inside of the die(encapsulate the core)
1) how can we define the width and height of core and die
dimensions of the chip mostly depends on the dimensions of the standard cells
Utilization factor = (area occuipied by netlist)/(total area of the core) = 1 (100%utilization)
ideally we go for (50 - 60) %
aspect ration = height/width = 1 (square)
if it other than 1 it signifies rectangle
intial netlist uses less space so that we can place other cells for optimisation

Pre placed cells
they can be macros or ips
they are a part of the top level netlist
the location or arrangements of the cells are done only once because their functionality does not change
they are placed before routing and placement of other cells
locations of pre placed cells are never touched and placed depending on the design scenarios

decoupling capacitors are generally used incase the power supply for the gate falls under the undefined region in noise margin level which may cause it fall under both logic 1 or 0
it is a capacitor filled with a lot charge which will result in less loss

power planning
if supply is being provided from one point we face two issues namely
ground bounce (discharging)
voltage droop (charging)
solution is to use multiple vdd vss lines creating power meshes
handshaking
logical cell placement blockage is used

./flow.tcl -interactive

-tag name(custom name folder)
-tag name(custom name folder) -overwrite
echo env$(clock for example) -> view
set env(clock for example) new number  -> changes

changes have to be made in the floorplanning configurations

giving shapes and sizes for the gates
bind netlist with physical cells
library will have width delay and condtion of the cell
bigger cells mean it is faster
various flavours
then we place them in the floorplan
depending on the i/o ports cells are placed to avoid any delays
problems are generally solved by the technique called as optimize placement 
basically we estimate the length and capacitance on the basis of this information we try to insert repeaters to maintain the signal integrity

need for library characterization
common  thing is GATES or Cells
logic synthesis
floorplanning
placement
clock tree synthesis
routing
sta

cell design flow
standard cell is placed in a library
cells with differnt sizes (vt less means time taken is less)
inputs (from foundries)
PDKs (DRC &LVS rules, SPICE models files (these give us data on threshold voltage, capacitance etc), library & user-defined specs(cell height is gap btw power line and gnd line , cell width))
L = minimum feature size
lambda = L/2
design steps
circuit design, layout design(art of layout design is eulers path and stick diagram which results in pmos network graph and nmos network), characterization 
custom layout **
outputs
CDL (circuit description language), GDSII(layout file),LEF(defines layout), extracted space netlist(.clr)(gives characterization flow)
8 steps are followed in characterization 
read in the models
read the extracted spice netlist
reconize the behavoiur 
read the sub circuits
attach the power sources
stimulus
necessary simulation command
steps -> GUNA -> modeling

timing characterization
slew low rise threshold
slew high rise threshold
slew low fall threshold
slew high fall threshold
in rise thr
in fall thr
out rise thr
out fall thr

time(out_*_thr) - time(in_*_thr)

![floorplan die to core ](https://user-images.githubusercontent.com/58397908/114306029-251a9c80-9af8-11eb-89a6-90e1058eb9ab.jpg)

the is general viewing format for a floorplan designs 

![placement](https://user-images.githubusercontent.com/58397908/114306373-71b2a780-9af9-11eb-8be8-bc8b2abe64dc.jpg)

the picture shows the placement done before the placing of ip

![magic floorplan](https://user-images.githubusercontent.com/58397908/114306031-25b33300-9af8-11eb-98b0-c714d30aa249.jpg)

magic floorplan view

![ioplacer pic1](https://user-images.githubusercontent.com/58397908/114306102-601cd000-9af8-11eb-930f-48d08c4233d3.jpg)

ioplacer showing the data about the designs

![transient response](https://user-images.githubusercontent.com/58397908/114306286-1a143c00-9af9-11eb-96ab-c257b3f75baf.jpg)

the transient response with respect to voltage and time (spice simulations)

![calculating time delay](https://user-images.githubusercontent.com/58397908/114306316-3912ce00-9af9-11eb-81ce-c68be03a8333.jpg)

calculating time delay 


![cell rise delay](https://user-images.githubusercontent.com/58397908/114306319-3c0dbe80-9af9-11eb-8918-5062ff7682ad.jpg)

calculatinf rise delay

![pins are non symmetrical](https://user-images.githubusercontent.com/58397908/114306407-93139380-9af9-11eb-8c6e-850250a226ef.jpg)

pins are non symmetrical

![ngspice simulation](https://user-images.githubusercontent.com/58397908/114306642-074e3700-9afa-11eb-9336-de153c90da29.jpg)




# Day 3
notes
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

