
We know when an exception is thrown, the destructors are called for all local variables in the current scope.

What happens if a destructor throws an exception?
If we have a catch block in the destructor and it handles the exception, then it should be fine.
If the exception is not handled inside the destructor, we now have a stack unwind on top of the original stack unwind. C++ implementations assume only one active exception at a time, so we have undefined behavior.

So, the rule is destructors should never throw exceptions. If it does throw, the exception must be handled inside the destructor.

If an exception is thrown inside a constructor, and unhandled, the partially-initialized object will be destroyed including all its data members. If the object is of a child class, then its parent part will be destroyed as well. As far as the program is concerned, an object does not exist until its constructor has successfully completed.

So, the rule is constructor can throw an exception, but don't handle it inside the constructor. The chance of handling the exception and continue initializing perfectly is really low. If you can do it, then sure handle the exception inside the constructor. But if you cannot, we would get a defective object which is even worse than no object at all. So, handle it outside the constructor to decide whether we should call the constructor again or what not.