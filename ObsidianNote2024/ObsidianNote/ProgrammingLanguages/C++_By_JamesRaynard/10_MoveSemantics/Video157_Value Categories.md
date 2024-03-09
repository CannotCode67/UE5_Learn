
Value categories are introduced in C++17, but the concept is related to what we just learnt or as an improvement on what we just learn. All objects belong to one of those value categories. So, now each expression has a type plus a value category to describe its characteristics.

`lvalue`: represents persistent objects which occupy memory accessible to the programmer. They remain valid until they go out of scope or get deleted. For example, local variables, global variables, function arguments, etc. It is one of the category.

`rvalue`: are stored in locations which are not accessible to the programmer. It is a category on its own, and it also contains two sub categories: literals and temporary objects.

Literals are something like `2` or `'c'`. They have no name and cannot be referred to again. Its category is called pure `rvalue` or `prvalue`.

Temporary objects are destroyed in the same statement in which they are created. They represent objects of some classes, and their data can be moved. Its category is called `xvalue`, where the `x` stands for expiring as the object will expire at the end of the statement.

`lvalue` and `xvalue` are both objects, and their dynamic type can be different from their static type. Both of them are under a category called generalized `lvalue` or `glvalue`.![[Value_Categories_Picture.png]]