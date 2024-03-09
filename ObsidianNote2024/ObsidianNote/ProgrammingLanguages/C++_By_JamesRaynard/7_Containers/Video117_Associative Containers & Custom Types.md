
When we use associative containers, we expect elements to be in order where `<` operator is used. However, when we store custom types, we might not overload the `<` operator, and things can go wrong.
```
class book {
private:
	string title;
	string publisher;
public:
	book(string title, string publisher) : title(title), publisher(publisher) {}
};

int main() {
	multimap<string, book> library;
	book easy_math("Easy Math", "Nike");
	book hard_math("Hard Math", "Nike");
	book advanced_math("Advanced Math", "Nike");
	library.insert({"Albert", hard_math});
	library.insert({"Albert", easy_math});
	library.insert({"Ada", advanced_math});
	// the order is below
	// {("Ada", advanced_math), ("Albert", hard_math), ("Albert", easy_math)}
}
```
The key will be in order where string's `<` operator is used, but under the same author, the books are not in any particular order. What if we want the books under the same author to be arranged based on their titles, like a real library? One solution is to create what is known as compound key. So we use a custom type as the key, and we overload its `<` operator to provide the logics to arrange our books. If we use a custom type as the key, we have to overload the `<` operator, otherwise, it will not compile.
```
class book_idx {
private:
	string author;
	string title;
public:
	book_idx(string author, string title): author(author), title(title){}
	bool operator < (const book_idx& other) const {
		if (author == other.author)
			return title < other.title;
		return author < other.author;
	}
};

int main() {
	multimap<book_idx, book> library;
	
	book easy_math("Easy Math", "Nike");
	book_idx easy_math_idx("Albert", "Easy Math");
	book hard_math("Hard Math", "Nike");
	book_idx hard_math_idx("Albert", "Hard Math");
	book advanced_math("Advanced Math", "Nike");
	book_idx advanced_math_idx("Ada", "Advanced Math");
	
	multimap.insert(hard_math_idx, hard_math);
	multimap.insert(easy_math_idx, easy_math);
	multimap.insert(advanced_math_idx, advanced_math);
	// order is below
	// {((advanced_math_idx, advanced_math), (easy_math_idx, easy_math), (hard_math_idx, hard_math)}
}
```

Above is how we handle it in ordered associative containers. The unordered associative containers use the hash number of the keys to organize their elements. The standard library provides the hash function `std::hash<T>` for all built-in types and types defined in library. But if we use custom types for keys, then we have to write our own hash function to generate hash number from our custom types. In that case, we also need to overload the `==` operator for our custom types.