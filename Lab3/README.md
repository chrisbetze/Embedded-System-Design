# ARM Assembly
## Description - Emulator
To run a program written in ARM assembly, we use virtualization. 
Specifically, we use a system simulator with ARM and the simulator run on a system that use a different processor, typically an x86 type. 
The simulator that we used is **QEMU** ([Quick Emulator](http://wiki.qemu.org/Main_Page)) which is open source and has the ability to simulate a large number of systems and processors.

## Part 1: Convert input from terminal
We write a program in ARM assembly, which receives an input string up to 32 characters from terminal. If the input is larger, the characters excess are ignored. 
In this string the following conversions are done:
* If the character is a capital letter, then converted to lowercase and vice versa.
* If the character is in the range ['0', '9'], the following conversion will take place:
    * '0' → '5'
    * '1' → '6'
    * '2' → '7'
    * '3' → '8'
    * '4' → '9'
    * '5' → '0'
    * '6' → '1'
    * '7' → '2'
    * '8' → '3'
    * '9' → '4'

The program is continuous and terminates when receives as input a string of length one, consisting of the character 'Q' or 'q'.

*The full steps of the program are described in the [report](https://github.com/chrisbetze/Embedded-System-Design/blob/25cee5b9eef1aebe16e5c4db56757fa778e92c3c/Lab3/report.pdf)*.

## Part 2: Communication of guest and host machines through a serial port
<img src="https://user-images.githubusercontent.com/50949470/111884030-894db300-89c7-11eb-8317-c5c7bd3eb23a.PNG" width="600" height=auto>

Qemu provides several features to support serial port to virtual machine. The obvious option is for the guest machine to use the serial port of host machine. 
But our pc does not have a serial port, so we use a feature of linux called **pseudoterminal**.

The purpose of the exercise is to create 2 programs, one in **C on the host** machine and one in **ARM on the guest** machine which will communicate via virtual serial port. 
* The program on the host machine will receive as input a string of size up to 64 characters. 
* This string will be sent via a serial port to the guest machine. 
* The guest will answer which is the character of the string with the highest frequency occurrence and how many times it occurred. 
* The blank character (ascii number 32) is excluded from counting. 
* In case two or more characters have a maximum occurrence frequency, the program will return to the host the character with the smallest ascii code.

*The full steps of the program and the string manipulation are described in the [report](https://github.com/chrisbetze/Embedded-System-Design/blob/25cee5b9eef1aebe16e5c4db56757fa778e92c3c/Lab3/report.pdf)*.

## Part 3: Linking C and ARM assembly code
The purpose of this exercise is to combine code written in C, with functions written in ARM assembly.
Specifically, we replace the following functions from *string.h* library used by *string_manipulation.c* program, with ours written in ARM assembly:
* strlen
* strcpy
* strcat
* strcmp

For each function, we declare it as *extern* in C code and its source code is written to another file that contains assembly code.
The function is declared in assembly code with the *.global* directive, so that it can be seen by linker when connecting the two files.
Then we just compile (-c flag of gcc) the *string_manipulation.c* file and the same for each file *.s* of functions. 
We linked the object files generated by the above steps with a *Makefile*, to produce the final executable.

*The full steps of the program are described in the [report](https://github.com/chrisbetze/Embedded-System-Design/blob/25cee5b9eef1aebe16e5c4db56757fa778e92c3c/Lab3/report.pdf)*.