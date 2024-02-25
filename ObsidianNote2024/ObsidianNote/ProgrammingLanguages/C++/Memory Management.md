Memory management is another very scaring point of C++. Two keywords were added since the beginning of this language to deal with memory management: the `new` keyword and the  `delete` keyword. When we use `new` keyword to create something, that thing lives in heap or known as free store, and we get a pointer pointing to that object. When the pointer goes out of scope, it is automatically destroyed, but that heap living object it refers to, will not, and we get a memory leak. The destructor of that heap living object need to be invoked through the keyword `delete`. 
Let's see an example.
```C++
#include "Resource.h"

int main()
{
	{
		Resource localResource("local");
	}
	Resource* pResource = new Resource("created with new");
	delete pResource;

	return 0;
}
```
Code above, shows two ways to create Resource object. The first one is what we do normally, and that object lives in stack. When execution hits the first closing bracket, its destructor gets invoked and the localResource object gets deleted. However, for the object we create with the keyword `new`, we have to use `delete` keyword to destroy it, otherwise it lives in the heap even after the program finishes. Remember we don't need to dereference the pointer with `delete` keyword, but we do if we modify the object. That inconsistency is one of the factors making C++ difficult to learn.

Now, we see a slightly more complex example.
```C++
#include "Resource.h"

int main()
{
	{
		Resource localResource("local");
	}
	Resource* pResource = new Resource("created with new");
	Resource* p2 = pResource;
	delete p2;
	pResource -> GetNote();
	delete pResource;

	return 0;
}
```
Code above would explode. We assign another Resource pointer `p2` to equal to the `pResource`, now they are pointing to the same object. And if we use `delete` with `p2`, we are deleting the object. When we try to invoke the object's method through `pResource`, we get an exception. And if we don't invoke the member function, we delete the `pResource` , we still blow up, since the object is already gone, and we cannot delete it twice. Therefore, copying a pointer is a very dangerous move. Very much like modifying a public field member of a class from multiple places, and later when we use that field for something, we find ourselves lose the track of the modification of the field, and thus don't know the value of it. But pointer is worse, because it makes our program explode.

Now, here it is another way to die.
```C++
#include "Resource.h"

int main()
{
	{
		Resource localResource("local");
	}
	Resource* pResource = new Resource("created with new");
	int j = 3;
	if (j == 3)
	{
		return 0;
	}
	delete pResource;
	return 0;
}
```
Code above simulates a situation where some logics or maybe exception causes the execution to skip the `delete` keyword, thus the heap-living object never gets deleted.

You may think this is actually not too bad. But if you are not working alone, or this is a piece of software that are meant to be used by others, we have to do things in a defensive way. We have to write code to explicitly deal with the situations where our pointer might get copied, either it is through a constructor or it is through a copy assignment operator. Also, watch out for multiple invocations for methods in which objects are created with the keyword `new`. We first learn the old way, and then move on to some tools that change the way we manually handle memory management.
Normally, when pointer is involved, it is a member field of a class. Here it is our example.
main file
```C++
//...
int main()
{
	Person Blinn("Tim", "Blinn", 123);
	Blinn.AddResource();
}
```
Person.h header file
```C++
#include "Resource.h"

class Person
{
	private:
		//...
		Resource* pResource;
	public:
		Person(std::string first, std::string last, int id);
		~Person(void);
		void AddResource();
		//...
}
```
Person.cpp implementation file
```C++
#include "Person.h"

Person::Person(string frist, string last, int id)
	:firstName(first), lastName(last), idNumber(id), pResource(nullptr)
{}

Person::~Person(void)
{
	delete pResource;
}

void Person::AddResource()
{
	pResource = new Resource(GetName() + " resource");
}
//...
```
Code above, when a class has a pointer member field, we initializes it to null pointer in the constructor. In the destuctor, we `delete` the pointer. Now, when main function ends, the class object goes out of scope, it gets destroyed automatically since it is a stack living object. Meanwhile, its destructor cleans up the pointer and its Resource memory. We explicitly invoke AddResource() to create the Resource object, but if we don't, then the Person class's destuctor would be deleting a null pointer, which is fine because deleting a null ponter would do nothing, so no need to check if it is null or not.
Moving on.
```C++
//...
int main()
{
	Person Blinn("Tim", "Blinn", 123);
	Blinn.AddResource();
	Blinn.AddResource();
}
```
Code above, we call AddResource() twice, and in the second time we do that, we create another Resource object and assign its memory address to the pointer pResource. What we end up is losing the pointer to the first Resource object, a memory leak. There is nothing to prevent someone from calling your function multiple times, so we need to implement some logics inside AddResource() to fix this.
```C++
void Person::AddResource()
{
	delete pResource;
	pResource = new Resource(GetName() + " resource");
}
```
Code above it is a quick fix, for situation where there can be only one Resource object for every Person object.
Here it is another change in main file.
```C++
//...
int main()
{
	Person Blinn("Tim", "Blinn", 123);
	Blinn.AddResource();
	Blinn.AddResource();
	Person Phong = Blinn;
}
```
Code above, when we construct Phone this way, by default, compiler would just copy all the values from Blinn object, and use them as the values for Phong object. So, we end up a Phong object with a full name called Tim Blinn, funny, isn't it? Not so funny however, the pointer also gets copied, and we now have two pointers as the member field of two Person objects, but they are pointing to the same Resource object. When main ends, Phong gets deleted first, and cleans up the Resource object, then Blinn gets deleted and try to delete the Resource object again. We blow up. To prevent this, we need to overwrite what is known as the copy constructor for Person class.
Person.h header file
```C++
#include "Resource.h"

class Person
{
	private:
		//...
		Resource* pResource;
	public:
		Person(std::string first, std::string last, int id);
		Person(Person const& p);
		~Person(void);
		void AddResource();
		//...
}
```
The copy construtor has the same name as the class, because it is a constructor, and it always takes in a constant reference of an object of the same class.
Person.cpp file
```C++
Person::Person(Person const& p)
{
	firstName = p.firstName;
	lastName = p.lastName;
	idNumber = p.idNumber;
	pResource = new Resource(GetName() + " resource");
}
```
Code above, now we explicitly create a new Resource object when the copy constructor is invoked. So now phong object would have its own Resource object different from the one belongs to Blinn object.
Moving on to the copy assignment oeprator
```C++
//...
int main()
{
	Person Blinn("Tim", "Blinn", 123);
	Blinn.AddResource();
	Blinn.AddResource();
	Person Phong = Blinn;
	Blinn = Phong;
}
```
Code above, we copy the Phong object's values into the Blinn object's values. Because we are not constructing, the copy constructor will not be invoked. The possibility to use a copy assignment between them is due to the fact they are both Person objects. But we need to overload the copy assignmnet operator to deal with this kind of interaction between Person objects, for they now have pointers pointing to the same Resource object again, and Blinn's original Resource object has become a memory leak.
Person.h header file
```C++
#include "Resource.h"

class Person
{
	private:
		//...
		Resource* pResource;
	public:
		Person(std::string first, std::string last, int id);
		Person(Person const& p);
		~Person(void);
		Person& operator=(const Person& p);
		void AddResource();
		//...
}
```
Person.cpp file.
```C++
//...
Person& Person::operator=(const Person& p)
{
	delete pResource;
	pResource = new Resource(p.pResource->GetName());
	return *this;
}
//...
```
Code above, it might be a little confusing. In our example main, the logics goes into this direction: the Phong object is the parameter, and the Blinn object is the one calling this overload operator. So Blinn object already has a Resource object when we say `Blinn = Phong`. The first thing is to cleanup that Resource object and give Blinn's pointer a new one which has the same name as the Phong Resource object does. Both Resource objects have the same name, but they are two different Resource objects. This is the example way to implement it, you may consider a different approach to suit your needs. But the bottom line is we safely deal with it and now memory is safe when we use copy assignment operator.

