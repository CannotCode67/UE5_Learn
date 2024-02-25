Enum is also one of those things that you can feel C# is a more modern language than C++. In C++, we have enum first, later then scoped enum is added. Scoped enum is also known as enum class. We look at both.
They can be inside the same file, and that file is a header file, not a cpp file.
```
enum Status
{
	Pending,
	Approved,
	Cancelled
};

enum class FileError
{
	NotFound;
	Found;
}
```
You may wonder what is the difference except we need to type class for scoped enum. The real difference comes when we use them. 
```
#include "Status.h"

int main()
{
	Status s = Pending;
	s = Approved;
	FileError fe = FileError::NotFound;
	fe = FileError::Found;
}
```
For enum in C++, the value is one of its define values. For enum class, the value contains the namespace. Enum class is very informative. As we can have NotFound for FileError or one for NetworkError, and with Enum class we can tell which is which very easily. And that's how C# choose to do enum, although C# uses a `.` sign instead of `::` sign. C# doesn't have the enum in C++, its enum is equivalent to the enum class in C++.