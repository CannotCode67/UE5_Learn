
Software is deterministic, so it needs to be able to produce the same result, therefore, no real random numbers. The ones we use are pseudo-random number generators (PRNG). They are computational algorithms which generate a random-looking sequence of numbers. To initialize one, we use a number called seed. With the same seed, we get the same result.

The PRNG we have for C++ is inherited from C, and it is under `<cstdlib>` header.
`rand()` returns the next number in a sequence, and this sequence ranges from `0` to `RAND_MAX` inclusive. Different compilers implement `RAND_MAX` differently. And if we want a random floating number between 0 and 1, we need to do some arithmetic operations to scale it.

To use a different seed, we use `srand()` and pass a number as the seed. The traditional practice is to use another function `time()` as the seed. `time(0)` will return the current time in second since 1970.

The problem with `rand()` and `srand()` is not random enough. When we scale it, we introduce bias resulting some numbers are more likely to appear. And the traditional practice just makes it easy to guess the next number of sequence, making it less random and less safe for security purpose.

C++11 introduced a number of classes for working with random numbers. They are defined under the `<random>` header.
A random number engine is implemented as a functor. The constructor generates the sequence of pseudo-random numbers. When we call the its `()` operator, we get the next number from the sequence. Nothing too different from `rand()`.
```
static default_random_engine eng; // create the pseudo-random sequence
eng(); // get the next number of the sequence
```

A distribution is also implemented as a functor. Its constructor takes the range as arguments. Its `()` operator takes a function object as argument, most likely a random number engine.
```
static default_random_engine eng;
static uniform_int_distribution<int> intDist(0, 10);
intDist(eng); // give a number from 0 to 10 based on eng's returned number
```
When we call a distribution constructor, the object is set to convert number into a given range, here it is from 0 to 10 inclusive. When we call this distribution object's `()` operator, it automatically calls the random engine's `()` operator and process the returned number to fit in the distribution range.

There are three random number engines provided. 
The `default_random_engine` is a wrapper of the `rand()`, producing sequences with a period of 2^16 -1. 
Another random number engine is `mt19937`, which gets its name from the underlying algorithm called Mersenne Twister. It produces sequences with a period of 2^19937-1. This is usually the choice since it is good enough for general purpose programming.
The last random number engine is `random_device`. It produces a random number from system entropy data. System entropy data is data available to the system at runtime, such as process ID, CPU temperature, etc. This engine scrambles those data to produce a random number. Because it actually uses data from hardware, it is the slowest of the three. But it is truly random, so it is often used to seed the `mt19937` engine.
```
static random_device rd; // random device constructor call
static mt19937 mt(rd()); // mt19937 constructor call with seed of rd
```

Distribution also comes with many kinds. We have seen `uniform_int_distribution<T>` to distribute integer inside a given range uniformly. `uniform_real_distrubution<T>` is the same, but uniformly distributes floating point number. Here uniformly means all integers inside that range have equal chance to appear. Other ways to distribute are Bernoulli, Normal, Poisson, etc. For more details, we should check the standard library documentation.

Lastly, we should make the engine and distribution objects static. As they take a long time to initialize, and we should do it once for program. So putting them in global space is the way to go.