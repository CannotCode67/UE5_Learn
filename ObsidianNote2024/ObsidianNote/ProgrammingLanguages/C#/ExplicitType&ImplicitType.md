C# is a strongly typed language, so when we define a variable, we have to provide a type keyword, be it int, float, string, or whatever type we define, which is called explicit typing.

However, we can still use implicit typing through the usage of keyword var.

>var number = "17.1;"

With var, we can delay the decision for variable type until the initialization. Here, number would be a string type variable. Its type is determined as string when a string value is assigned to it. Therefore, implicit typing is not equal to weakly typing.

One might argue implicit typing brings few benefits, but add some confusions into the mix. Well, when a variable has a type that is complex and long, you will find that the keyword var increases quite a lot of readibility for the code.
