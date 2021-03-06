# Memory Barrier

A memory barrier, also know as a membar, memory fence or fence instruction, is a type of barrier instruction that causes a central processing unit (CPU) or compiler to enforce an ordering constraint on memory operations issued before and after the barrier instructions. This typically means that operations issued prior to the barrier are guaranteed to be performed before operations issued after the barrier.

Memory barriers are typically used when implementing low-level machine code that operates on memory shared by multiple devices. Such code includes synchronization primitives and lock-free data structures on multiprocessor systems, and device drivers that communicate with computer hardware.

Memory barriers are low-level primitives and part tof an architecture's memory model, which, lick instruction sets, vary considerably between architectures, so it is not appropriate to generalize about memory barrier behavior. The conventional wisdom is that using memory barriers correctly requires careful study of the architecture manuals for the hardware being programmed.

Mutlithreaded programs usually use synchronization primitives provided by a high-level programming environment, such as Jave and .NET Framework, or an application programming interface (API) such as POSIX. Primitives such as mutexes and semaphores are provided to synchronize access to resources from parallel threads of execution. These primitives are usually implemented with the memory barriers required to provide the expected visibility semantices. In such environments explicit use of memory barriers is not generally necessary.

For my current stage of understanding, I think this will prevent "Out-of-order" execution. Also from reading the Linux code, compiler reordering optimization could also cause "Out-of-order" execution.

Memory barrier instructions address reordering effects only at the hardware level. Compilers may also reorder instructions as part of the program optimization process. Although the effects on parallel program behavior can be similar in both cases, in general it is necessary to take separate measures to inhibit compiler reordering optimizations for data that may be shared by multiple threads of execution. Note that such measures are usually necessary only for data which is not protected by synchronization primitives such as those discussed in the prior section.

In C and C++, the volatile keyword was intended to allow C and C++ programs to directly access memory-mapped I/O. Memory-mapped I/O generally requires that the reads and writes, specified in source code happen in the exact order specified with no omissions. Omissions or reorderings of reads and writes by the compiler would break the communication between the program and the device accessed by memory-mapped I/O. A C or C++ compilter may not omit reads from and writes to volatile memory locations, nor may it reorder read/writes relative to other such actions for the same volatile location (variable). The keyword volatile does not grarantee a memory barrier to enforce cahce-consistency. Therefore, the use of "volatile" alone is not sufficient to use a variable for inter-thread communication on all systems and processors.

The C and C++ standards prior to C!! and C++11 do not address multiple threads (or multiple processors), and as such, the usefulness of volatile depends on the compilter and hardware. Although volatile guarantees that the volatile read and write is reordered with regard to non-volatile reads or writes, thus limiting its usefulness as an inter-thread flag or mutex. Preventing such is compiler specific, but some compilers, like gcc, will not reorder operations around in-line assembly code with volatile and "memory" tags, like in: asm volatile ("" : : : "memory"). Moreover, it is not guaranteed that volatile reads and writes will be seen in the same order by other processors or cores due to chaching. cache coherence protocol and relaxed memory ordering, meaning volatile varibles alone may not even work as inter-thread flags or mutexes.

In Java version 1.5, the volatile keyword is now guaranteed to prevent certail hardware and compliler reorderings, as part of the new Java Memory Model.

References:
https://www.kernel.org/doc/Documentation/volatile-considered-harmful.txt
https://en.wikipedia.org/wiki/Memory_barrier#frbanner3
