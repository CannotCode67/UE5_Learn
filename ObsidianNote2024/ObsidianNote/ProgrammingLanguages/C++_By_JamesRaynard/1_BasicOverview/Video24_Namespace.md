
A namespace is used to group together a bunch of logically related symbols. By doing so, we are able to avoid name conflicts. Back in C, we don't have namespace, and the only way to avoid name conflicts is to write the name like this: `Ben_Test`, `Jack_Test`, etc.

To define a namespace
```
namespace abc{
	class Test;
}
```
Everything inside the namespace bracket, will be automatically prefixed by the compiler. So outside the namespace, when we want to use `Test`, we do `abc::Test firstTest`. We should have plenty of experiences with the standard library, `std::string`, or `std::vector`, etc.

In fact, there is a global namespace which has no name.
```
class Test{};
int main()
{
	::Test test; // here we provide nothing before the :: scope operator
}
```
Of course, we don't need to explicitly write out the `::` (pronounced colon colon) here. But what if we bring in some namespace which has a class also called `Test`, then in that case, we have to put `::` to specify we want to use the global namespace version of class `Test`.

We can have `namespace abc{}` in multiple places or even files, and the compiler would merge all the code within under the same namespace. So we don't have to write all the code in one place and jam them into one namespace bracket.

Once we bring in some namespace, if there is any name conflict, without specifying the prefix, we always get the ones defined in this brought in namespace. This is called name hiding. We use class as example, because it is obvious. But variables can be defined in namespace as well, and they are easy to overlook. Again, to access the hidden names, we need to add the namespace prefix and scope operator before the name.

How to bring in namespace? We have a `using` declaration that comes in two forms.
```
using abc::Test; // this will allow us to use Test to refer abc::Test
using namespace abc; // this will allow us to use everything defined in abc namesapce without prefix
```
The first one is suggested, as we know for sure what we bring in just by looking at the declaration. The second one is bad, because we don't know what we bring in unless we check what are defined in that whole namespace.