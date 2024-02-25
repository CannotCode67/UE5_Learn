The if statement, for loop, while loop are exactly the same as the one in C#. We discuss the ones that are not the same. 

// switch statement
The switch statement in C++ has the same semantics, but it appears to be only capable of handling integer value or enum value in C++, not enum class value be careful. char can also be handled as char can hold integer value, but again, we are not supposed to put an integer into a char variable.

// immediate if
This immediate if statement in C++ is just what we know as ternary statement.
`result = something? 7 : 302;`

// ranged for loop
The ranged for loop is what we call foreach statement in C#, but with a slightly different semantics.
```
for (auto num: numbers)
{
	//...
}
```
Code above, is the ranged for loop. Very similar to foreach loop in C#.
```
foreach(var num in numbers)
{
	//...
}
```
In both examples, `numbers` has to be a collection, but we discuss how a collection is qualified to be iterated through using foreach statement in C#. Just not sure that's how it works under the hood for C++.
