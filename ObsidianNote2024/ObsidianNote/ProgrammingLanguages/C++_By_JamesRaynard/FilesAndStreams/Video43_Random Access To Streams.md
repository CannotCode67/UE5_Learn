
C++ streams have a position marker member which keeps track of where the next read or write action is performed. By default, the stream controls the marker's position. At the end of the stream for write operations. Just after the last read for read operations. However, in some cases, the programmer can alter its position. `fstream` not opened in append mode, or `stringstream`.

Stream objects provide following member functions to alter the marker.
`tellg()` returns the current position in an input stream in a `pos_type` object.
`tellp()` returns the current position in an output stream in a `pos_type` object.
`seekg()` sets the current position in an input stream, taking a `pos_type` object as argument.
`seekp()` sets the current position in an output stream, taking a `pos_type` object as argument.

The `pos_type` object can be converted to `int`, and it would equal to -1 if fails. So a check needs to be made before using this object.
```
auto pos = file.tellg();
if (pos != -1) {
	...
}
```

We acquire this position in a `pos_type` object, and it works like a save point. We can get back to this position as long as we have this variable.

The best way to modify a file is usually to read it into a `istringstream`, process the data however you like, then write it to the original file. The only reason we would use seek and tell operations to directly modify a file, is we don't have the memory to load the whole file.