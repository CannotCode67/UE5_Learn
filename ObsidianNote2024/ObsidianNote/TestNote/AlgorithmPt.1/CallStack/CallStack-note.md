Call stack is what the computer uses behind the scene to handle function calls. 

Although different programming languages or hardware might implement call stack differently, the general idea remains the same. 

When a function is called, the computer creates a stack frame, which is an element of the call stack. This stack frame contains all the relevant variables and arguments to that specific function call. When the function returns/terminates, the stack frame is popped from the call stack. 

Now, how does the call stack work when functions are called within functions? 

Function within function example
![[AlgorithmPt.1/CallStack/GetImage.png]]
-funcOne stack frame is created first, then funcTwo, then funcThree. Now, funcThree returns 1, so funcThree got popped and passed the return value to funcTwo. funcTwo got popped and passed the value to funcOne. funcOne finally got popped as well. 

-With each additional function call, a new stack frame is created and pushed to the call stack. A stack frame is popped only when its specific function returns/terminates.


Recursive function example![[AlgorithmPt.1/CallStack/GetImage (1).png]]
![[AlgorithmPt.1/CallStack/GetImage (2).png]]
-it is similar to the last example, but recursion makes it more difficult to know how many stack frames are created. 

Too many stack frames are created might cause computer memory to run out, and that is called stack overflow. Stack overflow is prone to happen when using recursion.
![[AlgorithmPt.1/CallStack/GetImage (3).png]]
