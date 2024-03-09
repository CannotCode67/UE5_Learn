
Professional programs need to be able to handle error to some extent because error in reality is inevitable. Error here means the data in a wrong format, or connection is down, or anything our program does not expect, not bugs we produce.

A naive approach would be to handle error at place where it happens, like ask the user to input again if the previous input is not qualified. But, doing it this way would mix up the error handling code with our actual functionality code together. And as a program gets complicated, this approach would have error handling spread all over the places.

A better approach would be to have error handling codes written at one place, and the problem now becomes how to pass the error from the place where it occurs to the place handing it.

Following study of this chapter is tools and ways to achieve this better approach.