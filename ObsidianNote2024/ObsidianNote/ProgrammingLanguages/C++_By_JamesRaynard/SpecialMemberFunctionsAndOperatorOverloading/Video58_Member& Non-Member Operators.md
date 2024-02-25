
When we overload operators, we usually should implement them as member functions. But sometimes, we have to implement them as global functions.

```
class String {
	string s;
public:
	String(const char* str): s(str) {}
	String(const string& s): s(s) {}
	
	String operator +(const String& arg) {
		return s + arg.s;
	}

	void print() { cout << s << endl;}
};

int main()
{
	String w{"world"}, bang{"!"};
	String wbang = w + bang; //invoke w.operator+(bang)
	String hi = "hello" + w; //"hello".operator+(w), compiler error
}
```
The member operator would not make the jump to convert the `hello` into `String`, but a global one can.
```
String operator +(const String& arg1, const String& arg2);
```

Operators which change the state of object are best implemented as member functions: `+=, -=, *=, /=, ++, --, *`.
Some operators must be defined as member functions: `=, [], (), ->`.

Binary operators which might require a type conversion should be implemented globally.