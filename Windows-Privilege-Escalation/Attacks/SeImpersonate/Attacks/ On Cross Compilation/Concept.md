#  On Cross Compilation Privilege escalation
---
1. What is compilation
  - The process that use to translate **source code** into **binary code** 
  - compilation process have complex stages  

- Practical example
let's say we have the c code and need to compile it in linux 
```c
   #include <stdio.h>

int main(void) {
  printf("Hello World!\n");
  return 0;
}
```

To compile this code we will use
```bash
    gcc main.c -o compile
```
![](images/example-1.png)

ok let's see if we try to execute this file in windows machine it's run or not ?
- in linux
```bash
  nc -lnvp 4444 < complied.c
```
- in windows
```powershell
    .\nc64.exe 192.168.x.x 4444 > compile.c
```
![](images/ex2.png)

we can see that dont't running in windows
The reason is tin the linux the binary format used is the **ELF > **Executable and linked format** 
but in windows used id the **PE > Portable Executable** you can see difference from this blogs
1. [ELF](https://medium.com/@louaylouay20/understanding-the-elf-format-a-comprehensive-guide-3ee1dc938389#:~:text=ELF%2C%20or%20Executable%20and%20Linkable%20Format%2C%20is,for%20various%20processor%20architectures%20and%20operating%20systems.)

2. [PE](https://en.wikipedia.org/wiki/Portable_Executable)


To solve this problem we use **On cross Compilation**
1. install **sudo apt install mingw-w64** in linux
```bash
    sudo apt install mingw-w64
```

and Then execute
```bash
    x86_64-w64-mingw32-g++ main.c -static -o compile2
```
this will return
![](images/ex3.png)

```Explain This command```
1. x86_64-w64-mingw32-g++ 

  - **x86_64** > detect the target architecture as 64-bit
  - **w64-mingw32** > detect the target operating system is windows
  - **g++** > use c++ compiler

2. static
  - this linker option that instructs the linker to link all nessary libraries
    statically into the executable 
  - this make file self-contained and not dependent on external DLLs at runtime

Transfer file after excuted this and ok it's will run
![](images/ex4.png)

This Techiques use to make reverse shell and gain access to administrator in windows machine