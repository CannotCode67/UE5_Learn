
By now, we know `ifstream` object and `ofstream` object, but there is `fstream` object. When we call the constructors of `ifstream` or `ofstream`, the constructor knows whether we are reading from a file or writing to a file. With `fstream` object, it doesn't know, so `fstream` object can read or write to a file.

That's a bit extra knowledge we would like to know. Now, C++ gives us a number of options for opening a file, which are file modes. By giving an optional second argument when opening a file, we can specify which file mode we want.

By default, output files are opened in truncate mode. This mode means any original data in the file will be overwritten by new data. 
Another mode for output file is append mode, which new data will be appended at the back of the original data in the file.
`ofile.open("text.txt", fstream::app);`
Here we use `open()` method, only this would accept the file mode argument.

By default, files are opened in text mode. Here, it mean we open the file with `ifstream` object or `fstram` object. With this mode, if we write 42 to a file, it will be stored as the ASCII characters, '4' and '2'. 
Another mode for all files is binary mode. If we write 42 to the file, it will be stored in as a single binary number, 101010. This mode is for low level.

Some other modes, and their restrictions:
`fstream::trunc`, the explicit truncate mode argument, only for output.
`fstream::in`, open the file for input, so automatically text mode, `fstream` & `ifstram` only.
`fstream::out`, open the file for output, so automatically truncate mode, `fstream` & `ofstram` only.
`fstream::ate`, similar to append, but new data can be added to anywhere in the file.
`fstream::app`, append mode, only for output, cannot combine with truncate mode.
`fstream::binary` binary mode

We can combine different modes by putting a vertical bar `|` between them.
```
fstream file; // fstream constructor without opening specific file
file.open("text.txt", fstream::out | fstream::app); // open for output in append mode
```
If we open a file for input and output, the default mode will be text mode unless we specify it.
```
file.open("text.txt", fstream::in | fstream::out | fstream::trunc);
```
