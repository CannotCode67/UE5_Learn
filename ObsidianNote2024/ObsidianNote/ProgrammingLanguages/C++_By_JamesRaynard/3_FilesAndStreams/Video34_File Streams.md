
We have used `cout`, and `cin` for `<iostream>`header, it is used for communicating with console. The `fstream` object is under the `<fstream>` header, and there are two kinds of them. `ofstream` object is for writing data to the file `o` for output, and `ifstream` object is for reading data from the file, `i` for input.
```
ifstream ifile{"text.txt"}; // opens text.txt file for reading
if (ifile) {
	
}
```
After we call the constructor of `ifstream`, it may or may not succeed. That's why we need to check if `ifile` is true, if `ifile` is true, then we successfully open the file. Before C++11, we have to use a C-Style string for the file name, but after that, we can use a standard string just fine. By the way, the file needs to be in the program directory.

Reading from a file, we can use the `>>` operator just like we do with iostream.
```
while (ifile >> text)
	cout << text << ",";
```
This will read one word at a time and remove all whitespace from the input. If this is not what you want, we have `getline()` function which read a complete line of input from the file, except the newline character.
```
while (getline(ifile, text))
	cout << text << endl;
```

After we are done reading from a file, we should close it right away.
`ifile.close();`

Now, for writing to a file, we can use `<<` operator.
```
ofstream ofile{"text_out.txt"};
if (ofile) {
	vector<string> words = {"Hello", "World"};
	for (auto word : words) {
		ofile << word << ",";
	}
}
```
The text_out.txt file will be overwritten by our program input, so what was originally there will be gone.

When `fstream` destructor is called, the file is automatically closed, for `ofstream` objects, this will cause any unsaved data to be written to the file. So on theory, when `fstream` object goes out of scope, we don't need to explicitly call `close()`. But we still do, in case anything unexpected happens before the end of scope.