
So far, we have learnt two kinds of C++ streams, the `iostream` for console, and `fstream` for file. There is one more stream, the `stringstream`m `ostringstream` for writing, and `istringstream` for reading. `stringstream` is defined in the `<sstream>` header. It is a wrapper class around `std::string` so that we have a similar interface like other stream but dealing with `std::string`.

```
template <typename T>
string To_String(const T& t) {
	ostringstream os; // construct an empty stringstream object
	os << t; // write data from t to os like other stream object
	return os.str(); // str() returns the string it has
}
```
Why don't we directly work with string? I don't know, I guess we can come back to the details once I figure out the why.

