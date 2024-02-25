LINQ comes in two flavors, and they are mostly interchangeable. Whether we should stick to one of them and choose which one to use are merely personal choices. But the bottom line is we as developers need to have the ability of reading both syntaxes, so that when we are given a piece of code written by other developers, we can understand it nonetheless.

First, let's see both syntaxes. Both queries work just the same, but query uses method syntax, and query 2 uses query syntax. And under the hood, the quert syntax are just keywords referring to the same extension methods. Query syntax must start with the keyword from, and end with the keyword select or the keyword group.
```C#
var query = developers.Where(e => e.Name.Length == 5)
                      .OrderBy(e => e.Name)
                      .Select(e => e);

var query2 = from developer in developers
             where developer.Name.Length == 5
             orderby developer.Name
             select developer;
```