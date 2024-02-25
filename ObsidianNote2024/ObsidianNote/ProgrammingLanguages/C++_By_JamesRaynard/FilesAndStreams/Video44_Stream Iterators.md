
Stream iterators is the result when designers of standard library want to setup an iterator-consistent interface for dealing with streams. This makes stream iterators look redundant, but the good thing is if we use stream iterators to process streams, we can use some of the algorithms in standard library which only work for iterators.

Defined in the `<iterator>` header, there are `istream_iterator` reads an input stream and `ostream_iterator` writes an output stream. They are template classes, so when they instantiated we need to know what type of data we will deal with in the stream objects.

Need to do a binding with a stream object before use.
`ostream_iterator<int> oi(cout);`
Assigning data to an `ostream_iterator` will send the data onto the stream, and we need to dereference it. Increment it would take marker to next position.
`*oi = 7; // displays 7`
The `ostream_iterator` constructor can take a second argument which is a C-Style string. The argument will be sent to the stream after each data assignment.
```
ostream_iterator<int> oi(cout, "a");
for (int i = 0; i < 10; ++i) {
	*oi = i;
	++oi;
}

//display be like
//0a1a2a3a4a5a6a7a8a9a
```

`istream_iterator` can be dereferenced like regular iterator, and this will get the data at the current position in the stream. To increment `istream_iterator` will move the stream to the next data.
```
istream_iterator<int> ii(cin);
int x = *ii; // read the data from the current position of the stream
```
If there is no more input to read, `istream_iterator` will be empty. We can get an empty `istream_iterator` by not binding it to any stream object.
```
istream_iterator<string> iis(cin);
istream_iterator<string> eof; // this is the empty iterator
vector<string> vs;
while (iis != eof)
{
	vs.push_back(*iis);
	++iis;
}
```
