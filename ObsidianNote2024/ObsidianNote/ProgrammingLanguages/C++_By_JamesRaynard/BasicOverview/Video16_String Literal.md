
The C-Style string literal is an array of const char, terminated by a null character.
We cannot do what we often do with words, concatenate.

Since C++14, the standard library offer suffix to convert a C-Style string into a standard string.
We need to use its namespace to enable the suffix.
```
using namespace std::literals;
...
string str = "Hello"s; // the s is the suffix
cout << "Hello"s + "World!"s; << endl; // works everywhere expecting a std::string
```

In string literals, some characters are called escape characters.
`\n` (pronounced back slash n) means newline character in string literal.
`\"` means the double quote string literal, not string terminator.
`\\` means the back slash string literal.
`\t` means tab space string literal.
etc.

Consider the message:`<a href="file">C:\"Program Files"\</a>\n`
To write the message as a string literal, we get:
`string url = "<a href=\"file\">C:\\\"Program Files\"\\</a>\\n"`
Which is prone to mistake and hard to read.

Since C++11, we have raw string literals to avoid this.
`string url = R"()";` Anything goes into the `()` will be exactly appear like that.
`string url = R"(<a href="file">C:\"Program Files"\</a>\n)";`
If the message contains `)"`, then it could mess up the terminating position.
The solution is we add a delimiter.
`string url = R"x()x";` The `x` is the delimiter we add, so now the termination position is captured when `)x"` is found, like below.
`string url = R"x(<a href="file">C:\"Program Files (x86)"\</a>\n)x";`
