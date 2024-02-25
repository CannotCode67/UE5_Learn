The arithmetic operators are the same for both C# and C++. So are the boolean operations. But we have what is known as the bitwise operators and bit shift operators in C++. Pretty crazy, huh? Normally, we don't use them, but at least we need to know what they do, so when we see them in other people's codes, we would still recognize them.
Let's explain them one by one.

// & bitwise operator and | bitwise operator
This operator is the reason why we have `&&` for boolean and operator, because `&` bitwise operator comes first. And it compares the binary digit of two numbers to form a result of integer. The same story goes for `||` bolean operator and `|` bitwise operator.
```
// 7 in binary form is 0111
// 5 in binary form is 0101
// 6 in binary form is 0110
// 4 in binary form is 0100
bool both = 5 && 6; // this returns true
int masked = 5 & 6; // this returns 4
int stacked = 5 | 6; // this reutrns 7
```
Code above, since only 0 is mapped to false, so 5 is true, and 6 is true, so `both` would have a value of true. For `masked`, we perform the and logics to each binary digit of 5 and 6, only when both 5 and 6 have 1 in the same digit, that result in that digit would get a 1. The result is 0100 in binary, which equals to the binary form of 4, so `masked` now has an integer value of 4. For `stacked`, it is the same process, but instead performing the and logics, we perform the or logics.

// >> bitshift operator and << bitshift operator
Once we understand the bitwise operator, bitshift operator is easy to understand.
```
// 5 in binary form is 0101
// 2 in binary form is 0010
int shifted = 5 >> 1; // returns 2
```
Code above, we shift the binary digit to the right by 1 digit as if we push the last digit off the cliff. The new added digit seems to be 0 by defualt. That new binary number 0010 is equals to 2 in its binary form, so we get 2 in `shifted`. We can control how many digits to shift with the number we put at the right side of the `>>`. And we can control the direction, if we want to shift to the left, we use `<<`. The sign itself is a pretty good indication of the shifting direction.

// operator overloading
In C++, it is common to overload a operator for a certain class or struct. As we will learn later, manual memory managerment requires overloading the `=` copy assignment operator. And we have already seen operator overloading in action. When we concatenate string, the `+` sign would bring string together because people who write the string class overload the `+` sign to do that. And in the iostream, the cin and cout overloads the bitshift operators to do just that as well. So how do we overload an operator? It is like writing a function. Let's see. By the way, the reference size for this is cppreference.com, in case we need to lookup the canonical signature for operators, meaning their return types and take-in parameters.
```C++
//...
int main()
{
	Person p1("tim", "blinn", 123);
	Person p2 = p1 + 7; //member function
	p1 = 4 + p2; //free function
}
```
Code above, we see we can add a person object to an integer, and we can add an integer to a person. Be aware, in order to do both, we need to overload the `+` sign twice. One is as the member function of the Person class, and the other is as free function.
We both both declaration in Person.h header file
```
class Person
{
	private:
		//...
	public:
		//...
		Person operator+(int i) const;
}
Person operator+(int i, Person const& p);
```
And we implement them in Person.cpp file
```
//...
Person Person::operator+(int i) const
{
	return Person(firstname, lastname, id+i);
}

Person operator+(int i, Person const& p)
{
	return p + i;
}
//...
```
Code above, we see we need to do declaration, but the name has to be `operator+` for `+` sign. For the implementation part, we just need to match the return type and do whatever logics that fits your ideas. Here is a good place to show a workaround of the encapsulation in C++. If we cannot implement the free overload function for `+` like the code above, we would write
```
Person operator+(int i, Person const& p)
{
	return Person{p.firstName, p.lastName, i + p.id}
}
```
But we are outside of the Person class, how can we get into its private members? Yes, we have the getters, but just pretend we don't. A way to workaround this is to use `freind` keyword in the declaration of the Person class.
In Person.h
```C++
class Person
{
	private:
		//...
	public:
		//...
		Person operator+(int i) const;
		friend Person operator+(int i, Person const& p);
}
Person operator+(int i, Person const& p);
```
Code above, basically it is saying the function after `friend` keyword can access the private members of the class. Be aware, this is not the first choice. Just to showcase this keyword, so that we know what's going on when we encounter code like this.

In cases where we want to enable comparison between objects of class we define. We have `>, <, ==, >=, <=, !=` six different operators to overload. The C++ 20 has a feature to do that for us. 
```C++
class Person
{
	private:
		//...
	public:
		//...
		Person operator+(int i) const;
		friend Person operator+(int i, Person const& p);
		auto operator<=>(Person const& p) const {return id <=> p.id};
}
Person operator+(int i, Person const& p);
```
Code above, the `<=>` is called space ship operator. It denotes all those six operators in one. And we are using the id as our comparison crtieria. We can actually give no comparison critieria like this.
```C++
class Person
{
	private:
		//...
	public:
		//...
		Person operator+(int i) const;
		friend Person operator+(int i, Person const& p);
		auto operator<=>(Person const& p) const = default;
}
Person operator+(int i, Person const& p);
```
Code above, we tell the complier this overload implementation is the default. What it means is, the comparison critieria is comparing each member variable in the order of their declaration. Sometimes, this doesn't work because the first member variable can be a type we define, and we haven't define any comparison behaviour for that type. But just as you know, there is such an option here.
