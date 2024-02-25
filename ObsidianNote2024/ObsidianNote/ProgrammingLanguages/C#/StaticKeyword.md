Putting a static keyword before return type would make a method a static method, and the same goes about property.

Meaning, this method now belongs to the class, not the instance of the class. 

We access this method with the class name not the instance name. 

The problem is this method now can only reach the static members(methods, fields) inside its own class. For members in other classes, whether they are static or not, it is reachable. But in its own class, one static member can only reach for other static members.