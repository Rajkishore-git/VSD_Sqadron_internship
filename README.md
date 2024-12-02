# VSD_Sqadron_internship
The program is built on the RISC-V architecture and leverages open-source tools to teach VLSI chip design and RISC-V concepts. The internship is guided by Kunal Ghosh.

<details> 
<summary><b>Task 1:</b> Implementing C and RISC-V programs for the sum of n numbers</summary> 
<br>
  
C Implementation
  
Step 1: Install the Leafpad editor.

Run the following command to install Leafpad:
```
sudo apt install leafpad

```
Step 2: Write a C program to calculate the sum of numbers from 1 to n and save it as sum1ton.c.


![sum1ton](https://github.com/user-attachments/assets/caa1a9c8-47b8-4a39-a63b-9856688f4030)

After compiling and running the program using the commands:
```
gcc sum1ton.c
./a.out
```
The output of the C code will be the sum of numbers from 1 to n, based on the value of n provided in the program or entered during execution. For example:

![output sum1ton](https://github.com/user-attachments/assets/db5285ce-fd32-482c-af8e-c38acb9d30af)

RISC-V implementation 
------------------------------------------

You can view the sum program written in RISC-V assembly using the following command:
```
cat sum1ton.c
```
This command displays the content of the "sum1ton.s" file, which contains the RISC-V assembly code for calculating the sum of numbers from 1 to n.
The terminal output of the above the commad :

![out2ter](https://github.com/user-attachments/assets/092f17d4-e9bb-4aff-a4c4-007c34175521)


To compile the RISC-V assembly code, use the following command:
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
![o3](https://github.com/user-attachments/assets/5082ad98-3b87-4bfb-a67b-7d8829a09414)

This generates an executable file named sum1ton from the assembly code.

Now the file has been saved "sum1ton.o"
In the new tab we need to give the command ``` riscv64-unknown-elf-objdump -d sum1ton.o | less ```

The assembly language code for ```O1``` (optimized code with optimization level 1) can be viewed after running the command:

![o4](https://github.com/user-attachments/assets/3486c762-b7cd-4075-bb03-a86cc3493104)

This displays the disassembled machine code for the compiled sum1ton.o file.
The output includes the RISC-V assembly instructions generated with optimization level O1, showing the efficient instructions used for the sum computation.

Here if we calculate the number of instructions, we get the total instructions as 11.
It is calculated as 
``` 
101b0 - 10184 = 2c
2c/4 = b  => 11
```
similarly, for ``` Ofast ``` command

The input is:

![o5](https://github.com/user-attachments/assets/e5f4ddfd-36df-40c8-8271-18431cadf94d)

The output of the ``` Ofast ``` command is :

![06](https://github.com/user-attachments/assets/4f8b3ca2-fd88-4dda-a896-6485e142ca08)

If we count the number of instructions again, we find a total of 11 instructions. The calculation is as follows: 
``` 
100dc - 100b0 = 2c
2c/4 = b  => 11
```



</details>
<details>
<summary><b>Task 2:</b> RISC-v spike simulation</summary> 

## About Spike
Spike is the official RISC-V ISA (Instruction Set Architecture) simulator and a reference implementation for RISC-V processors. It is an open-source, cycle-accurate simulator that models the execution of RISC-V instructions on a virtual machine. 

Spike is primarily used for:

- **Testing and Validation**: It helps test and validate RISC-V hardware implementations without the need for actual hardware.
- **Debugging**: Provides a platform for debugging and troubleshooting RISC-V programs.
- **Software Development**: Developers can use Spike to run and debug RISC-V programs in a simulated environment.

Spike simulates various aspects of a RISC-V processor, including:

- Different privilege levels (user, supervisor, and machine modes)
- Memory access and control flow
- Supports various RISC-V extensions, offering flexibility for different system configurations.

By providing an accurate simulation of RISC-V behavior, Spike serves as a valuable tool for both hardware and software development in the RISC-V ecosystem.

## Verififing outputs of gcc and spike
use the command `spike pk sum1ton.o` give the output of the C code.

![gcc spike_out](https://github.com/user-attachments/assets/c8f15105-a875-45de-9cc7-dcd40820a1ab)
from the above image we can verify that the outputs are indeed same.

## Steps to debug Assembly Language Program

1. Open a new terminal tab.
2. Run the following command to disassemble the object file and view the assembly language representation:
   ```riscv64-unknown-elf-objdump -d sum1ton.o | less```
3. Find the starting memory adress of main. in this case it is ```100b0```.

 ![scr2 1](https://github.com/user-attachments/assets/21786efd-16a6-4c1c-ae4a-9d09b6e811d9)
4. Use spike ```spike -d pk sum1ton.o```, to start debug mode.
5. Enter ```until pc 0 100b0```. The command `until PC 0 100b0` in Spike is used to **pause program execution** until the **program counter (PC)** reaches the memory address **0x100b0**.

### Explanation:
- **until**: This is a debugging command to set a condition for execution.
- **PC**: Refers to the **program counter**, which holds the address of the next instruction to be executed.
- **0 100b0**: Specifies the range for the PC. The program will continue executing until the PC reaches or exceeds **0x100b0**.

This command is useful to stop the execution at a specific point in the program, allowing you to inspect or debug before that address is reached.

6. Enter ``` reg 0 a0```. The command `reg 0 a0` in Spike is used to **display the value** of the **a0** register (RISC-V register) at the current point in the program's execution. 

- **reg**: Command to inspect register values.
- **0**: Refers to the register number or index (for display purposes).
- **a2**: The name of the register you want to check.

This allows you to view the contents of the **a0** register during debugging.

![scr2 2](https://github.com/user-attachments/assets/cb6d37a8-8281-4f04-9e93-442ce88dfdf7)

## Application: 7-Segment Display Decoder

The application is designed to convert a decimal number (0-9) into its corresponding 7-segment display pattern. This can be used in embedded systems or digital circuits that drive a 7-segment display to visually show numerical values.

### Algorithm:
1. **Input**: A number between 0 and 9.
2. **Array Representation**: Store 7-segment patterns for each digit (0-9) in an array.
3. **Check Validity**: Ensure the input is between 0 and 9.
4. **Display Output**: If the input is valid, print the corresponding 7-segment pattern. Otherwise, print an error message.

### Code:

```c
#include <stdio.h>

int main() {
    int binary = 5; // Example input (change this to test other numbers)

    // Array representing 7-segment patterns for numbers 0-9
    const char* segments[] = {
        "1110111", // 0
        "0010010", // 1
        "1011101", // 2
        "1011011", // 3
        "0111010", // 4
        "1101011", // 5
        "1101111", // 6
        "1010010", // 7
        "1111111", // 8
        "1111011"  // 9
    };

    if (binary >= 0 && binary <= 9) {
        printf("Input: %d -> 7-segment: %s (a-g segments)\n", binary, segments[binary]);
    } else {
        printf("Invalid input. Please enter a number between 0 and 9.\n");
    }

    return 0;
}
```
On compiling the code, we have the output using gcc/spike as,
![scr2 4](https://github.com/user-attachments/assets/c5e4f80e-cca6-4f0d-85d5-1652c9baec37)

the assembly code is,
![scr2 5 (2)](https://github.com/user-attachments/assets/c4983500-1802-4567-b564-8cec1727eff9)

debugger:
![scr2 6](https://github.com/user-attachments/assets/d2963ed7-22bd-49cf-8ea2-ad41d7520918)

## Functionality
### Registers:
1. **`sp` (Stack Pointer)**: Points to the top of the stack. It is used to manage function calls, local variables, and saving/restoring register states.
2. **`ra` (Return Address)**: Holds the return address for function calls (i.e., the address to return to after a function is completed).
3. **`a0`-`a7` (Argument Registers)**: Used for passing arguments to functions. `a0` holds the first argument, `a1` holds the second, and so on.
4. **`a2`**: This register is used to hold arguments (in this case, it is loaded with the value `0x21` and then incremented).
5. **`li`**: The `li` instruction is used to load an immediate value into a register.
6. **`lui`**: The `lui` instruction loads an immediate value into the upper 20 bits of a register.
7. **`jal`**: The `jal` (Jump and Link) instruction is used to perform a function call. It jumps to the address provided and saves the return address in `ra`.

### Program Explanation:

1. **`addi sp, sp, -16` (Instruction at `10184`)**:  
   - Decreases the stack pointer (`sp`) by 16, creating space for saving registers.
   
2. **`sd ra, 8(sp)` (Instruction at `10188`)**:  
   - Saves the return address (`ra`) at an offset of 8 from the current stack pointer (`sp`).

3. **`lui a2, 0x21` (Instruction at `1018c`)**:  
   - Loads the upper 20 bits of register `a2` with `0x21` (the value `0x21000`).

4. **`addi a2, a2, 384` (Instruction at `10190`)**:  
   - Adds 384 to register `a2`, making `a2` hold the value `0x21000 + 384 = 0x21180`.

5. **`li a1, 5` (Instruction at `10194`)**:  
   - Loads the immediate value `5` into register `a1`.

6. **`lui a0, 0x21` (Instruction at `10198`)**:  
   - Loads the upper 20 bits of register `a0` with `0x21` (the value `0x21000`).

7. **`addi a0, a0, 392` (Instruction at `1019c`)**:  
   - Adds 392 to register `a0`, making `a0` hold the value `0x21000 + 392 = 0x21188`.

8. **`jal ra, 1040c <printf>` (Instruction at `101a0`)**:  
   - Jumps to the `printf` function located at address `0x1040c` (a call to the `printf` function), saving the return address in the `ra` register.

9. **`li a0, 0` (Instruction at `101a4`)**:  
   - Loads the value `0` into register `a0`.

10. **`ld ra, 8(sp)` (Instruction at `101a8`)**:  
    - Loads the return address (`ra`) from the stack (offset 8 from `sp`) back into the `ra` register.

11. **`addi sp, sp, 16` (Instruction at `101ac`)**:  
    - Increases the stack pointer (`sp`) by 16, cleaning up the space previously allocated.

12. **`ret` (Instruction at `101b0`)**:  
    - Returns from the function by jumping to the address stored in `ra`.

</details>
<details>
<summary><b>Task 3:</b> Types of instructions in RISC-V</summary> 

### Introduction  

- **RISC-V Base ISA Instruction Formats**  
  - The base RV32I ISA includes four core instruction formats: **R**, **I**, **S**, and **U**.  
  - All instructions are fixed at **32 bits** in length.  
  - Instructions must be **aligned on a four-byte boundary** in memory (`IALIGN=32`).  

- **Instruction Alignment**  
  - Misaligned instructions trigger an **instruction-address-misaligned exception** during taken branches or jumps.  
  - Exceptions are reported on the misaligned branch/jump instruction, not the target instruction.  
  - When extensions with 16-bit instruction lengths are added, alignment relaxes to a **two-byte boundary** (`IALIGN=16`).  

- **Handling Reserved Instructions**  
  - Behavior for decoding reserved instructions is **unspecified**.  
  - Platforms may:  
    - Raise an **illegal-instruction exception** for reserved standard opcodes.  
    - Permit non-conforming extensions using reserved opcode spaces.  

- **Register and Immediate Design**  
  - Source (`rs1` and `rs2`) and destination (`rd`) registers are **uniformly positioned** across all formats to simplify decoding.  
  - Immediates:  
    - Are **sign-extended** for simplicity and efficiency.  
    - Positioned to minimize hardware complexity, with the sign bit always at **bit 31**.  
    - Include a 12-bit immediate field for regular instructions and a 20-bit field for **load-upper-immediate** (LUI) instructions.  

- **Design Principles**  
  - Register specifiers are consistent across formats to reduce critical path delays.  
  - Immediate bits are optimized for hardware simplicity, even if they require cross-format rearrangement.  
  - The design prioritizes simplicity and hardware efficiency over including features like zero-extension for certain immediates.  

### Types of instructions in RISC-V
![image](https://github.com/user-attachments/assets/86c9a01e-32e3-4cc5-ad86-df57d8840e7d)

The image illustrates the following RISC-V instruction formats:  
- **R-Type**: Used for register-to-register operations.  
- **I-Type**: Used for immediate-based instructions, including loads.  
- **S-Type**: Used for store instructions.  
- **B-Type**: Used for conditional branch instructions.  
- **U-Type**: Used for upper immediate instructions like LUI.  
- **J-Type**: Used for jump instructions like JAL.  
## R-Type  

![image](https://github.com/user-attachments/assets/a617b8f1-7bd0-4ddf-bada-4d3b9a07b08b)

The **R-Type** format is used for register-to-register operations, such as arithmetic, logical, and shift instructions. Its structure is detailed below:  

- **[31:25] (funct7)**:  
  - A 7-bit field providing additional instruction-specific information.  
  - Differentiates variations of operations within the same category (e.g., `ADD` vs. `SUB`, which share the same `opcode` and `funct3` but differ in `funct7`).  
  - Common examples:  
    - `0000000` for `ADD` and `SLL`.  
    - `0100000` for `SUB` and other reverse operations.  

- **[24:20] (rs2)**:  
  - Specifies the second source register (5 bits).  
  - Used in operations requiring two input registers, such as `ADD`, `SUB`, or logical AND/OR.  

- **[19:15] (rs1)**:  
  - Specifies the first source register (5 bits).  
  - Works with `rs2` to provide inputs for the operation.  

- **[14:12] (funct3)**:  
  - A 3-bit field specifying the operation category.  
  - Works alongside `opcode` and `funct7` to identify the exact instruction.  
  - Examples:  
    - `000` for addition (`ADD`) or subtraction (`SUB`).  
    - `111` for bitwise AND.  
    - `100` for bitwise XOR.  
  - Encodes operation variants, especially when multiple operations share the same `opcode`.  

- **[11:7] (rd)**:  
  - Specifies the destination register (5 bits).  
  - The result of the operation is stored in this register.  

- **[6:0] (opcode)**:  
  - A 7-bit field identifying the broad instruction type.  
  - Indicates that the instruction uses the R-Type format.  
  - Examples:  
    - `0110011` for most register-based arithmetic and logical operations.  
  - Combined with `funct3` and `funct7` to uniquely identify the instruction.

### I-Type

The **I-Type** format is used for instructions that involve immediate values, such as loads, arithmetic operations with immediates, and system calls. Its structure is detailed below:  
![image](https://github.com/user-attachments/assets/7552d082-3ad9-4a79-8442-4aa32956bfe7)

- **[31:20] (imm[11:0])**:  
  - A 12-bit field that contains the immediate value.  
  - The value is sign-extended to fit the operation's requirements.  
  - Commonly used as:  
    - A direct operand in arithmetic instructions (e.g., `ADDI`, `SLTI`).  
    - An offset in memory access instructions (e.g., `LW`, `LH`).  

- **[19:15] (rs1)**:  
  - Specifies the source register (5 bits).  
  - Provides the base register in memory load instructions or the operand in immediate arithmetic instructions.  

- **[14:12] (funct3)**:  
  - A 3-bit field specifying the operation category.  
  - Works alongside `opcode` to identify the exact instruction.  
  - Examples:  
    - `000` for `ADDI` (add immediate).  
    - `010` for `SLTI` (set less than immediate).  
    - `011` for `SLTIU` (set less than immediate unsigned).  
    - `100` for bitwise XOR immediate (`XORI`).  
  - Encodes operation variants within the same instruction class.  

- **[11:7] (rd)**:  
  - Specifies the destination register (5 bits).  
  - The result of the operation or the loaded value is stored in this register.  

- **[6:0] (opcode)**:  
  - A 7-bit field identifying the instruction type.  
  - Indicates that the instruction uses the I-Type format.  
  - Examples:  
    - `0010011` for arithmetic operations with immediates.  
    - `0000011` for memory load instructions.  
    - `1110011` for system calls (e.g., `ECALL`).  
  - Combined with `funct3` to specify the instruction’s behavior.  

### Summary of Fields in I-Type Format:  
- **`opcode`**: Defines the instruction type and category (e.g., load, immediate arithmetic, or system calls).  
- **`funct3`**: Specifies the sub-operation (e.g., `ADDI`, `SLTI`).  
- **Immediate (`imm[11:0]`)**: Encodes an offset or operand directly in the instruction.

### S-Type Format  

The **S-Type** format is used for **store instructions**, where data from a source register is written to a memory location. Its structure is detailed below:  
![image](https://github.com/user-attachments/assets/92e1e7ef-0047-4ee9-81d0-c3f563769207)

- **[31:25] (imm[11:5])**:  
  - The upper 7 bits of the immediate value.  
  - Combined with `imm[4:0]` (from bits [11:7]) to form the full 12-bit immediate.  
  - Used as an offset in memory addressing.  

- **[24:20] (rs2)**:  
  - Specifies the second source register (5 bits).  
  - Contains the value to be stored in memory at the computed address.  

- **[19:15] (rs1)**:  
  - Specifies the first source register (5 bits).  
  - Provides the base address for the memory operation.  

- **[14:12] (funct3)**:  
  - A 3-bit field defining the operation category.  
  - Specifies the type of store instruction.  
  - Examples:  
    - `000` for `SB` (store byte).  
    - `001` for `SH` (store halfword).  
    - `010` for `SW` (store word).  

- **[11:7] (imm[4:0])**:  
  - The lower 5 bits of the immediate value.  
  - Combined with `imm[11:5]` to form the full 12-bit immediate.  

- **[6:0] (opcode)**:  
  - A 7-bit field identifying the instruction type.  
  - Indicates that the instruction is of the S-Type format.  
  - Example:  
    - `0100011` for store instructions (`SB`, `SH`, `SW`).  
  - Works with `funct3` to determine the specific instruction.  
### B-Type Format  

The **B-Type** format is used for **branch instructions**, which control the program's flow based on conditional evaluations. Its structure is detailed below:  
![image](https://github.com/user-attachments/assets/c245626c-38dc-41ea-93d7-c165ba0080db)

- **[31] (imm[12])**:  
  - The most significant bit of the immediate value.  
  - Used for sign-extension to compute the target branch address.  

- **[30:25] (imm[10:5])**:  
  - Part of the 12-bit immediate value.  
  - Combined with other immediate bits to determine the branch offset.  

- **[24:20] (rs2)**:  
  - Specifies the second source register (5 bits).  
  - Provides the second operand for the branch condition.  

- **[19:15] (rs1)**:  
  - Specifies the first source register (5 bits).  
  - Provides the first operand for the branch condition.  

- **[14:12] (funct3)**:  
  - A 3-bit field specifying the branch condition.  
  - Examples:  
    - `000` for `BEQ` (branch if equal).  
    - `001` for `BNE` (branch if not equal).  
    - `100` for `BLT` (branch if less than).  
    - `101` for `BGE` (branch if greater than or equal).  

- **[11] (imm[11])**:  
  - A bit from the immediate value, used in computing the branch target address.  

- **[10:1] (imm[4:1])**:  
  - Part of the immediate value, forming the middle portion of the branch offset.  

- **[6:0] (opcode)**:  
  - A 7-bit field identifying the instruction type.  
  - Indicates that the instruction is of the B-Type format.  
  - Example:  
    - `1100011` for all branch instructions (`BEQ`, `BNE`, `BLT`, etc.).  

#### Immediate Field Combination  
The immediate field in B-Type instructions is assembled as follows:  
- Concatenate `imm[12]`, `imm[10:5]`, `imm[4:1]`, and `imm[11]`.  
- The full immediate is then left-shifted by 1 to compute the branch offset (since branch targets must align with 2-byte boundaries).  

















