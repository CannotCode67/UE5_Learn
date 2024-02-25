
When we write a for loop, it is intuitive to declare and initialize the loop counter `i` inside the for loop statement, because that's what we are taught. However, this feature only came in with C++98. And for iterator, it was C++17. After C++17, we can declare and initialize our iterator inside the for loop statement.
```
for (auto it = begin(str); it != end(str); ++it) // this is allowed after C++17
{
	...
}
```

About switch statement...

C++17 provides a `[[fallthrough]]` attribute for switch statement. This attribute is to be used for shutting down compiler warning. When using switch statement, if we don't `break;` for cases, execution would fall through, and compiler would give us a warning. If this is intentional behavior, then we can use `[[fallthrogh]]` attribute to tell the compiler it is intentional.