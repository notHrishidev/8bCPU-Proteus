# 8-bit Turing complete CPU in Proteus
In this project, an 8-bit CPU is built which can perform 11 different operations on a maximum of two operands. The entire CPU was modelled at a gate level. A separate .txt file can be used to write the program, and a MS Excel workbook converts the code written in the custom assembly language to machine code, which can be loaded to the program memory. The memory unit used Dual Port reading which enabled an instruction to be completed within one clock cycle.
# Specifications
* All data is transferred and manipulated as 8 bit words
* 16 bit instruction word with 4 bit opcode, and 3 operands each having a 4 bit address.
* Can also load 8 bit data directly to the registers
* Faster operand fetching and least complex design using Dual-port memory.
* 16 general purpose 8 bit registers available to the user.
* Jump instructions: Jump to specified address and Jump to stored address (the current address can be stored by an instruction and then the control can jump to that instruction plus a specified offset)
* 4 bit program counter, can store a maximum of 16 instructions. Can be expanded by using a longer width counter and cascading multiple instruction memory units together.
* Instruction memory can be loaded through a pattern generator, which loads the instructions serially.

# Limitations
* Too small word length for larger operations (limited to 8 bit word length to prevent complexity)
* Random race conditions and logical congestions due to bugs in the Proteus software.
* Easy to identify bugs thanks to real time simulation in Proteus but difficult to solve them if it involves changing connections and components.

# How to use
Here to generate the machine code, I'm using MS Excel. Leveraging the feature of easily creating and manipulating Look Up Tables, MS Excel is the most straightforward way to convert the custom assembly code to machine code.
The code.txt file should have the instructions with each line having all four components, or filling in the arguments with "X". This file is linked to the Excel Workbook.
Whenever the code.txt is edited, it must be saved and the data in the Excel Workbook must be refreshed. Only then the latest set of instructions will be reflected in the converted machine code.
# Steps to generate machine code:
1. Write the program in the code.txt file in the custom assembly language, about which you can refer to the excel Workbook.
2. Save this file and then in the Excel Workbook, go to the "Compiler" Sheet.
3. Go to the Data tab and select "Refresh all". Now the new instructions would be loaded.
4. Now the cell at the bottom right part will be the instructions converted to machine code, which is to be loaded to the Instruction memory.

# Steps to load Instruction memory
1. Copy the text in the bottom left cell and paste it using the "paste as text" option in the paste options to the larger merged cell.
2. Now select the merged cell and copy all the contents from the formula field. Copying the cell as such might cause special characters to creep in in the copied text.
3. Now open the Proteus file, HrishComp 8016 v"X".pdsprj, and find the pattern generator module that says "Load instructions".
4. Double click on it, make sure Manual edits are turned on.
5. On the right side there would be the properties of the generator. In that, there is an attribute called "PATTERN". Edit that line so that it becomes PATTERN={ *_the copied machine code string_* }. Now press OK.
6. Start simulating by pressing the play button.

# How it works
* The pattern generator continuously outputs the bits in the pattern provided. During the first 10 seconds, a 32hz clock loads the instruction memory with this pattern in a serial mode. 
* This clock shouldn’t be altered in any way. No other parameters in the pattern generator or the clock should be changed except for the Pattern itself. 
* It takes 8 seconds to load the 256 bits of instructions to the instruction memory (1/32 x 16 registers x 16 bit instruction word). An extra two seconds is given as buffer time in order to account for the computing power limitations of 
  the host machine and the subsequent clock skipping and bugs. 
* The clock that drives the program counter is separate from the clock used to load the Instruction memory. The frequency of this clock can be varied freely as it only changes the speed of execution. This clock frequency also must be 
  selected based on the hardware that the program in running on as the circuit simulation gets heavy on the CPU very fast. 
* Once the 10 seconds is over the clock starts and instructions are executed. Incase there is no loops in the program, the program is executed from the first line again as there is no HALT feature.