Those three pieces of defensive programming is known as the Rule of Three, when we are involed with the heap living objects with `new` keyword.

// move semantics (Need to learn the sematics because the example doesn't show!!!)
Move semantics was introduced in C++11 to make codes faster. It brings in benefits in situations where we need to copy heap living objects around. For manipulation of heap living objects, we would have pointers pointing to them. And when we make a copy of a heap living object, and we don't keep the source one, so we just need the copy for now on. In that case, we can tell the compiler that the new pointer is pointing to the same heap living object, and we null the old pointer, in case it deletes the heap living object. This is move semanics, instead of copying the whole heap living object and have the new pointer to point at that. It would be possible to use move semantics only when we don't need the source heap living object when we make a copy. But why do we bring this up and not show any detaills of the move semantics? It is because, in order to actually have move semantics we have to overload the move constructor and the move assignment operator. That upgrades our Rule of Three into Rule of Five, lovely isn't it?

// smart pointers
C+11 not just brought in move semantics, it also introduced smart pointers. These are the tools we mentioned to change the way we manage our memory. They are not really new things, but do make a difference. Under the hood, they are just classes wrapping around the pointer and implement logics inside their methods to enable or disable certain functionalities of a raw pointer.
The first one and most commonly used one is unique_ptr. It cannot be copied, but can be changed to point at a different memory address. The second one is shared_ptr. This one can be copied, and has a reference count mechanism, counting how many copies out there. When it is copied, it is like a raw pointer, both the copy and the source would point to the same memory address. However, that memory will only be cleaned up when the count of copies drops to zero. There is also a weak_ptr, allowing you to look at the content of a shared_ptr, but not get involved into the counting mechanism. Again, we need more information for them to actually use in production.
Here it is a shared_ptr in action.
Person.h
```C++
#include "Resource.h"

class Person
{
	private:
		//...
		std::shared_ptr<Resource> pResource;
	public:
		Person(std::string first, std::string last, int id);
		~Person(void);
		void AddResource();
		std::string GetResourceName() const {return pResource ? pResource->GetName() : "";}
		//...
}
```
We have an inline function GetResourceName(), and the semantic to work with shared_ptr is just like working with a raw pointer. The implementation is a ternary statement, checking if pResource is null, if true, maning it is not null, then we get the name, otherwise, we output nothing in string.
Person.cpp file
```C++
//...
Person::Person(string first, string last, int id)
	:firstName(first), lastName(last), idNumber(id)
{}
//...
void Person::AddResource()
{
	pResource.reset();
	pResource = std::make_shared<Resource>(GetName() + " resource");
}
//...
```
Code above, we can see the cleanup is wrapped inside the shared_ptr class's reset() function. And for construction, we now need to invoke the custom method `make_shared<T>()` instead. And we no longer need to initialize the pointer to null in the constructor, the shared_ptr class's default constructor does that for us.
Into the main file
```C++
//...
int main()
{
	Person Blinn("Tim", "Blinn", 123);
	Blinn.AddResource();
	Blinn.AddResource();
	Person Phong = Blinn;
	Blinn = Phong;
}
```
Now, when we say `Person Phong = Blinn`, and `Blinn = Phong`, each Person object has its own pointer, but pointing to the same Resource object. The reason we don't explode when main ends is the shared_ptr class's counting mechanism to make sure the memory gets cleaned up exactlly once.




// C# garbage collector
C# is a language with garbage collector which would clean up heap-living objects when there is no variable referencing them. In C++, we don't have that.