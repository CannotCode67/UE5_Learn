
The `std::chrono` library provides three clocks.

System clock measures the wall time using the hardware system's clock. It can be modified to synchronize with the real time by the system or other applications. So time can jump forward or even backward.

Steady clock is the idealized clock which only goes forwards, one tick at a time. It is like a program's internal clock, unaffected by things outside of the program.

High resolution clock is the most precise clock supported by the system. Usually, it is just an alias for system clock or steady clock to save some work.

They all have the static function `now()` to return the clock's current time point.
```
system_clock::now();
steady_clock::now();
high_resolution_clock::now();
```
The current time point means the duration since the clock's epoch. We can add or subtract time points to get a duration representing their difference of time intervals.

Let's see an example.
```
long long fibonacci (long long n) {
	return (n < 2)? n : fibonacci(n-1) + fibonacci(n-2);
}

int main() {
	auto start = steady_clock::now();
	long long n = fibonacci(40);
	auto finish = steady_clock::now();
	auto duration = duration_cast<milliseconds>(finish-start).count();
	cout << duration << endl;
}
```

Another function `sleep_for()` is under the `<thread>` header, and it is related to multi-threading. The reason to mention it here is because it also deals with time.

The function can pause the current thread for a specified duration.
```
#include <thread>

cout << "begin!" << endl;
this_thread::sleep_for(2s);
cout << "finish!" << endl;
```
When the thread is put to sleep, the system scheduler or the thread scheduler will start other things they expect to take 2 seconds, and they will not come back until those things are finished. Therefore, 2 seconds is really the bottom line here, usually it sleeps for a slightly longer time.