# Process Environment
## Main thread

A C program starts execution with main function, the prototype is
```C
int main(int argc, char *argv[])
```

When a C program is executed by the kernel -- by one of the *exec* functions --
a special start-up routine is called before the main function is called. This program
will specify this routine as the entrance, this is set up by the link editor when it
is invoked by the C compiler. The start up routine will takes values from the kernel --
the command line arguments and environment.

## Process Termination
Normal ways are,
1. Return from main
2. Calling exit
3. Calling _exit or _Exit
4. Return of the last thread from it's start routine
5. Calling pthread\_exit from the last thread

Abnormal ways,
1. Calling abort
2. Receipt of a signal
3. Response of the last thread to a cancellation request

## atexit Function
With ISO C, a process can register up to 32 functions that are automatically called by exit.
These are called exit handlers and are registered by calling the atexit function.

```C
#include <stdlib.h>

int atexit(void (*func)(void));
```

## Environment List
Each program is also passed an _environment list_. Like argument list, the environment list
is an array of character pointers, with each pointer containing the address of a null-terminated
C string.

```C
extern char **environ;
```
ISO C provides a function to get environment variables
```C
#include <stdlib.h>

char *getenv(const char *name);
```

## Memory Layout of C Program

- Text segment. A readonly segment of memory for machine instructions.
- Initialized data segment.  Contains variables that are specially initialized in the program,
```C
int masxcount = 89;
```
this variable declared outside any function cause it to be stored in initialized data segment.

- Uninitialized data segment, often called "bss" segment that stands for "block started by symbol".
Data in this segment is initialized by the kernel to arithmetic 0 or null pointers before the 
program starts execution. The C declaration
```C
long sum[1000]
```
appearing outside any function cause it to be stored in the uninitialized data segment.

- Stack. Automatic variables are stored, along with information that is saved each time a function
is called. Each time a function is called, the address of where to return to and certain information
about the caller's environment, such as some of the machine registers, are saved on the stack. Stack
frame will be created each time a function is called, so the recursive function's variable set will
not interfere by another's.

- Heap, where dynamic memory allocation usually take place.

A typical arragement of these segment could be: the stack grows from the higher-numbered address to
lower-numbered address, the heap in contrast. With Linux on an Intel x86 processor, the text segment
starts at location 0x08048000, and the bottom of stack starts just below 0xC0000000.

## Shared Libraries
gcc defaults to use shared libraries. we could use *-static* to prevent.

## Misc
There still some other contents, like Atomic, register, setjump and longjump functions and getrlimit.

# Threads
## Thread Identification
Every thread has a thread ID which is represented by pthread\_t data type.

## Thread Creation

The thread creation need to call to the kernel, so it could be expensive compared to function calls.

To synchronize threads, we could use mutex or reader-writer locks.
We could simulate counting semaphores with mutex and conditional variables. Check this
[page](http://www.cs.wustl.edu/~schmidt/win32-cv-1.html)


