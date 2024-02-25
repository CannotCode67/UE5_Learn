We have learnt that there are reference type and value type, and C# handles them differently. 

If we assign the value type variable to a reference type variable, what would happen? Two things happen: first the value type variable gets copied as this is how value type gets handled; secondly an object created on the heap to store that copy is assigned to the obj variable as reference value. When an object is created to store an value type variable, this process is called boxing. 

Boxing helps C# to deal with this kind of scenario, or you may say boxing is the way C# choses to handle this kind of scenario. 
Let's say similar thing happens and there is no boxing, then how a reference variable can store a reference value pointing to the memory address where a value type variable is located. C# has to find a way to return a reference value point to stack where value type variables live, or C# must find a way to allow value type variable to directly live in heap which is a memory place for only objects.

Above two ways are very low-level and complicated routes to go about. So instead, C# developers decide to use boxing the deal with this. Creating an object to store a value type variable almost defeats the purpose of defining these two types in the first place, but maybe just so the action of assigning a value type variable to a reference type variable. Boxing is slow, in fact almost twenty times slower than a normal assignment, because it is an easy way out, not sophistcated engineered, but this also presents a message from Microsoft that we as the programmer should prevent this kind of scenario from happening as much as possible.

>int num = 23;
>Object obj = num; // boxing
>int i = (int)obj; // using cast to perform unboxing

Unboxing is also slow, but if we can prevent boxing in the first place, then there will not be any unboxing happening.