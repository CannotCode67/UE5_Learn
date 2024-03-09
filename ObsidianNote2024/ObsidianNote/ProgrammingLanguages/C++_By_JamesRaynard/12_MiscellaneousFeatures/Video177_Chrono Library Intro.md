
C++ inherit some time-handling types and functions from C.

`clock()` returns the number of clock ticks since the program started. The return value is of type `clock_t` which is a synonym for integer. Its precision is implementation-defined, usually it is to the nearest microsecond. So we have to do some arithmetic to convert it into seconds.

`time()` takes a variable of type `time_t` by address, and this `time_t` is a synonym for integer. This will set the variable to the number of seconds since 1st of January, 1970. Its precision is clearly 1 second.

C++11 provided a chrono library to handle time more precisely. It is in the `std::chrono` namespace, under the `<chrono>` header. It introduces three important concepts.

A clock is something with a start date known as epoch and a tick rate. Under this concept, C's clock has a start date 1st of January, 1970 and a tick rate of 1 second.

A time point is the number of clock ticks since the epoch at a given point of time.

A duration is an interval between two time points, measured in clock ticks.