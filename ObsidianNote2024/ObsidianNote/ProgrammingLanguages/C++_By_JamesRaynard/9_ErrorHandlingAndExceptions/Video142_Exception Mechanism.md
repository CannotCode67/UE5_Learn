

When an exception is created and thrown, the runtime allocates a memory space and copy the thrown exception object into it. This memory space is accessible by any catch block which can handle this kind of exception. 

Next, in the scope where the exception is created and thrown, all local variables including the exception object will be destroyed. Now, only that special memory space has the copy of the thrown exception object.

After destroying all local variables, the program leaves this scope without execute any further instructions that are after the throwing of exception.

After exiting the scope, the execution now comes to the scope a level higher, and the runtime starts to look for a catch block to handle the exception. If it fails to find one, it will destroy all local variables in that higher scope, and exit that higher scope.

This process of repeatedly destroying local variables and exiting the current scope is known as stack unwinding.

This continues until it finds a suitable handler or it reaches main() without finding one.

If the runtime reaches main() without finding a suitable handler, the program terminates.

If the runtime finds a suitable handler, the program executes the handling code in the catch block, and continues execution starting at the next instruction after the catch block. So the execution will not go back to the place where the exception is thrown. All variables are destroyed, so there is no point of going back. If we recreate all the variables is like rerun that piece of code which is about to throw an exception, so no point of doing that either.

In the catch block, we can rethrow the exception by writing `throw` without anything followed. If we choose to do that, the program sees this as a new exception, and the stack unwinding begins again from the current scope.
```
catch (const std::exception& ex) {
	...
	throw; // rethrow the exception
}
```
You may wonder, why to rethrow an exception? If the catch block cannot handle it, then why catch it in the first place? 

Well, sometimes, an exception is throw and the real handler is many scopes above. And we have info we need to log right away, otherwise when the runtime finds the real handler, the info will be different and lost to us. We would have a catch block to log the info, then rethrow the exception to let the runtime find the real handler. 

Or maybe an exception happens and we need to manage the resource such as closing the network connection as soon as possible. We have a catch block to close the network socket, then rethrow the exception. There will be a handler higher up to decide whether we should try to connect again or give up or what not.



