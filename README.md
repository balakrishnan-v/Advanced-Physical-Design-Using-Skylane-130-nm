# Advanced-Physical-Design-Using-Skylane-130-nm
# Contents
- Day 1 
   - Introduction
   - RISC-V
   - ABIs
   - OpenLANE
   - Getting started with the platform
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

![tem files](https://user-images.githubusercontent.com/58397908/114047080-07a2c400-98a7-11eb-9590-155454da816f.jpg)

![creation of runs file](https://user-images.githubusercontent.com/58397908/114047097-0c677800-98a7-11eb-879d-30ee18b7c764.jpg)

![synthesis report](https://user-images.githubusercontent.com/58397908/114047129-11c4c280-98a7-11eb-8443-0678f9bc13f8.jpg)
