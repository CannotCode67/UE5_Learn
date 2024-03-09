
We always have a default constructor for built-in types. For classes, we always have one provided by compiler given we don't write one by ourselves. If our class has member fields of built-in types, we should initialize them with default values.
```
class refrigerator {
	private:
		int temp;
		bool power_on;
};

class refrigerator {
	private:
		int temp{2};
		bool power_on{true};
};
```
Sometimes, we write a constructor that takes arguments for initial values.
```
class refrigerator {
	private:
		int temp{2};
		bool power_on{true};
	public:
		refregerator(int temp, bool power): temp(temp), power_on(power){}
};
```
If we have a function we want the object to run upon creation, we put it in the constructor as well.
```
class refrigerator {
	private:
		int temp{2};
		bool power_on{true};
	public:
		refregerator(int temp, bool power, std::string word): temp(temp), power_on(power)
		{
			SaySomething(word);
		}
		void SaySomething(word) {
			cout << word << endl;
		}
};
```

Above is very basic. Now, imagine we need to provide a constructor asking for only the `temperature` argument, or provide a constructor asking for only `power_on` argument and so on. It seems we have so many constructors to write. Therefore, C++11 allows us to delegate constructor, meaning we can call another constructor in one constructor.
```
class refrigerator {
	private:
		int temp{2};
		bool power_on{true};
	public:
		refregerator(std::string word): refregerator(2, true, word);
		refregerator(bool power): refregerator(2, power, "Hello"s);
		refregerator(int temp): refregerator(temp, true, "Hello"s);
		refregerator(int temp, bool power, std::string word): temp(temp), power_on(power)
		{
			SaySomething(word);
		}
		void SaySomething(word) {
			cout << word << endl;
		}
};
```
The first three constructors each take in one argument for each member field, and they are all delegating the last constructor. With this feature, we still need to write many constructors, but many of them are delegating which make them easy to read and write.



