// Compliers
C++ is like C#, a programming language that needs to be complied before executed. Therefore, we need a complier for C++. However, unlike C#, C++ isn't owned by a company, so there are multiple compliers to choose. In C#, we mainly use the complier written by Microsoft. The most popular compliers for C++ are GCC, Clang, and one provided by Microsoft. They are usually the ones which adopt the new language features at the fastest pace. Other compliers may still support only the older version of C++, or only some new features, but not all of them. GCC and Clang are more various platforms, and Microsoft complier is for Windows only.

// Linker
The term linker sounds unfamiliar. The whole build process of turning C++ source code into executatble file goes like this: We have multiple source code files, and through complier, they become object files. Those object files through the linker become one executatble file. That's where the linker fits in. And usually, linker and complier come in a pair, so you choose GCC or Clang for complier, there is already a linker included.
![[buildProcess.png]]

// Supporting Website
When we have questions about the language, where do we find information? C++ is regulated by a committee, and it is this committee gets to decide what features to add into the language in the future, as well as responsibel to update the standard library for C++. What is the standard library for C++? It is the C++ equavilent of .Net's System library for C#. Members of this committee are compiler vendors or some other software companies. The website of this committee is isocpp.org, which is the supporting website for C++. Like the supporting website for C# is Microsoft. However, Microsoft also provides C++ language documentation in its own website, which is also reliable. There is also a website called cppreference.com, which is also useful.

// the int main()
C++ has a main() function which indicates the starting point of the whole application. And, in the end, we return 0 to indicate the program executed successfully, which is just a convention.
```
int main()
{
	//...
	return 0;
}
```
