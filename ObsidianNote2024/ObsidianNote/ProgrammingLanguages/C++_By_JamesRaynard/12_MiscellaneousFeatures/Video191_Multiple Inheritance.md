

Multiple inheritances refers to a derived class has more than one parent class. This is controversial as many programmers think the extra flexibility is much outweighed by the extra complexity. As a result, many programming languages don't support this feature, but C++ does.

The syntax is straightforward.
```
class HardwareDevice;
class TouchResponder;

class Mouse: public HardwareDevice, TouchResponder {...}
```
The order matters a bit. As a `Mouse` object will be store in memory as a `HardwareDevice` object, followed by a `TouchResponder` object, followed by `Mouse` specific part. When we construct a `Mouse` object, the `HardwareDevice` constructor gets called, then`TouchResponder` constructor gets called, followed by `Mouse` constructor gets called. When we destroy a `Mouse` object, destructors get called in the reverse order.

If both parent class defines a member function with the same name, then even though the function signatures are different, we cannot invoke either of them through a derived class object.
```
bool HardwareDevice::initialize(Params&);
void TouchResponder::initialize();

Mouse mouse;
mouse.initialize(); // will not compile
```
The reason is the compiler doesn't know which one to call even though we expect it can find out through the function signature.

If we do need to call them, we need to write a derived class member function to wrap around them.
```
void Mouse::initialize() {
	TouchResponder::Initialize();
}
bool Mouse::initialize(Params&) {
	return HardwareDevice::initialize(Params&);
}
```