When a float value is put into a int variable, we know that the fractional part of the value gets thrown away, and we call this trucation. Same thing can happen to objects in stack and we call it slicing.

When a derived class object in stack is copied and assigned into a base class variable, we essentially get an derived class object with only the base class portion remain. This can also happen through function as we know, by default, functions in C++ pass parameters by value.

```C++
std::string Say(Person p)
{
	return p.GetName();
}

int main()
{
	Person p{"Someone", "Else", 567};
	Tweeter t("Local", "Tweeter", 444, "@local");

	p = t;
	//t = p; // this cannot even build.

	string s = Say(p);
	s = Say(t);
	
	return 0;
}
```
Code above, when we say `p = t`, we are copying t and assigning the copy into a variable of class Person. Even though the GetName() is virtual,  when `s = Say(p)`, we get a string value of "LocalTweeter", because the Tweeter specific portion is discarded when t is put into p. And when `s = Say(t)`, we get a string value of "LocalTweeter" again, since the Say() function passes parameters by value, which is doing the same thing, throwing the Tweeter specific portion away.

If this behaviour is not what you want, then to avoid this, we can use reference or pointer.
```C++
std::string Say(Person const& p)
{
	return p.GetName();
}
```
Now, we actually act as if we are invoking the GetName() through a base class reference. And since the GetName() is virtual, so with `s = Say(t)`, we get a string value of "LocalTweeter@local". I guess with reference and pointer, the object remains the same, and thus the Tweeter part is intact. But when `s = Say(p)`, we still get a string value of "LocalTweeter", that's because when t is put into p, the slicing already happens.
