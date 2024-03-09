
One problem that can arise when we have multiple inheritance is both the parent classes have the same base class.

Let's see an example.
```
class SalesEmployee: public Employee {...};
class Manager: public Employee {...};
class SalesManager: public SalesEmployee, public Manager {...};
```
Recall the memory layout we study in the last video. When we create a `SalesManager` object, we will have a `SalesEmployee` object followed by a `Manager` object followed by `SalesManager` specific part. However, both parent classes inherit from `Employee`, so in the memory layout of a `SalesManager` object, we have two `Employee` objects within.

Is there any problem with that besides it is conceptually wrong?

Well, to make a `SalesManager` object only have one `Employee` object within, we use something call virtual inheritance. Both the parent classes virtual inherit from `Employee`.
```
class SalesEmployee: public virtual Employee {...};
class Manager: public virtual Employee {...};
class SalesManager: public SalesEmployee, public Manager {...};
```
We don't need to use virtual on `SalesManager` class, but now in the memory layout, a `SalesManager` object will have one `Employee` object within as the `SalesEmployee` object and the `Manager` object are sharing the same `Employee` object.