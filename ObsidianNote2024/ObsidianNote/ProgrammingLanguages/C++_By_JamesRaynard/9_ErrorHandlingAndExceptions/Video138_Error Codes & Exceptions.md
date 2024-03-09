
In C, the only practical way to communicate an error condition is with an error code. Error code is an example of the approach where we take care of the runtime error at the place it occurs. When we write function, we put extra code that returns a coded number corresponding to a specific kind of error. The caller of the function needs to check the return value to make sure if there is any returned error code. If so, then the caller can decide to handle the error itself or do whatever it needs to meet the requirement.
```
int get_data() {
	...
	if(!file)
		return ERR_FILE_NOT_FOUND;
	...
}

int main() {
	...
	int retval = get_data();
	switch(retval) {
		...
		case ERR_FILE_NOT_FOUND:
			... // do something about the error
		...
	}
}
```
We have to check for error codes whenever we expect to encounter any. If a function can return many different error codes, then we have to check all of them. Also, if this function is passed around as function pointer, then getting the error codes is more complicated. Also C++ constructors return nothing, not even error code.

Therefore, C++ does not suggests us to use error code. Instead, the language provides a tool called exception. The code which can cause a runtime error is put inside its own scope, so we wrap it with `{}`. If an error does occur, an exception object is created, and the runtime will find a handler for this exception depending on the exception's type. If the runtime finds one, then it moves the execution to the handling code. If not, the program will terminate. 

This is a tool of the approach where we take care the error in one place. Some programmers are responsible to write all kinds of exception classes, and provide handling codes for all of them. Other programmers who write the regular functionality code can specify when to create an exception object of some kind. But the runtime would not try to find a handler for this exception object, unless we use the try block to wrap the code around. When compiler sees this try block, it generates some code for the runtime to execute. Only then the runtime is responsible to pass this exception object around to find the handling code if error occurs.

So, we write all the exception definitions and handler codes in one place. Conceptually, we separate the error related code from our regular functionality code. Also, we can define our own exception class to carry whatever data we need to handle the error instead of just an integer. Constructors can throw exceptions, and callbacks also work well with exceptions because no matter where the code is, the runtime will help to find a handler for it.

The drawback of exceptions is obvious. The runtime now has to generate an exception object, and find a handler for an exception object, which averagely takes a very long time. That's why many companies' coding standards forbid exceptions. Even though it is allowed, we should use it carefully for it can impact the performance greatly.