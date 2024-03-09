
`std::next_permutation()` takes an iterator range, then it reorders the elements in that range to give the next permutation in ascending order. It returns a bool, depending on whether there is a next permutation.
```
string str {"abc"};
do {
	cout << str << endl;
} while (next_permutation(begin(str), end(str)));
// we will get display below
//abc
//acb
//bac
//bca
//cab
//cba
```

`std::prev_permutation()` reorders the elements to give the previous permutation. For it to work, the elements must be sorted in descending order.
```
sort(begin(str), end(str), [](int m, int n){return m > n};); // cba
do {
	cout << str << endl;
} while(prev_permutation(begin(str), end(str)));
// we will get display below
//cba
//cab
//bca
//bac
//acb
//abc
```

`std::is_permutation()` takes two iterator ranges, and it returns true if both ranges contain the same elements, even if they are in different orders.
`is_permutation(cbegin(vec), cend(vec), cbegin(vec1), cend(vec1));`