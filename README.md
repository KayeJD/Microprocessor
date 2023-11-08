# Microprocessor (2021)
This project follows my process of building the digital logic for a brainless microprocessor through the use of [Digital](https://github.com/hneemann/Digital/releases/latest/download/Digital.zip), a schematic entry tool. We manipulate binary code to navigate data, memory, and instruction sets.
</br>
Design testing involves generating test waveform files to show toggling between inputs and their outputs. The designs are ran through Verilog, and waveforms are viewed with GTKWave. The final delivery of the project is a brainless microprocessor that is programmable through the manipulation of the ROM values</br>



## Part 1
We start by building a combinational logic circuit using primitive components. 
### 1.1 4-Bit Increment
We will use this component for the Program Counter (PC). The 4-bit incrementor is composed of 4 1-bit adders to accept a 4-bit binary number. This circuit produces a 4-bit binary number and a carry (CRY) output. If CRY is _1_, the output number is one more than the 4-bit input. </br> </br>
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitinc.gif) </br>
Half Adder Logic: </br>
| a | b | sum | cry |
| --- | --- | --- | --- |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
### 1.2 4-Bit Full Adder
A 3-input XOR gate 
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitadder.gif)
#### 1.3 Full Adder
![](https://github.com/KayeJD/Microprocessor/blob/main/fulladder.gif)



## Part 2
We need to be able to control the data flow through the circuit using the 4-bit 2-to-1 multiplexer. We also need to create the arithmetic logic unit (ALU) to perform add, sub, negate, etc. and the logical operations. 
### 2.1 4-bit 2-to-1 Mux
The microprocessor I'm designing operates on 4-bit numbers, therefore, to accomplish multiplexer behavior on 4-bit numbers, I simply placed four instances of 2-bit muxes (logic table below) and wired up the _sel_ input for the 2-bit muxes from one singular _sel_ signal. Inputs _a_ and _b_ are 4-but buses and are split by bit and wired into each of the 2-bit mux's _a_ inputs. The same concept is applied for _b_. When **sel = 0**, the value on **a** is displayed for the output. When **sel = 1**, the value of **b** is passed through to the output. 
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitmux.gif) </br>
2-bit-mux logic:
| s | a | b | y |
| --- | --- | --- | --- |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 |
### 2.2 Arithmetic Logic Unit 
![](https://github.com/KayeJD/Microprocessor/blob/main/alu.gif)
#### NOT_NEG
This circuit component can perform **2's complement** operations, **1's complement** operations, or allow input to **pass through**.
| Invert | Neg | Function |
| --- | --- | --- |
| 0 | 0 | Pass Through |
| 0 | 1 | Pass Through |
| 1 | 0 | 1's complement |
| 1 | 1 | 2's complement |
#### AND/ADD 
| Pass | Add | Function |
| --- | --- | --- |
| 0 | 0 | AND |
| 0 | 1 | ADD |
| 1 | 0 | Pass Through |
| 1 | 1 | Pass Through |



## Part 3
### 3.1 Accumulator and PC
A 4-bit storage unit is needed since this microprocessor operates on 4 bits. A **4-bit parallel D register** is needed and this will be a component in the CPUs accumulator. 
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitreg.gif)
![](https://github.com/KayeJD/Microprocessor/blob/main/pc.gif)
### 3.2 Instantiating RAM
We still need a memory component in order to store instructions and data. The memory for this project has 16 words, with 4 bits each word (_4-bit RAM with 16 4-bit words_). </br>
The **ram_vals.hex** file is used to initialize the contents of the RAM. Each line of is read in binary and then loaded into each address in the RAM. A short commented instruction set is inserted as an example
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/b0e1235b-6ae5-42f6-b5cd-49c3ee51e815)
### 3.3 Testing the brainless CPU
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/6994a54c-a4ae-4330-b41c-dfc16f8d8766)



## Part 4
The final part of building the microprocessor involves defining the instruction set for the controller, creating a simple program to insert into the microprocessor's memory, and executing the program.
### 4.1 Memory-Address-Generation Circuit
The memory address generation unit will be able to access memory locations in and out of sequential order. It will either automatically increment memory addresses after each instruction, or change memory locations in order to store or read data. This unit is comprised of a **register and incrementor (PC),** which will act like the pc, and a **memory address register (MAR).** </br>
The MAR takes a memory address from the RAM into the _data_bus_ and _load_mar_ is the enable signal to load the memory and push it into the output. When the _use_pc_ is high, the pc values drive the following memory address, when it is low, the MAR content is supplies into the address bus. 
![](https://github.com/KayeJD/Microprocessor/blob/main/pgrmctr.gif)
### 4.2 Controller Input
The controller will automatically set all of the signals to execute each operation. The **Program Ram** stores the opcodes that stores operations in hexidecimal form. The ROM is going to be used to decode the instructions and generate all of the signals neededd to operate the CPU according to the instructions. The only other signals that will not be automated will be the **clk, reset, and data_in.** </br>
Some of the instructions that will be used to test the microcontroller will not be completed in one clock cycle, so this microcontroller will be a **ROM-based finite state machine** that will be able to break down the opcodes into _micro-ops_. </br> </br>
The ROM will have preloaded values. The **Instruction Register (IR) and the Step Register (SR)** combine to form the address inputted to the ROM (4 bits from the IR, and 2 bits from the SR). The **Instruction ROM** will act as the decoder for the control signals and the programmable logic for the finite-state machine. The inpt for the IR will be the opcode stored inside the Program ROM, and to load it into the IR, the controller has to set the **load_ir** to _1 (The instruction fetch)_. The fetch state will begin each of the instructions.</br>
The ROM values willl be loaded from the **rom_vals.hex** file. The _0x1205_ instruction is the hexadecimal value needed to correctly fetch each instruction and the _0x3FFF_ instruction to force Digital to write out the entires of the ROM when exporting to Digital. </br>
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/de7d8def-683c-4177-b5f0-7e3620fe8657) </br>
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/28f90c55-d557-43fb-b988-e42753102da9) </br>
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/1c235a2f-1b55-4ba2-b8f9-1b82dff9ffba) </br>
Each component in the microcontroller is a copressed version of all the other components built in parts 1-3. 



## Part 5 - Complete Microprocessor Circuit
### Microprocessor
![](https://github.com/KayeJD/Microprocessor/blob/main/microprocessor.gif)
### GTKWave Simulation
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/df8670d6-a2df-457b-827c-d9ae23ee01cd)
