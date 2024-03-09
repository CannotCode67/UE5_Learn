
Templates were introduced to support generic programming. When we write a class or function template, we normally will get the same behavior for every type that we instantiate it with.

But sometimes, we may wish to have different behavior for certain types. Maybe it is because we simply want to handle certain types differently, or the code doesn't behave correctly for some types, or we want to optimize the code for some types.

To do all that, we can use template specialization. Let's see an example.
```
// generic vector class
template <typename T>
class Vector {
	...
}
// specialization of vector class for bool type
template<>
class Vector<bool> {
	...
}
```
Usually, the specialization follows immediately after the generic template. If the specialization comes before the generic template, it will not compile. If there is no generic template but only specialization, it will not compile. If the specialization is not visible to code that uses it, the compiler will instantiate the generic template.

When a template is instantiated, the compiler has to choose which version to use. It will always choose the most specific one out of all the choices that match. So, if we have `Vector<bool> vec;`, the compiler is presented with two choices, `Vector<bool>` the specialization and `Vector<T>` with `T == bool`. The compiler will choose the specialization as it is more specific.

We can use `identify()` member function to obtain the class of an object. If it is instantiated with the generic template, we get `Vector<T>`. If it is instantiated with the bool specialization, we get `Vector<bool>`.

Next, we study the partial specialization, meaning the template parameter is used to indicate not a single type, but as a group of types. Partial specialization is half way between generic template and template specialization. The example below is one closer to template specialization.
```
template <typename T>
void Reverse(T& container) {
	std::reverse(begin(container), end(container));
}
template <typename Elemtype>
void Reverse(std::list<Elemtype>& container) {
	container.reverse();
}
```
Here, we have the specialization for `list` of all the types.

Now, we have an example of partial specialization closer to generic template.
```
template <typename T>
class Vector {
	...
}

template <typename T>
class Vector<T*> {
	...
}

Vector<int> vec; // generic Vector class
Vector<int*> vec; // partial specialization of Vector class
```

