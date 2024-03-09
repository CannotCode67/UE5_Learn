
We know namespace is mainly used to avoid name conflicts. We can create a namespace inside another namespace, so they are nested.
```
namespace A {
	namespace B {
		int x;
	}
	B::x;
}

int main() {
	A::B::x = 5;
}
```

We can inline a nested namespace, so that we can access the defined symbols as if there is no such a namespace.
```
namespace A {
	inline namespace B {
		int x;
	}
	x;
}

int main() {
	A::x = 5;
}
```

You may wonder why would anyone want to use something like that. In fact, the standard library uses inline namespaces as well.
```
namespace std {
	...
	inline namespace literals {
		inline namespace string_literals {//"s" suffix for string}
		inline namespace chrono_literals {//chrono suffix for duration}
	}
	...
}
```
We can bring in the whole standard library by `using namespace std;` because the sub namespaces are inline. We can only bring in the literals by `using namespace std::literals;` because both `string_literals` and `chrono_literals` namespaces are inline.

Another useful case for inline namespace is working with versioning.
```
namespace product {
	inline namespace version1 {
		class refrigerator {...}
	}
	namespace version2 {
		class refrigerator {...}
	}
}
```
In the main() `cpp` file.
```
int main() {
	product::refrigerator fridge(); // one defined in version1 namespace
}
```
We write code to use the `refrigerator` class under the `product` namespace in the main file. But we can change this `refrigerator` class from the one defined under `version1` namespace to the one defined under `version2` namespace simply by inline `version2` namespace instead of `version1`. The beauty of this is we don't need to change the code in the main file.