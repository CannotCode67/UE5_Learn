
We mentioned data is gathered in a buffer, and only it reaches a size big enough, it would get sent to the file. Now, we give a little bit more details on this process.

It is C++ stream objects to be responsible for the buffering. During write operation, data is temporarily held in a memory buffer before it reaches a size. This size is usually the buffer's max size, and it is chosen to match the maximum amount of data that the operating system will accept. When the buffer is full, the stream will remove the data from buffer and send it to the operating system. This part is known as flushing the output buffer.

The flushing behavior can be adjusted.
For input streams, there is no direct way to flush.
For `ostream`, it depends on the terminal configuration. Usually it is the end of every line. `cout` is always flushed before the program reads from `cin`, because we should output everything we get before asking for input.
For `fstream`, it is only flushed when the buffer is full unless we use `std::flush` which allows us to control when the stream's buffer is flushed. `std::flush` and `std::endl` are known as stream manipulators.
`cout << i << flush; // i will appear on the screen immediately` 
`cout << i << endl;` is equal to `cout << i << "\n" << flush;`
It hurts performance, of course. Normally, we don't use it because as we mentioned, when the `fstream` destructor is called, any data in the buffer will be sent to the file. However, if the program crashes before the destructor is called, the C++ runtime would release the memory and close all the file. There is no guarantee that closing file will come before releasing memory, so there is a good chance the data in buffer will be lost without being sent to the file. As result, a log file like debug log to find out why a program crashed has to be flushed to be up to date. 