In C++, when we create a class, we have two files, the header file, and the cpp file. If this class A has a member of another class B, then we would need to write  `#include "classB.h"` in both the header file and cpp file of class A, right? There is a way to save the class A's header file from having the line `#include "classB"`. That's forward declaration.

Inside classA.h header file
```
#pragma once
class classB
class classA
{
	// declaration...
}
```
Code above shows the syntax of forward declaration, it is simply declaring the classB as a class before the declaration of classA.

But, there is no escape of `#include "classB.h"` inside classA.cpp file, we have to write that line. That's okay, but the question is why do we need this forward declaratin? Aren't we have the `#pragma once` already? And that's taken care of the multiple `#include`, right? Yes, `#pragma once` solves the problem of multiple `#include` of the same translation unit. And if we use `#include` in the classA.h instead of forward declaration, then we have to have `#pragma once` in classB.h because both classA.h and classA.cpp write the `#include classB.h` line. Now, if some classC needs to include classA.h, it would have classB.h included as well. Yes, there is pragma once to prevent classB.h from being included multiple times, but the problem is classC doesn't need classB in anyway. Meaning, classC only needs the parts of classA that are not related to classB. Forward declaration can help classC to not have any codes of classB included since we don't have the include statement in classA.h. That provides invisible benefits like compiler optimization, file size, performance speed, etc. because there are less unrelated codes. However, think of it this way, if classB eventually needs to be used, so it is included once somewhere and linked together into the last single execution file. The benefits are gone. We rarely write classes that will not be used in our program, but forward declaration would be a good pracice when dealing with huge utility class from library for example.