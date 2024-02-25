C# is the programming language we use to write the program or script. 

.Net is the runtime or framework that would translate the C# code into something the computer can understand and execute (complier is part of the runtime). It contains two parts, one is the CLR (common language runtime) and the other is FCL (framework class library). Runtime manages how the translated code get executed by the computer, because C# is a relatively high-level language, meaning we don't need to care about memory management. Low-level tasks like that are managed by runtime. Library, on the other hand, provides some written and tested classes, so that we don't need to reinvent the wheel every time.  

.Net framework is a framework that has been around since about 2001, and it is only for Windows. 

.Net core is a framework that is relatively new, and it works across various platforms.  

Both .Net framework and .Net core are versions of .Net supporting C# language, it is just one can work on Windows only, and one can work on various platforms. 

We use command line interface(CLI) to create our directory. Then, use dotnet command in CLI to build the project. We then, can use editor to write our c# code. After we finish the writing, we can go back to CLI to run and test the project. 

In CLI, we can tell the computer to run or test the project through dotnet commands. It is the CLI to control the construction of the files and folders, instead of mouse clicking. It is much powerful and convenient than doing it with mouse clicking. 

First, we go to CLI, create a top level directory for our project, then inside this directory, create two more directories, one called src, and the other called test. Then we go to the src directory, run CLI command dotnet new with template choice to create the project.
After the project is created, inside this directory, there are two important files, the project file and the source code file. The project file contains information about references, and the source code file contains the production code. Now, when we are in src directory, we can run CLI command dotnet run, to run the source code.
Behind the scene, the CLI command dotnet run is an ordered combination of three CLI commands, dotnet restore, dotnet build, and dotnet with the system path to the dll file. The dotnet restore command would look into the project file to grab all the external dependencies, so that when the dotnet build command runs, it can see all the codes (our source code and external referenced code) and compiles them into a binary representation, a file with the same name of the project but with a .dll in the end. Lastly, the dotnet command with system path to the dll file would tell the computer to run this dll file with dotnet runtime. Directly run this dll file without dotnet runtime would not work.

However, we still need to use editor to write code. 

Visual studio, on the other hand, can run those CLI commands through a button press on the visual studio interface, that's why visual studio is an IDE, and visual studio code is just an editor. With visual studio code, we need to go out to the CLI to run commands.