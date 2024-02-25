
`std::shuffle()` rearranges an iterator range in a random order. It takes an iterator range and a random number engine.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
static mt19937 mt;
shuffle(begin(vec), end(vec), mt);
```
`shuffle()` use uniformly distribution underneath to process the generated random number into index, then do a swap `std::swap(vec[i], vec[i_random]);`, that is one go. We are not sure how many times this operation is repeated.

There used to be a function `random_shuffle()` which uses `rand()` to do similar things, but it gets deprecated and removed.

One distribution class is the `std::bernoulli_distribution`. It rescales a sequence of numbers into Boolean values. It is like tossing a coin, which is 50-50.
```
static mt19937 mt;
static bernoulli_distribution bd;
if (bd(mt))
	cout << "we get true" << endl;
else
	cout << "we get false" << endl;
```