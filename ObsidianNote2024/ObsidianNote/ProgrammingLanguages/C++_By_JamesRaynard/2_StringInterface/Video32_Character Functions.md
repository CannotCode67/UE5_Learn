
C has a bunch of character functions which are useful, so they get inherited by C++. They are defined in the `<cctype>` header, and operate on chars.
`isdigit(c)` returns true if c is a digit (0-9).
`islower(c)` returns true if c is lower case (a-z).
`isupper(c)` returns true if c is upper case (A-Z).
`isspeace(c)` returns true if c is a whitespace character (space, tab, etc.).
`ispunct(c)` returns true if c is a punctuation character (?, !, ., etc.).
and more.

`tolower()` and `toupper()` return the lower and upper case copy of their arguments, so not modifying the argument.

`std::string` are case sensitive, so lower case and upper case are regarded as two different data. C++ doesn't provided direct support for doing a case-insensitive comparison of two standard strings. We have to write our own.
