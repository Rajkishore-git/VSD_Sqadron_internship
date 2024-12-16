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

## Introduction  

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

## Types of instructions in RISC-V
![image](https://github.com/user-attachments/assets/86c9a01e-32e3-4cc5-ad86-df57d8840e7d)

The image illustrates the following RISC-V instruction formats:  
- **R-Type**: Used for register-to-register operations.  
- **I-Type**: Used for immediate-based instructions, including loads.  
- **S-Type**: Used for store instructions.  
- **B-Type**: Used for conditional branch instructions.  
- **U-Type**: Used for upper immediate instructions like LUI.  
- **J-Type**: Used for jump instructions like JAL.  
## R-Type  

The **R-Type** format is used for register-to-register operations, such as arithmetic, logical, and shift instructions. Its structure is detailed below:  
![image](https://github.com/user-attachments/assets/a617b8f1-7bd0-4ddf-bada-4d3b9a07b08b)

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

### U-Type

The **U-Type** format is used for instructions that require a large immediate value, typically for constructing addresses or performing arithmetic operations on upper bits. Its structure is detailed below:
![image](https://github.com/user-attachments/assets/8647a772-3078-44f3-9693-4e6d0780440b)

- **[31:12] (imm[31:12])**:  
  - A 20-bit immediate value.  
  - Stored in the upper 20 bits of the destination register.  
  - The lower 12 bits of the destination register are filled with zeros.  
  - Commonly used for:  
    - Loading upper 20 bits into a register (`LUI`).  
    - Adjusting addresses or constants (`AUIPC`).

- **[11:7] (rd)**:  
  - Specifies the destination register (5 bits).  
  - The result of the instruction is stored in this register.  

- **[6:0] (opcode)**:  
  - A 7-bit field identifying the instruction type.  
  - Examples:  
    - `0110111` for `LUI` (Load Upper Immediate).  
    - `0010111` for `AUIPC` (Add Upper Immediate to PC).  
  - Combined with the `imm[31:12]` field to determine the operation.

### J-Type


The **J-Type** format is used for **jump instructions**, specifically for transferring program control to a specified target address with a 20-bit immediate offset. It supports unconditional jumps while optionally storing the return address in a register. Its structure is detailed below:  
![image](https://github.com/user-attachments/assets/02d89bd7-430b-40e3-b9f5-af9a0618dc17)

- **[31] (imm[20])**:  
  - The most significant bit of the 20-bit immediate value.  
  - Used for sign-extension when calculating the jump target address.  

- **[30:21] (imm[10:1])**:  
  - Part of the immediate value, forming the middle portion of the jump offset.  

- **[20] (imm[11])**:  
  - A bit of the immediate value, included in the target offset calculation.  

- **[19:12] (imm[19:12])**:  
  - The upper 8 bits of the immediate value, contributing to the jump offset.  

- **[11:7] (rd)**:  
  - Specifies the destination register (5 bits).  
  - Stores the return address (the address of the next instruction) when the jump is executed.  
  - If `rd` is set to `x0`, no return address is stored.  

- **[6:0] (opcode)**:  
  - A 7-bit field identifying the instruction type.  
  - Example:  
    - `1101111` for `JAL` (Jump and Link).  

#### Immediate Field Combination  
The 20-bit immediate offset is constructed as follows:  
1. Concatenate `imm[20]`, `imm[10:1]`, `imm[11]`, and `imm[19:12]`.  
2. Left-shift the immediate by 1 bit (to align with 2-byte instruction boundaries).  
3. Add the offset to the program counter (PC) to compute the jump target address.

## Identifing instructions from the application code (7seg-decoder.c)
![scr2 5 (2)](https://github.com/user-attachments/assets/31c3ee0c-bb75-4e2e-ba9d-92fee4b03754)
 
---

### 1. **`addi sp, sp, -16`**
*Add Immediate:* Adds an immediate value to a source register and stores the result in the destination register. 
- **Format:** I-type
- **Opcode:** `0010011` (7 bits)  
- **Immediate:** `-16` (`1111111111110000` in two's complement, 12 bits)  
- **Destination Register (rd):** `sp` (x2, 5 bits)  
- **Source Register (rs1):** `sp` (x2, 5 bits)  
- **Function (funct3):** `000` (3 bits)  

#### Breakdown:  
- Immediate: `1111111111110000` split into imm[11:0] = `1111111111110000`  
- rd (sp = x2): `00010`  
- rs1 (sp = x2): `00010`  
- funct3: `000`  
- Opcode: `0010011`  

#### Binary Representation:  
`1111111111110000 00010 000 00010 0010011`  

---

### 2. **`sd ra, 8(sp)`**
*Store Doubleword:* Stores a 64-bit value from a source register into memory. 
**Format:** S-type
- **Opcode:** `0100011` (7 bits)  
- **Immediate:** `8` (`0000000001000`, 12 bits split into imm[11:5] and imm[4:0])  
- **Source Register (rs2):** `ra` (x1, 5 bits)  
- **Base Register (rs1):** `sp` (x2, 5 bits)  
- **Function (funct3):** `011` (3 bits)  

#### Breakdown:  
- imm[11:5]: `0000000`  
- rs2 (ra = x1): `00001`  
- rs1 (sp = x2): `00010`  
- funct3: `011`  
- imm[4:0]: `01000`  
- Opcode: `0100011`  

#### Binary Representation:  
`0000000 00001 00010 011 01000 0100011`  

---

### 3. **`auipc a1, 1952`**
*Add Upper Immediate to PC:* Adds an upper 20-bit immediate to the PC and stores it in the destination register.  
**Format:** U-type
- **Opcode:** `0010111` (7 bits)  
- **Immediate:** `1952` (`000000011110` shifted left by 12 bits, 20 bits total)  
- **Destination Register (rd):** `a1` (x11, 5 bits)  

#### Breakdown:  
- imm[31:12]: `0000000000000111`  
- rd (a1 = x11): `01011`  
- Opcode: `0010111`  

#### Binary Representation:  
`0000000000000111 01011 0010111`  

---

### 4. **`jal ra, 1040c`**
*Jump and Link:* Saves the address of the next instruction in a register and jumps to a target address.  
- **Format:** J-type
- **Opcode:** `1101111` (7 bits)  
- **Immediate:** `1040c` (`0000010000001100`, split into parts)  
- **Destination Register (rd):** `ra` (x1, 5 bits)  

#### Breakdown:  
- imm[20]: `0`  
- imm[10:1]: `0000001100`  
- imm[11]: `0`  
- imm[19:12]: `00000100`  
- rd (ra = x1): `00001`  
- Opcode: `1101111`  

#### Binary Representation:  
`00000100 0000001100 0 00001 1101111`  

---

### 5. **`mv a1, a0`** *(Pseudo-instruction for `addi`)*
- **Format:** Same as `addi`, with `a1 = a0 + 0`. (I-type)  

#### Breakdown: 
- Immediate: `0`  
- rd (a1 = x11): `01011`  
- rs1 (a0 = x10): `01010`  
- funct3: `000`  
- Opcode: `0010011`  

#### Binary Representation:  
`000000000000 01010 000 01011 0010011`  

---

### 6. **`li a0, 33`** *(Pseudo-instruction for `addi`)*
- **Format:** Same as `addi`, with `a0 = 33`. (I-type) 

#### Breakdown:  
- Immediate: `33` (`000000100001`)  
- rd (a0 = x10): `01010`  
- rs1 (x0): `00000`  
- funct3: `000`  
- Opcode: `0010011`  

#### Binary Representation:  
`000000100001 00000 000 01010 0010011`  

---

### 7. **`ret`** *(Pseudo-instruction for `jalr`)*
- **Format:** Same as `jalr` with `rs1 = ra` and `imm = 0`. (I-type) 

#### Breakdown:  
- rd: `00000` (x0)  
- rs1: `00001` (ra)  
- funct3: `000`  
- imm: `0`  
- Opcode: `1100111`  

#### Binary Representation:  
`000000000000 00001 000 00000 1100111`  

---

### 8. **`ld a5, 8(a0)`**
*Load Doubleword:* Loads a 64-bit value from memory into a register.
- **Format:** J-type
- **Opcode:** `0000011` (7 bits)  
- **Immediate:** `8` (`0000000001000`, 12 bits split into imm[11:0])  
- **Destination Register (rd):** `a5` (x15, 5 bits)  
- **Base Register (rs1):** `a0` (x10, 5 bits)  
- **Function (funct3):** `011` (3 bits)  

#### Breakdown:  
- imm[11:0]: `000000001000`  
- rd (a5 = x15): `01111`  
- rs1 (a0 = x10): `01010`  
- funct3: `011`  
- Opcode: `0000011`  

#### Binary Representation:  
`000000001000 01010 011 01111 0000011`  

---

### 9. **`beqz a5, 1ff4`**
*Branch if Equal to Zero (Pseudo-instruction for `beq`):* Branches if a register equals zero. 
- **Format:** B-type
- **Opcode:** `1100011` (7 bits)  
- **Immediate:** `1ff4` (`0001111111110100`, split into imm[12|10:5|4:1|11] order)  
- **Source Register 1 (rs1):** `a5` (x15, 5 bits)  
- **Source Register 2 (rs2):** `x0` (always zero, 5 bits)  
- **Function (funct3):** `000` (3 bits)  

#### Breakdown:  
- imm[12]: `0`  
- imm[10:5]: `011111`  
- rs1 (a5 = x15): `01111`  
- rs2 (x0): `00000`  
- funct3: `000`  
- imm[4:1]: `1010`  
- imm[11]: `1`  
- Opcode: `1100011`  

#### Binary Representation:  
`0 011111 01111 00000 000 1010 1 1100011`  

---

### 10. **`lui gp, 1952`**
*Load Upper Immediate:* Loads a 20-bit immediate value into the upper 20 bits of a register.  
- **Format:** R-type
- **Opcode:** `0110111` (7 bits)  
- **Immediate:** `1952` (`000000011110` shifted left by 12 bits, 20 bits total)  
- **Destination Register (rd):** `gp` (x3, 5 bits)  

#### Breakdown:  
- imm[31:12]: `000000011110`  
- rd (gp = x3): `00011`  
- Opcode: `0110111`  

#### Binary Representation:  
`000000011110 00011 0110111`  

---

### 11. **`add a0, a0, a5`**
*Add:* Adds two registers and stores the result in the destination register.  
- **Format:** R-type
- **Opcode:** `0110011` (7 bits)  
- **Destination Register (rd):** `a0` (x10, 5 bits)  
- **Source Register 1 (rs1):** `a0` (x10, 5 bits)  
- **Source Register 2 (rs2):** `a5` (x15, 5 bits)  
- **Function (funct3):** `000` (3 bits)  
- **Function (funct7):** `0000000` (7 bits)  

#### Breakdown:  
- funct7: `0000000`  
- rs2 (a5 = x15): `01111`  
- rs1 (a0 = x10): `01010`  
- funct3: `000`  
- rd (a0 = x10): `01010`  
- Opcode: `0110011`  

#### Binary Representation:  
`0000000 01111 01010 000 01010 0110011`  

---

### 12. **`jalr zero, 0(ra)`**
*Jump and Link Register:* Saves the address of the next instruction into the destination register and jumps to the target address. 
- **Format:** I-type
- **Opcode:** `1100111` (7 bits)  
- **Immediate:** `0` (`000000000000`, 12 bits)  
- **Destination Register (rd):** `zero` (x0, 5 bits)  
- **Base Register (rs1):** `ra` (x1, 5 bits)  
- **Function (funct3):** `000` (3 bits)  

#### Breakdown:  
- imm[11:0]: `000000000000`  
- rd (zero = x0): `00000`  
- rs1 (ra = x1): `00001`  
- funct3: `000`  
- Opcode: `1100111`  

#### Binary Representation:  
`000000000000 00001 000 00000 1100111`  

---

### 13. **`jal zero, 12dfc`**
*Jump and Link:* Similar to `jalr`, except the immediate is used as a direct offset.  
- **Format:** j-type
- **Opcode:** `1101111` (7 bits)  
- **Immediate:** `12dfc` (split across imm[20|10:1|11|19:12]).  

#### Breakdown:  
- imm[20]: `0`  
- imm[10:1]: `1011111100`  
- imm[11]: `1`  
- imm[19:12]: `00010010`  
- rd (zero = x0): `00000`  
- Opcode: `1101111`  

#### Binary Representation:  
`00010010 1011111100 1 00000 1101111`  

---

### 14. **`call_exitprocs`** *(Pseudo-instruction calling another function)*  
This pseudo-instruction expands into `jal` with a target offset. 
- **Format:** j-type
- **Opcode:** `1101111`  
- Immediate and target would be calculated based on the address of `exitprocs`.  

---

### 15. **`li t0, 1`** *(Pseudo-instruction for `addi`)*
- **Format:** Same as `addi`, with `t0 = 1`. (I-type) 

#### Breakdown:  
- Immediate: `1` (`000000000001`)  
- rd (t0 = x5): `00101`  
- rs1 (x0): `00000`  
- funct3: `000`  
- Opcode: `0010011`  

#### Binary Representation:  
`000000000001 00000 000 00101 0010011`  

---

</details>
<details>
<summary><b>Task 4:</b> Risc-V iverilog functional simulation using gtkwave</summary> 

### Installing Icarus Verilog and GTKWave

#### **Step-by-Step Procedure**

1. **Update the package manager**:  
   ```bash
   sudo apt update
   ```

2. **Install Icarus Verilog**:  
   Icarus Verilog is a Verilog simulation and synthesis tool used for verifying Verilog designs.  
   ```bash
   sudo apt install iverilog
   ```

3. **Verify Icarus Verilog Installation**:  
   Check the version to confirm successful installation.  
   ```bash
   iverilog -v
   ```

4. **Install GTKWave**:  
   GTKWave is a waveform viewer for analyzing simulation outputs generated by tools like Icarus Verilog.  
   ```bash
   sudo apt install gtkwave
   ```

5. **Verify GTKWave Installation**:  
   Launch GTKWave to confirm it is installed.  
   ```bash
   gtkwave
   ```

#### **Brief Descriptions**

- **Icarus Verilog**:  
  A widely used open-source tool for simulating Verilog HDL. It supports Verilog-2005 and is commonly used for functional verification in digital design projects.  

- **GTKWave**:  
  A graphical tool for viewing simulation waveforms (e.g., `.vcd` files) generated by HDL simulators. It is instrumental in debugging and analyzing digital designs.

### Reference Repository 
  The Verilog codes used for simulation are sourced from the [iiitb_rv32i GitHub repository](https://github.com/vinayrayapati/rv32i/).

### Creating the Functional Simulation Block

Follow these steps to set up the directory and create the required files:

1. **Create a Directory**:  
   Use the `mkdir` command to create a new directory named `raj9`.  
   ```bash
   mkdir raj9
   ```

2. **Navigate to the Directory**:  
   Change into the newly created directory.  
   ```bash
   cd raj9
   ```

3. **Create Verilog Files**:  
   Use the `touch` command to create two files: `ex.v` (for the Verilog code) and `ex_tb.v` (for the testbench).  
   ```bash
   touch ex.v ex_tb.v
   ```
 

### Steps to Add the Code

1. **Open `ex.v` for Editing**:  
   Use any text editor like `nano`, `vim`, or `gedit`.  
   ```bash
   nano ex.v
   ```  

2. **Paste the Module Code**:  
   Copy and paste the following code into `ex.v`:
   ```verilog
   module iiitb_rv32i(clk,RN,NPC,WB_OUT);
   input clk;
   input RN;
   integer k;
   wire  EX_MEM_COND ;

   reg 
   BR_EN;

   // I_FETCH STAGE
   reg[31:0] 
   IF_ID_IR,
   IF_ID_NPC;                                
   ...
   (Rest of the module code as provided)
   ...
   endmodule
   ```  

3. **Save and Exit**:  
   In `nano`, press `Ctrl+O` to save and `Ctrl+X` to exit.  

4. **Open `ex_tb.v` for Editing**:  
   ```bash
   nano ex_tb.v
   ```  

5. **Paste the Testbench Code**:  
   Copy and paste the following code into `ex_tb.v`:  
   ```verilog
   module iiitb_rv32i_tb;

   reg clk,RN;
   wire [31:0]WB_OUT,NPC;

   iiitb_rv32i rv32(clk,RN,NPC,WB_OUT);

   always #3 clk=!clk;

   initial begin 
   RN  = 1'b1;
   clk = 1'b1;

   $dumpfile ("iiitb_rv32i.vcd"); // by default VCD
   $dumpvars (0, iiitb_rv32i_tb);
     
     #5 RN = 1'b0;
     
     #300 $finish;

   end
   endmodule
   ```  

6. **Save and Exit**:  
   Use `Ctrl+O` to save and `Ctrl+X` to exit in `nano`.  

This section will integrate the commands for running and simulating your Verilog project using `iverilog` and GTKWave.

---

### **Functional Simulation and Waveform Analysis**

1. **Compile the Verilog Files**:  
   Use the `iverilog` tool to compile the Verilog source file (`ex.v`) and its testbench (`ex_tb.v`) into an executable file named `iiitb_rv32i`.  
   ```bash
   iverilog -o iiitb_rv32i ex.v ex_tb.v
   ```

2. **Run the Simulation**:  
   Execute the compiled file to generate the Value Change Dump (VCD) file for waveform analysis.  
   ```bash
   ./iiitb_rv32i
   ```

3. **View the Waveform**:  
   Open the generated `iiitb_rv32i.vcd` file in GTKWave to analyze the simulation results.  
   ```bash
   gtkwave iiitb_rv32i.vcd
   ```
![gtk](https://github.com/user-attachments/assets/40b932a1-6727-4f8c-abb7-46a54824854e)

### Explanation of Hardcoded vs. Actual RISC-V ISA Instructions

#### 1. **Hardcoded Instructions**
- **Definition**: In the provided Verilog file, the instructions are **hardcoded**, meaning their binary representation (32-bit instruction format) does not follow the standard RISC-V ISA specifications. Instead, the designer used their own patterns to encode operations.
- **Example**:
  - **Hardcoded ADD Instruction**: `MEM[0] <= 32'h02208300;`  
    Here, `02208300` is the 32-bit encoding of the `ADD` instruction, but it does not match the standard RISC-V instruction format.

#### 2. **Actual RISC-V ISA Instructions**
- **Definition**: The RISC-V ISA uses a well-defined instruction format with standard opcodes, funct3, funct7 fields, and specific bit placements for operands and immediate values. These instructions are decoded in hardware to perform specific operations.
- **Example**:
  - **Standard ADD Instruction**:  
    Format: `opcode[6:0] | rd[11:7] | funct3[14:12] | rs1[19:15] | rs2[24:20] | funct7[31:25]`  
    Binary Encoding: `0000000 00010 00001 000 00110 0110011`  
    Hexadecimal: `0x00208033`

---

### Differences Between Hardcoded and Actual RISC-V ISA


| **Instruction** | **Description**              | **Actual Encoding** (Hard-coded) | **Standard Encoding** (RISC-V ISA) |
|------------------|------------------------------|-----------------------------------|-------------------------------------|
| `add r6,r1,r2`   | Addition of `r1` and `r2`   | `0x02208300`                      | `0x00008033`                        |
| `sub r7,r1,r2`   | Subtraction of `r1` and `r2`| `0x02209380`                      | `0x40008033`                        |
| `and r8,r1,r3`   | Bitwise AND of `r1` and `r3`| `0x0230A400`                      | `0x00708133`                        |
| `or r9,r2,r5`    | Bitwise OR of `r2` and `r5` | `0x02513480`                      | `0x00514133`                        |
| `xor r10,r1,r4`  | Bitwise XOR of `r1` and `r4`| `0x0240C500`                      | `0x0060C033`                        |
| `slt r11,r2,r4`  | Set Less Than               | `0x02415580`                      | `0x00415033`                        |
| `addi r12,r4,5`  | Add Immediate 5 to `r4`     | `0x00520600`                      | `0x00520213`                        |
| `sw r3,r1,2`     | Store Word                  | `0x00209181`                      | `0x00209023`                        |
| `lw r13,r1,2`    | Load Word                   | `0x00208681`                      | `0x00208283`                        |
| `beq r0,r0,15`   | Branch if Equal             | `0x00F00002`                      | `0x00F00063`                        |
| `add r14,r2,r2`  | Addition of `r2` and `r2`   | `0x00210700`                      | `0x00210133`                        |
| `bne r0,r1,20`   | Branch if Not Equal         | `0x01409002`                      | `0x01409063`                        |
| `srl r16,r14,r2` | Shift Right Logical         | `0x00271803`                      | `0x00271033`                        |
                                                          
---

### Functional simulation
![ev 1](https://github.com/user-attachments/assets/f0532e2c-7fc2-44e7-a810-2f4bd65deea1)


#### Instruction 1: ADD R6, R1, R2  


---

1. **Instruction Details:**  
   - The **ADD** instruction adds the values of two registers (`ID_EX_A` and `ID_EX_B`) and stores the result in the destination register.

2. **Values in Registers:**  
   - The value in `ID_EX_A` is `1`.  
   - The value in `ID_EX_B` is `2`.

3. **Output of ADD Operation:**  
   - The result of adding `1 + 2` is `3`, which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the ADD operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the first operand (`R1`).  
   - **ID_EX_B:** Represents the second operand (`R2`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R3`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the ADD instruction.

--- 

![WhatsApp Image 2024-12-10 at 16 06 54_21d28469](https://github.com/user-attachments/assets/3786c6f0-49ac-4488-8693-d155a6ab0db2)

- Hardcoded: `MEM[0] <= 32'h02208300;`  
- Standard RISC-V ISA: `0x00208033`  

---

#### Instruction 2: SUB R7, R1, R2 

---

1. **Instruction Details:**  
   - The **SUB** instruction subtracts the value of `ID_EX_B` from `ID_EX_A` and stores the result in the destination register.

2. **Values in Registers:**  
   - The value in `ID_EX_A` is `1`.  
   - The value in `ID_EX_B` is `2`.

3. **Output of SUB Operation:**  
   - The result of subtracting `1 - 2` is `-1`, which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the SUB operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the first operand (`R1`).  
   - **ID_EX_B:** Represents the second operand (`R2`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R3`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the SUB instruction.

--- 
![WhatsApp Image 2024-12-10 at 16 06 55_c8ca0db7](https://github.com/user-attachments/assets/a648cdd5-f93a-4acb-893e-88eb4f9689d9)

- Hardcoded: `MEM[1] <= 32'h02209380;`  
- Standard RISC-V ISA: `0x40208033`  

---

#### Instruction 3: AND R8, R1, R3  

---

1. **Instruction Details:**  
   - The **AND** instruction performs a bitwise AND operation between the values of `ID_EX_A` and `ID_EX_B`, and stores the result in the destination register.

2. **Values in Registers:**  
   - The value in `ID_EX_A` is `3` (binary: `11`).  
   - The value in `ID_EX_B` is `1` (binary: `01`).

3. **Output of AND Operation:**  
   - The result of the bitwise AND operation (`11 & 01`) is `01` (decimal: `1`), which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the AND operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the first operand (`R1`).  
   - **ID_EX_B:** Represents the second operand (`R2`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R3`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the AND instruction.

--- 

![WhatsApp Image 2024-12-10 at 16 06 55_826359e7](https://github.com/user-attachments/assets/8c5bf76e-5fc6-4c56-8ce7-474a10dd950c)

- Hardcoded: `MEM[2] <= 32'h0230A400;`  
- Standard RISC-V ISA: `0x0030A033`  

---

#### Instruction 4: OR R9, R2, R5  


---

1. **Instruction Details:**  
   - The **OR** instruction performs a bitwise OR operation between the values of `ID_EX_A` and `ID_EX_B`, and stores the result in the destination register.

2. **Values in Registers:**  
   - The value in `ID_EX_A` is `2` (binary: `0010`).  
   - The value in `ID_EX_B` is `5` (binary: `0101`).

3. **Output of OR Operation:**  
   - The result of the bitwise OR operation (`0010 | 0101`) is `0111` (decimal: `7`), which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the OR operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the first operand (`R1`).  
   - **ID_EX_B:** Represents the second operand (`R2`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R3`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the OR instruction.

---

![WhatsApp Image 2024-12-10 at 16 06 55_bc0771b1](https://github.com/user-attachments/assets/00dfedea-222a-4613-9a92-e04ae5730269)

- Hardcoded: `MEM[3] <= 32'h02513480;`  
- Standard RISC-V ISA: `0x00512033`  

---

#### Instruction 5: XOR R10, R1, R4  

---

1. **Instruction Details:**  
   - The **XOR** instruction performs a bitwise XOR operation between the values of `ID_EX_A` and `ID_EX_B`, and stores the result in the destination register.

2. **Values in Registers:**  
   - The value in `ID_EX_A` is `1` (binary: `0001`).  
   - The value in `ID_EX_B` is `4` (binary: `0100`).

3. **Output of XOR Operation:**  
   - The result of the bitwise XOR operation (`0001 ^ 0100`) is `0101` (decimal: `5`), which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the XOR operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the first operand (`R1`).  
   - **ID_EX_B:** Represents the second operand (`R4`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R10`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the XOR instruction.

---
![WhatsApp Image 2024-12-10 at 16 06 56_815b24a1](https://github.com/user-attachments/assets/ec9cda52-d1d3-4ebd-b71b-894aba795aed)

- Hardcoded: `MEM[4] <= 32'h0240C500;`  
- Standard RISC-V ISA: `0x0040C033`  

---

#### Instruction 6: SLT R11, R2, R4  

---

1. **Instruction Details:**  
   - The **SLT (Set Less Than)** instruction compares the values of `ID_EX_A` and `ID_EX_B`. If the value in `ID_EX_A` is less than the value in `ID_EX_B`, the result stored in the destination register is `1`; otherwise, it is `0`.

2. **Values in Registers:**  
   - The value in `ID_EX_A` is `2`.  
   - The value in `ID_EX_B` is `4`.

3. **Output of SLT Operation:**  
   - Since `2 < 4`, the result of the SLT operation is `1`, which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the SLT operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the first operand (`R2`).  
   - **ID_EX_B:** Represents the second operand (`R4`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R1`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the SLT instruction.

---
![WhatsApp Image 2024-12-10 at 16 06 56_2e8ede74](https://github.com/user-attachments/assets/bc76799b-3bac-4b5a-ac96-fac5cd71407e)
- Hardcoded: `MEM[5] <= 32'h02415580;`  
- Standard RISC-V ISA: `0x00415033`  

---

#### Instruction 7: ADDI R12, R4, 5  

---

1. **Instruction Details:**  
   - The **ADDI (Add Immediate)** instruction adds an immediate value to the value stored in a source register and stores the result in the destination register.

2. **Values in Registers and Immediate Value:**  
   - The value in the source register `ID_EX_B` is `4`.  
   - The immediate value is `5`.

3. **Output of ADDI Operation:**  
   - The result of the addition (`4 + 5`) is `9`, which is stored in `EX_MEM_ALUOUT`.

4. **Instruction Format:**  
   - The **32-bit instruction** for the ADDI operation is represented in `EX_MEM_IR`, with the specific encoding for this operation.

5. **Waveform Signals:**  
   - **ID_EX_B:** Represents the source operand (`R4`).  
   - **ID_EX_IMMEDIATE:** Represents the immediate value (`5`).  
   - **EX_MEM_ALUOUT:** Stores the output result (`R12`).  
   - **EX_MEM_IR:** Displays the hardcoded 32-bit ISA for the ADDI instruction.

---
![WhatsApp Image 2024-12-10 at 16 06 57_74ac6199](https://github.com/user-attachments/assets/bd40a68d-d0cc-4cb1-8ddc-81f76b9f8f50)
- Hardcoded: `MEM[6] <= 32'h00520600;`  
- Standard RISC-V ISA: `0x00520013`  

---

#### Instruction 8: BEQ R0, R0, 15 


---

1. **Instruction Details:**  
   - The *BEQ* (Branch if Equal) instruction checks if the values in two registers (R0 and R0 in this case) are equal. If they are, the program counter (PC) is updated by adding the immediate value specified in the instruction.

2. **Values in Registers:**  
   - The value stored in register R0 is `11`.

3. **Branch Operation:**  
   - Since the values in both registers (R0 and R0) are equal, the PC is incremented by the immediate value (15) provided in the instruction.  
   - New PC value = `10` (previous PC) + `15` (immediate) = `25`.

4. **Instruction Format:**  
   - The *32-bit instruction* for the BEQ operation is represented in the EX_MEM_IR signal, with the opcode and immediate values encoded appropriately.

5. **Waveform Signals:**  
   - *Program Counter (PC):* Displays the current instruction address.  
   - *EX_MEM_IR:* Represents the 32-bit encoded BEQ instruction.  
   - *EX_MEM_ALUOUT:* Shows the updated PC value after the branch operation.  

---

![WhatsApp Image 2024-12-10 at 16 25 02_89a3d534](https://github.com/user-attachments/assets/7333670e-d865-480c-8e97-a33668aa098d)

- Hardcoded: `MEM[9] <= 32'h00F00002;`  
- Standard RISC-V ISA: `0x00F00063`  
---

#### Instruction 9: BNE R0, R1, 20  
---
1. **Program Counter (PC):** Tracks the address of the current instruction being executed.
2. **Instruction Behavior:**  
   - The **BNE** (Branch if Not Equal) instruction checks the values stored in two registers (`R0` and `R1` in this case).  
   - If the values in the two registers are not equal, the PC is incremented by the immediate value provided in the instruction (20 in this case).  
   - Here, the initial PC value is 11. After execution, since `R0` ≠ `R1`, the PC is updated to `10 + 11 = 31`.
---
![hd 1](https://github.com/user-attachments/assets/96d392ac-ffdd-4679-b304-3a7236e3a5d1)

- Hardcoded: `MEM[27] <= 32'h01409002;`  
- Standard RISC-V ISA: `0x01408063`  
---

#### Instruction 10: SRL R16, R14, R2  

---


1. **Instruction Details:**  
   - The **SLL** (Shift Left Logical) instruction shifts the bits of the source register (`R1`) to the left by the number of positions specified in another register (`R2`).
2. **Values in Registers:**  
   - The value stored in `R1` is `1` (binary: `0001`).
   - The amount of shift specified in `R2` is `2`.
3. **Output of SLL:**  
   - The output after shifting `0001` left by 2 positions is `0100` (decimal: `4`), as shown in `EX_MEM_ALUOUT`.
4. **Instruction Format:** The **32-bit instruction** for `SLL R15, R1, R2` is highlighted in the waveform.
5. **Waveform Signals:**  
   - **ID_EX_A:** Represents the value to be shifted (`R1`).  
   - **ID_EX_B:** Represents the shift amount (`R2`).  
   - **EX_MEM_ALUOUT:** Represents the result of the shift operation.

---

![hd2 1](https://github.com/user-attachments/assets/796d2b5c-559e-45a1-91cb-6f1f957f732e)

- Hardcoded: `MEM[51] <= 32'h00271803;`  
- Standard RISC-V ISA: `0x00271033`  

---   

</details>
<details>
<summary><b>Task 5:</b> binary to 7-segment display decoder implementation using Vsdsquadron-Mini </summary> 

Here’s the updated README content including the details of which segments (`a` to `g`) glow for each number (0-9):

---

# Binary to 7-Segment Display Decoder Using Vsdsquadron-Mini

## Project Overview
This project implements a binary (0-9) to 7-segment display decoder using the Vsdsquadron-Mini board. The input binary values are provided via push buttons, and the corresponding decimal digit is displayed on a common-anode 7-segment display.

---

## Hardware Setup
### Components Required

- **VSD Squadron Mini Microcontroller**  
- **4.7 kΩ Resistors**  
- **Breadboard**  
- **7-Segment Display (Common Cathode)**  
- **4 Push Buttons**  
- **Connecting Wires**  
- **3.3V Power Supply**

### Push Button Inputs
- **Connections**:
  - Four push buttons connected to ports `PC0`, `PC1`, `PC2`, and `PC3` on the Vsdsquadron-Mini.
  - **MSB**: `PC3`, **LSB**: `PC0`.
  - Push button inputs are connected to the **3.3V supply** pin from the Vsdsquadron-Mini.
  - Push button grounds are connected to the **GND** pin of the Vsdsquadron-Mini.

### Seven-Segment Display
- **Type**: Common-Anode
- **Commons**: Connected to the **3.3V supply** pin from the Vsdsquadron-Mini.
- **Segment Pin Mapping**:
  - `A` → `PC4`
  - `B` → `PD2`
  - `C` → `PD3`
  - `D` → `PD4`
  - `E` → `PD5`
  - `F` → `PD6`
  - `G` → `PC5`

---

## LED Segment States for Numbers
| **Number** | **Segments Glowing** | **Binary Input (PC3-PC0)** |
|------------|-----------------------|----------------------------|
| 0          | a, b, c, d, e, f     | 0000                       |
| 1          | b, c                 | 0001                       |
| 2          | a, b, g, e, d        | 0010                       |
| 3          | a, b, g, c, d        | 0011                       |
| 4          | f, g, b, c           | 0100                       |
| 5          | a, f, g, c, d        | 0101                       |
| 6          | a, f, g, e, c, d     | 0110                       |
| 7          | a, b, c              | 0111                       |
| 8          | a, b, c, d, e, f, g  | 1000                       |
| 9          | a, b, c, d, f, g     | 1001                       |

---

## Circuit Diagram

![A](https://github.com/user-attachments/assets/73427e50-bb0f-4ef5-8270-4da9b46eaba0)



---

## Functionality
- The circuit decodes binary inputs from the push buttons and drives the 7-segment display to show digits `0-9`.
- Push button inputs represent a **4-bit binary number**:
  - `0000` = `0`, `0001` = `1`, ..., `1001` = `9`.
- Invalid inputs (`1010` to `1111`) are not displayed.

---
## Code 
```
#include <ch32v00x.h>

// Segment definitions
#define SEG_A GPIO_Pin_4   // PC4
#define SEG_B GPIO_Pin_2   // PD2
#define SEG_C GPIO_Pin_3   // PD3
#define SEG_D GPIO_Pin_4   // PD4
#define SEG_E GPIO_Pin_5   // PD5
#define SEG_F GPIO_Pin_6   // PD6
#define SEG_G GPIO_Pin_5   // PC5

// Port definitions
#define SEG_PORT_D GPIOD   // Port for segments B, C, D, E, F
#define SEG_PORT_C GPIOC   // Port for segments A and G
#define BUTTON_PORT GPIOC  // Port for BCD inputs
#define BCD_MASK (GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3)

// Function prototypes
void GPIO_Config(void);
void Display_7Seg(uint8_t num);
void Custom_Delay_Ms(uint32_t ms);

int main(void)
{
    SystemInit();      // System initialization
    GPIO_Config();     // Configure GPIO for 7-segment display and buttons

    while (1)
    {
        uint8_t bcd_input = GPIO_ReadInputData(BUTTON_PORT) & BCD_MASK;

        // Map BCD inputs to corresponding decimal numbers (0-9)
        uint8_t digit = 0;
        if (bcd_input <= 9)  // Valid BCD values range from 0 to 9
        {
            digit = bcd_input;
        }

        Display_7Seg(digit);  // Display the digit on the 7-segment display
        Custom_Delay_Ms(200); // Small delay to debounce and stabilize
    }
}

void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // Enable clocks for GPIOD and GPIOC
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOC, ENABLE);

    // Configure 7-segment display pins (GPIOD: B, C, D, E, F)
    GPIO_InitStructure.GPIO_Pin = SEG_B | SEG_C | SEG_D | SEG_E | SEG_F;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;  // Push-pull output
    GPIO_Init(SEG_PORT_D, &GPIO_InitStructure);

    // Configure 7-segment display pins (GPIOC: A, G)
    GPIO_InitStructure.GPIO_Pin = SEG_A | SEG_G;
    GPIO_Init(SEG_PORT_C, &GPIO_InitStructure);

    // Configure BCD input pins (PC0 - PC3) as input with pull-down resistors
    GPIO_InitStructure.GPIO_Pin = BCD_MASK;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;  // Input with pull-down
    GPIO_Init(BUTTON_PORT, &GPIO_InitStructure);
}

void Display_7Seg(uint8_t num)
{
    // Turn off all segments
    GPIO_ResetBits(SEG_PORT_D, SEG_B | SEG_C | SEG_D | SEG_E | SEG_F);
    GPIO_ResetBits(SEG_PORT_C, SEG_A | SEG_G);

    // Activate segments based on the BCD digit
    switch (num)
    {
        case 0:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C | SEG_D | SEG_E | SEG_F);
            GPIO_SetBits(SEG_PORT_C, SEG_A);  // A
            break;
        case 1:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C);
            break;
        case 2:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_D | SEG_E);
            GPIO_SetBits(SEG_PORT_C, SEG_A | SEG_G);  // A, G
            break;
        case 3:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C | SEG_D);
            GPIO_SetBits(SEG_PORT_C, SEG_A | SEG_G);  // A, G
            break;
        case 4:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C | SEG_F);
            GPIO_SetBits(SEG_PORT_C, SEG_G);  // G
            break;
        case 5:
            GPIO_SetBits(SEG_PORT_D, SEG_C | SEG_D | SEG_F);
            GPIO_SetBits(SEG_PORT_C, SEG_A | SEG_G);  // A, G
            break;
        case 6:
            GPIO_SetBits(SEG_PORT_D, SEG_C | SEG_D | SEG_E | SEG_F);
            GPIO_SetBits(SEG_PORT_C, SEG_A | SEG_G);  // A, G
            break;
        case 7:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C);
            GPIO_SetBits(SEG_PORT_C, SEG_A);  // A
            break;
        case 8:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C | SEG_D | SEG_E | SEG_F);
            GPIO_SetBits(SEG_PORT_C, SEG_A | SEG_G);  // A, G
            break;
        case 9:
            GPIO_SetBits(SEG_PORT_D, SEG_B | SEG_C | SEG_D | SEG_F);
            GPIO_SetBits(SEG_PORT_C, SEG_A | SEG_G);  // A, G
            break;
        default:
            break;
    }
}

void Custom_Delay_Ms(uint32_t ms)
{
    uint32_t i, j;
    for (i = 0; i < ms; i++)
    {
        for (j = 0; j < 1200; j++)  // Approximate delay loop
        {
            __NOP();
        }
    }
}
```
---
## Video link
[Click here](https://drive.google.com/file/d/1nke861P1vlEcE5wyuMgBgh0vID1Q3gkU/view?usp=drive_link)










