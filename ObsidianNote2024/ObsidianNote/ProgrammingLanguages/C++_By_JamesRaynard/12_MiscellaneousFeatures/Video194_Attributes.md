
There are various implementation-dependent compiler directives, such as `#pragma`, `__attribute`, `__declspec`, etc. They are used to give extra information which compiler behaves differently based on. They are compiler specific, so not part of the C++ language.

C++11 introduced attributes to provide a standard syntax for compiler directives, at the same time defined a few attributes which can be used in the language. This video is about the attributes which can be used in the language.

An attribute goes inside a pair of double square brackets, and it is normally put before the entity it applies to.
`[[ someAttribute ]] someEntity;`

Here we have the `noreturn`  attribute describing the function `func()` will not return any value, so execution will never exit `func()`'s scope.
`[[ noreturn ]] void func();`

This `always_inline` attribute will make a namespace inline.
`[[ somenamespace::always_inline]]`

This `deprecated` attribute will cause a compiler warning if the describing entity is used.
`[[ deprecated ]] void func();`
It can also take a string argument to be use as part of the warning.
`[[ deprecated ("will be removed in C++17") ]] void func();`

The `nodiscard` attribute will cause a compiler warning if the return value isn't stored.
```
[[ nodiscard ]] int func() {return 99;}

int main() {
	func(); // cause a compiler warning
}
```

When attributes apply to struct, class and enumeration, we need to put the attribute before the name.
`struct [[ nodiscard ]] Test {}`

```
Test func() {
	return Test(); // Test struct is marked as nodiscard
}

int main() {
	func(); // cause a compiler warning
}
```
`func()` return something marked as `nodiscard`, then even though `func()` itself is not marked, we still get a compiler warning if we don't store its return value.

Sometimes, when we declare a variable but end up not using it, the compiler will give us a warning. The `maybe_unused` attribute can turn that warning off.
```
int main() {
	[[ maybe_unused ]] int x;
	int y = 3;
	y = y + 10;
}
```