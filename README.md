# Microprocessor
This project follows my process of building the digital logic for a brainless microprocessor through the use of [Digital](https://github.com/hneemann/Digital/releases/latest/download/Digital.zip), a schematic entry tool. We manipulate binary code to navigate data, memory, and instruction sets.
</br>
Design testing involves generating test waveform files to show toggling between inputs and their outputs. The designs are ran through Verilog, and waveforms are viewed with GTKWave. The final delivery of the project is a brainless microprocessor that is programmable through the manipulation of  </br>


## Part 1
We start by building a combinational logic circuit using primitive components. 
### 4-Bit Increment
We will use this component for the Program Counter (PC). The 4-bit incrementor is composed of 4 1-bit adders to accept a 4-bit binary number. This circuit produces a 4-bit binary number and a carry (CRY) output. If CRY is _1_, the output number is one more than the 4-bit input. </br> </br>
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitinc.gif) </br>
Half Adder Logic: </br>
| a | b | sum | cry |
| --- | --- | --- | --- |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
### 4-Bit Full Adder
A 3-input XOR gate 
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitadder.gif)
#### Full Adder
![](https://github.com/KayeJD/Microprocessor/blob/main/fulladder.gif)


## Part 2
We need to be able to control the data flow through the circuit using the 4-bit 2-to-1 multiplexer. We also need to create the arithmetic logic unit (ALU) to perform add, sub, negate, etc. and the logical operations. 
### 2-bit Mux
### 4-bit 2-to-1 Mux
![](https://github.com/KayeJD/Microprocessor/blob/main/4bitmux.gif)
### Arithmetic Logic Unit 
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


## Part 4
The final part of building the microprocessor involves defining the instruction set for the controller, creating a simple program to insert into the microprocessor's memory, and executing the program.
#### Memory-Address-Generation Circuit
First, we modify the 4-bit incrementer to generate the memory addresses for the CPU (which acts like the PC). 
#### Controller Input

## Part 5 - Complete Microprocessor Circuit
### GTKWave Simulation
![image](https://github.com/KayeJD/Microprocessor/assets/139111295/df8670d6-a2df-457b-827c-d9ae23ee01cd)
