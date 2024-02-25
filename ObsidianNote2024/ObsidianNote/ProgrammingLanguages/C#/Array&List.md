Array and list are two very common data structures across many programming languages. However, just like other aspects in this field, different languages tend to come up with their own definitions of array and list, leading to different implementations and behaviours.

In C#, array is referring to the fixed array. It needs a size when created, and has no auto-expand functionality when there are too many elements. And array is a class, so when an array object is created, we need to use the keyword new.

>var numbers = new double[3];
>numbers[0] = 12.7;
>numbers[1] = 17.1;
>numbers[2] = 15.9;

If we know beforehand what the elements are, we can populate the array with a simplier syntax using curly braces. We can omit the array size and array type as well, as the complier would assign those according to the element information.

>var numbers = new[] {12.7, 17.1, 15.9};

In C#, list is referring to the dynamic array. It is enumerable through index like array, and it can auto expand when there are too many elements. List is also a class, a generic class actually, so we need to not just use the keyword new but also use <>  to hold its type when creating a list object.

>var numbers = new List<double>() {12.7, 17.1, 15.9};

It is very similar to array, so it can use the same populating syntax. This would also give the list a size of 3 under the hood, when the fourth element is added, the list object would auto expand the size by doubling the original size.
Also, it has many built-in methods and properties, making it a much more convenient data structure to work with than array.

