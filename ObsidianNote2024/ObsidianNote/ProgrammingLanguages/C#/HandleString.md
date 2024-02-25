When we manipulate string values, there are a couple of different ways to do it.

One way is string cancatenation, which is the most basic way.
In this method, we use "" to contain seperate pieces of srting values, and use + to add them together in sequence.  And args[0] contains name in string formet, that's why it can be added together.
>Console.WriteLine("Hello " + args[0] + "!");

Second way is string interpolation.
In this method, we can use "" to contain the whole string value. However, in order to insert expression, we need to first, put a dollar sign in front of the string; second, put a curly brace around to hold the expression.
>Console.WriteLine($"Hello {args[0]}!");

Third way is still string interpolation, but with a few differences.
In this method, we no longer need to put a dollar sign in front of the string, but we can still insert expression in the middle of string value with curly braces like this.
>Console.WriteLine("Hello {0} and {1}!", args[0], args[1]);

Also, we can add formatting into the expression with : sign. In this example, N1 means number with 1 decimal place. There are all kinds of formatting keywords, but they all need a : before them.
>float number = 17.1;
>Console.WriteLine($"The result is {number:N1}.");

String is a special type in C sharp. On one hand, pressing F12 would show that string type is a class, meaning it is a reference type. On the other, it acts like a value type, as it is immutable, meaning once a string value is created, it cannot be changed.

>string name = "scott";
>name.ToUpper();
>Console.WriteLine(name);

Since, string is a reference type, we would expect name would hold the reference to the value "scott", and the ToUpper() would modify that value.
However, the result would show scott. It is not the ToUpper() method gone wrong, it just ToUpper(), and all other string related methods, would return a copy which has the value equaling to the original value with modifications. This copy is a brand new string value, and we can either store it with a variable, or we just lose it.

>name = name.ToUpper();

With the code above, we store the reference to the new string value "SCOTT", and effectively lose the reference to the original string value "scott".

