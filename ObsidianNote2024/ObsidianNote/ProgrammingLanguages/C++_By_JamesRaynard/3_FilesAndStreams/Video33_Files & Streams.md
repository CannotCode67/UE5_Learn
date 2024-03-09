
In C++, a file is represented by a sequence of bytes, identified by a file name.

The standard library provides the `fstream` object to interact with files. `fstream` objects always access files sequentially, so no jumping forward or moving backward. `fstream` objects do not know or care the length of the file. `fstream` objects do not understand file formats either.

`fstream` operations
Open, it connects the `fstream` object to the file, the file becomes available for use by the program.
Read, data is copied from the file into the program's memory.
Write, data is copied from the program's memory into the file.
Close, this disconnects the `fstram` object from the file, the file is no longer available for use by the program unless another open operation comes along.

All the operations above, the `fstream` object will call a function in the operating system API. The program remains stop and wait before the operating system API finishes the task and returns, then the program resumes execution.

A file must be opened before we can use it, and should be closed right after we are done with it. There is a limit for a program to open how many files at anytime, just like a program asking for memory allocation. When C++ program terminates, the runtime will make sure close all still open files, just like it will release all the allocated memory. But we still need to manually manages the memory and manages file open and close.

As data passes between the program and the file, the data may be temporarily stored in a memory buffer, and until it reaches a size big enough, only then it will be sent to file. The reason is interacting with physical devices is very time-consuming. We batch up data so that the number to make the actual transfer is lower, thus more efficient.