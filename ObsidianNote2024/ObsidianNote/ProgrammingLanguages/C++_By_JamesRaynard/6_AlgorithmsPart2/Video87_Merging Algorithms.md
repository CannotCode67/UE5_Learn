
`std::merge()` combines two sorted iterator ranges into a destination. The destination will contain all the elements from both ranges, in the order where `<` operator is used.
```
vector<int> vec1 {3, 1, 4};
vector<int> vec2 {2, 1, 3};
sort(begin(vec1), end(vec1));
sort(begin(vec2), end(vec2));
vector<int> vec3;
merge(cbegin(vec1), cend(vec1), cbegin(vec2), cend(vec2), back_inserter(vec3));
```
Now, we have `vec3` of `{1, 1, 2, 3, 3, 4}`.

`std::set_intersection()` combines two sorted iterator ranges into a destination. The destination will contain only the elements present in both ranges, in the order where `<` operator is used.
```
vector<int> vec1 {3, 1, 4};
vector<int> vec2 {2, 1, 3};
sort(begin(vec1), end(vec1));
sort(begin(vec2), end(vec2));
vector<int> vec3;
set_intersection(cbegin(vec1), cend(vec1), cbegin(vec2), cend(vec2), back_inserter(vec3));
```
Now, we have `vec3` of `{1, 3}`.

`std::union()` combines two sorted iterator ranges into a destination. The destination will contain elements present in either ranges, in the order where `<` operator is used. However, elements present in both ranges will appear only once.
```
vector<int> vec1 {3, 1, 4};
vector<int> vec2 {2, 1, 3};
sort(begin(vec1), end(vec1));
sort(begin(vec2), end(vec2));
vector<int> vec3;
set_union(cbegin(vec1), cend(vec1), cbegin(vec2), cend(vec2), back_inserter(vec3));
```
Now, we have `vec3` of `{1, 2, 3, 4}`.