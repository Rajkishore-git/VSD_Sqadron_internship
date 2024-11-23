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





