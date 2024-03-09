
We have learnt the compiler will synthesize special member functions if we don't provide them. Now with move semantics, compiler can even synthesize move constructor and move assignment operator, but on some conditions.

The conditions are this class does not define a copy constructor, copy assignment operator or destructor; all data members of the class are either built-in type, types with defined move operations, or static data members. Built-in types and types with defined move operations are moveable. Static data members are shared across all objects of a class.

The synthesized move constructor will call the move constructor for each member, and the synthesized move assignment operator will call the move assignment operator for each member.

If a class defines a move operator, then both copy operators will be synthesized as deleted or not provided by the compiler. So the compiler is saying by default, the class with move operators defined is move-only. We would need to write the copy operators our own.
Below is a reference picture made by the man who implements move semantics.
![[SpecialMemberFunctionsDeclaration_ref.png]]

The rule of five with move semantics. A class which allocates memory with `new` in its constructor, should have a destructor to call `delete`, a copy constructor do perform a deep copy, a copy assignment operator to perform a deep copy, and move operators to set the argument's pointer to `nullptr`.




