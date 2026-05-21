UofTCTF Seminars 2026 

## Program Memory 
Programs are loaded from disk into RAM into a virtual memory space that the OS can interpret

## Virtual Memory Space 
This space contains: 
Binary (.text, .data, .bss, etc)
Stack 
Heap 
Kernel code (inaccessible) 
Helper regions 
Libraries 
Memory mapped by the program 
 
The **.text** section has the executed portion of the program

**Assembly** is the mneumonic we give to machine code


**.data, .bss, .rodata**
These sections are relating to global data in a program .data: literal data defined at compile time .bss: unknown data - variables with no value .rodata: read-only constants

