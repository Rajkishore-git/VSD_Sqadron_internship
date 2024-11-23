# VSD_Sqadron_internship
The program is based on the RISC-V architecture and uses open-source tools to teach people about VLSI chip design and RISC-V. The instructor for this internship is Kunal Ghosh Sir.

<details>
<summary><b>Task 1:</b> C based and RISC-V based programs for sum of n numbers</summary>   
<br>

C based
------------------------------------------

Install leafpad editor 

*Use the following command for installing leafpad*
```
sudo apt install leafpad
```
Now we need to write a program in c for sum of 1 to n numbers, and save the file as "sum1ton.c"
![sum1ton](https://github.com/user-attachments/assets/caa1a9c8-47b8-4a39-a63b-9856688f4030)

Now after we compile this and run using the commands :

```
gcc sum1ton.c
./a.out
```
The output of the c code is :

![output sum1ton](https://github.com/user-attachments/assets/db5285ce-fd32-482c-af8e-c38acb9d30af)

RISC-V based
------------------------------------------

We can view the sum code using the following command :
```
cat sum1ton.c
```
The terminal output of the above the commad :

![out2ter](https://github.com/user-attachments/assets/092f17d4-e9bb-4aff-a4c4-007c34175521)

For compiling the above code in RISC-V we use the command :
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
![o3](https://github.com/user-attachments/assets/5082ad98-3b87-4bfb-a67b-7d8829a09414)

Now the file has been saved "sum1ton.o"
In the new tab we need to give the command ``` riscv64-unknown-elf-objdump -d sum1ton.o | less ```

Now the assembly language code for ```O1``` is :

![o4](https://github.com/user-attachments/assets/3486c762-b7cd-4075-bb03-a86cc3493104)

Here if we calculate the number of instructions, we get the total instructions as 11.
It is calculated as 
``` 
101b0 - 10184 = 2c
2c/4 = b  => 11
```
Now similarly we need to execute the code for ``` Ofast ``` command

The input is shown as :

![o5](https://github.com/user-attachments/assets/e5f4ddfd-36df-40c8-8271-18431cadf94d)

The output of the ``` Ofast ``` command is :

![06](https://github.com/user-attachments/assets/4f8b3ca2-fd88-4dda-a896-6485e142ca08)

Again if we calculate the number of instructions , we get the instructions as 11.
It is calculated as 
``` 
100dc - 100b0 = 2c
2c/4 = b  => 11
```



</details>





