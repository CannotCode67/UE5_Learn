
When we have a base class pointer/reference pointing to a derived class object, and we have any base class member function virtual, then generally we should have a virtual destructor.

If the destructor is not virtual, static binding will be used. And as a result, when we call `delete` on the pointer, we invoke the base class destructor but not the destructor for derived class. So the derived part of the object is not destroyed. If the derive part of the object manages some kinds of resource, we would have a resource leak.

We know that compiler would synthesize a destructor for us if we don't provide one. And that default destructor is not virtual. We have to manually write one if we want a virtual destructor.
```
virtual ~Shape(){}
virtual ~Shape = default;
```