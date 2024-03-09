
Template is the C++ tool to solve generic programming. It allows programmers to write code only once, and then it works with different types of data. Although template can be more powerful than just a tool for generic, it was introduced for that purpose.

Template works differently as opposed to generics in C#. Template works at compile time, not runtime. When template class with a definite type parameter, like `vector<int>` is found by compiler, the compiler would insert the source code of `vector` class with `T` being replaced by `int` into the translation unit. Then this translation unit will be linked by the linker to become part of the program. This process is called template instantiation.

For this to be possible, the compiler must be able to see the full definition of the vector template class in the same translation unit. Meaning? Writing the template class in the header file?

With a regular function call, the compiler will settle with declaration. And we normally do declaration in header file. The implementation is in cpp file, and compiler doesn't care about this, but the linker does. However, with template function call, because compiler needs to generate the source code, it has to see the implementation. Therefore, implementation for template function are often written in header file. Some programmers put all their templates in a separate ".inc" file and include that file separately.

Template can either be a function template or a class template.
Below is an example of function template.
```
template <class T> // <typename T> works too
T Max(const T& t1, const T& t2){
	T bigT = t1 > t2 ? t1 : t2;
	return bigT;
};
```
Like generics in C#, the convention for type parameter is `T`, but technically, we can use a different one. And the `class` keyword is needed to know the following character is used to denote the type parameter, then `typename` keyword was introduced in C++98 as some think the `class` keyword is misleading. But both are commonly used, so we need to know both of them.

Below is an example of class template.
```
template <class T>
class Test{
	T data;
	Test(const T& data) : data(data) {}
};

int main()
{
	Test<string> test{"Hello"};
}
```

C++17 provides a feature called constructor template argument deduction, short for "CTAD". It means we don't need to use the angle bracket for a template, and the compiler can deduce the type based on the initial value.
`vector<int> vec{1, 2, 3};`
`vector vec{1, 2, 3}; // only possible with C++17 or later`



