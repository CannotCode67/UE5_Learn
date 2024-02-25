If no accessor is given, the class member is private by default. And struct is just like class, but the default accessor is public. Therefore, struct is used often as a group of data.

Member function is implemented as global function, meaning when we invoke a member function through a class object, the whole picture is we are invoking a global function and passing the class object's memory address as the first argument.
```
Test test;
test.func(1, 2.0f, "three"); // is called as Test::func(&test, 1, 2.0f, "three");
```

In member function, we have the keyword `this` as the pointer to the object. We can dereference `this` to access the member of the object. And every time we access the member of the object inside its member function, we are doing exactly that. But we just get used to the semantics that we don't need to dereference `this` every time because the compiler can deduce we are accessing the object's members.
```
func()
{
	i = 1;  // compiler would turn it into this->i = 1, so that we don't need to dereference this everytime we access a member.
}
```