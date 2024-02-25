
There are some applications where stream buffering is not suitable. For example, a network application requires data to be transmitted in packets of a specified size. Or communicating with a hard device requires data to be transmitted at specific timing. The C++ stream built-in buffer's behavior is not desirable for them.

But luckily C++ has some low level operations for streams. These operations bypass the stream's buffer, so if we still need buffering, we would have to write our own. And they do not format data. If you use an output stream, it may add spaces or change numbers to make them look nice. If you have an input stream with `>>` operator, it removes all the write space. But with low level operations, no data formatting will happen.

Stream object has member functions for reading or writing data, one character at a time. That's the smallest unit we can do, 1 byte or 8 bits.
`get()` fetches the next character from an input stream.
`put()` sends its argument to an output stream.
```
char c;
while (cin.get(c)) // returns true unless we get the symbol of end of input
{
	cout.put(c); // output the character to the console
}
```
They transmit the data one character by one character underneath, but you don't need to type one character every time you press enter. You still type a sentence or a word like we usually do, and we see the output like what normally see. Just the way those data got transmitted is by one character after another, not by a batch of data.

If we want to transmit the data in a group of characters, steam object has member functions for that, `write()` and `read()`. However, we need to provide our own buffer. For `write()`, the buffer will contain all the data we want to send at a time, so we are defining the flushing buffer size. For `read()`, the buffer must be large enough to store all the data we expect to receive. Both functions first argument is a pointer to char. In example below, we pass the C-Style array like that because C-Style array without indexing is a pointer to the first element.
```
const int fileSize{10};
char fileBuffer[fileSize];
ifile.read(fileBuffer, fileSize); // fetch a fileSize amount of data from the file into buffer
cout.write(fileBuffer, fileSize); // send a fileSize amount of data from the buffer to console output
```

Often, we need to know how much data an input stream has sent us. We may need to allocate memory to process the data, or to detect partial or incomplete data transfers. For that, we have `gcount()` member function which returns the number of characters that were actually received, so how many bytes.
```
auto nread = ifile.gcount(); // the type is not int, but around int
cout << nread << endl;
```
