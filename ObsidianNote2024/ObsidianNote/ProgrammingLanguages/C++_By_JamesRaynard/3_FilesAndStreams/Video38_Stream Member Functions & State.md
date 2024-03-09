
We give a bit of extra detail about `open()` member function. We have to ways to bind a file to a `fstream` object. One way is to pass the file name when we call the constructor. The other way is to do the binding with `open()`. Both ways are valid, but only `open()` member function takes file mode arguments.
```
ofstream ofile1{"text.txt"};
ofstream ofile2;
ofile2.open("text.txt");
```

There are member functions to check the state of a `stream` object.
`ofile.is_open()` returns true if the object is bind to a file, similar to just put `ofile` into a condition.
`ifile.eof()` returns true after the end of file has been reached. It doesn't ignore whitespace, so if all are left in the file are whitespaces, it would still return false. And if we fetch data from the file with `>>` operator, because `>>` operator ignores whitespace, the input stream would keep repeating the last valid value for each remaining iteration. Probably not something desirable.
```
while (ifile.eof()) {
	ifile >> x; // ifile input stream will have the last valid value
	cout << x << endl;
}

// correct way to do this below
while (ifile >> x) { //ifile >> x will give the state of the input stream as bool value
	cout << x << endl;
}
```

Other state member functions are mostly related to input.
```
int x{0};
cin >> x; // wait for input, and once input is sent, put the data into x
if (cin.good()) // returns true if cin's last action succeeds
	cout << "good" << endl;
else if (cin.fail()) // returns true if cin's last action fails
	cout << "need an integer" << endl;
else if (cin.bad()) // returns true if cin's last action fails horribly
	cout << "seriously wrong" << endl;
```
The state is a member field, that once it has a value, it will keep the value until a new one place it. So for the example above, we input a word, and `cin` has a state of fail. It would remain so, until we have another input or we use `cin.clear()` to restore the state to original.

Let's go a bit more detail into the `cin >> x;` process. When this line is executed, we must have pressed the enter key to flush the input buffer of the console into the input buffer of the stream, `cin`. If `cin >> x;` fails, it means the conversion from data in the input stream buffer into x variable did not reach the end of the input stream buffer. It maybe a partial success or a total failure. But regardless, the first character causing the conversion failure and anything followed will be left in the input stream buffer. Next time, we read from `cin`, it would read whatever left behind in its buffer right away, causing a failure again. The solution to that is to force `cin` to flush its buffer, but input streams do not support flushing. However, we have an `ignore()` member function which takes two arguments, the maximum number of characters to ignore, and a delimiter (if we run into this character, we stop the ignoring right away). The ignored characters will then be thrown away. Now, how many characters to ignore? We can get the size of stream buffer from `numeric_limits<streamsize>::max()`, which is defined in `<limits>` header.
```
int x{0};
cin >> x;
bool state{false};
while (!state)
{
	if (cin.good()) {
		state = true;
	}
	else if (cin.fail()) {
		cout << "need an integer" << "\n";
		cin.clear(); // clear the state of cin
		cin.ignore(numeric_limits<streamsize>::max(), '\n'); // clear the buffer
		cin >> x; // try again
	}
}
```

