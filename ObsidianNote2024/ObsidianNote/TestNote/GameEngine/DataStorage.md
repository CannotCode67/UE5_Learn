![[DataStorage.png]]

Basically, statically defined data storage means the size and layout of the memory is known. Dynamically memory allocation means the size and layout of the memory is unknown, and this kind of memory resides on the heap.  

Dynamically memory allocation should be avoided wherever possible, because writing and reading data from heap is relatively expensive nowadays. 

Modern CPUs are many times faster than the old ones, but the speed to read and write data from memory remains about the same. Therefore, a program's performance is dominated by its memory access patterns. Data locating in a contiguous block of memory can be operated much more efficiently than data were to be spread out across a wide range of memory addresses. 

Dynamically memory allocation is slow due to the fact that the default methods is written for a general purpose, and the management overhead enables the methods to handle any allocation but at the same time makes them very slow. The second reason is in order to access the memory, the process needs to switch to kernel mode, do its own things, switch back to user mode to continue the program. Those switching is expensive. 

UserMode & KernelMode  

[[[In it’s life span a process executes in user mode and kernel mode. The User mode is normal mode where the process has limited access. While the Kernel mode is the privileged mode where the process has unrestricted access to system resources like hardware, memory, etc. A process can access I/O Hardware registers to program it, can execute OS kernel code and access kernel data in Kernel mode. Anything related to Process management, IO hardware management, and Memory management requires process to execute in Kernel mode. 

This is important to know that a process in Kernel mode get power to access any device and memory, and same time any crash in kernel mode brings down the whole system. But any crash in user mode brings down the faulty process only. 

The kernel provides System Call Interface (SCI), which are the entry points for kernel. System Calls are the only way through which a process can go into kernel mode from user mode. Below diagram explains user mode to kernel mode transition in detail.]]] 

Many big studios would write their own custom allocators methods because entirely avoiding dynamic memory allocation is impossible, but if they need to do it, custom allocators are more efficient. 

Many big studios would also write their own custom container classes along with corresponding iterator classes. Some complex container structures are very efficient to certain operations, and iterators are there to make the iteration easy by dealing all the complex traversals in the back end.