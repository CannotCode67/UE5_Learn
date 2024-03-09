
We have seen `swap()` can be fast when it exchanges internal data instead of copying them. The concept is the same for move semantics, but move semantics are more concerned with passing arguments to functions and returning values from functions.

C++ by default uses value semantics, so there is a lot of data copying. One of the benefits of doing so is we don't need a garbage collector. In the early days, garbage collectors were slow and buggy. So the extra execution time bought by value semantics was nothing compared to the extra time with using a garbage collectors. But with more and more optimizations, garbage collectors are much faster now. As a result, languages with garbage collectors are catching up with C++ in speed.

C++ developers need to improve C++ further to retain its dominant place. So, they work on things to minimize the extra execution time bought by value semantics. Copy elision we learnt before is an attempt of that, saving copy constructor calls with compiler optimizations. Move semantics is another attempt.

C++11 introduced move semantics, where the source object is a `rvalue`, and its data can be moved into target instead of copying.

