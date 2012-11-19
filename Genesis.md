## What happens when I debug code in Visual Studio?

Whether you debug native code compiled from languages such as C++ or managed code that the CLR runtime compiles just-in-time during runtime, the key element of the process is a correct PDB file for the EXE or DLL you are working on. A PDB file contains metadata describing code as it was before compilation: what classes, methods or functions it consisted of before being transformed into much simpler machine-level concepts. It also provides reverse mappings between CPU registers and variables in high-level code and between machine instructions and programming language statements that generated them. Basically, it stores information lost during compilation that is essential for a debugger to allow step-by-step code execution and variable value monitoring. Of course, for managed code, much of this information is stores together with executable code in EXE or DLL assemblies, but a PDB file is still necessary because of code optimizations done by .NET language compilers.

## Why haven't I ever heard of PDB files?

Symbol files (i.e. PDB files, but historically these were also DBG files) are automatically generated during compilation, so there is no need to worry about them when working on projects for which you have all source code and the dependencies are mostly bug-free and well documented &ndash; projects that use only environment-provided libraries such as the C or C++ standard libraries or the .NET BCL (Base Class Library). However, once more dependencies are added to a project and issues arise that could be more easily solved by debugging the library code in its context of use, a need for symbol files arises.

## Where can I get symbols for the libraries that I use?

We already mentioned one method of obtaining symbol files: compiling the source code. It has a few major disadvantages, however, even for source code that you own. Including library projects in a solution can increases compile times drastically, and almost always will lead to performance degradation when working with Visual Studio. It is also preferable to link binaries instead of including source code to reference official, versioned builds &ndash; for ease of dependency management, increased security and stability. There are two ways you can obtain symbol files in this scenario: with the official binary distribution (or as a separate package) or through a symbol server.

## What is a symbol server?

Symbol server is an HTTP server that provides PDB files for any EXE or DLL requested. When using a symbol server you no longer have to manage symbol files for you dependencies (be it external libraries or internal &ldquo;common&rdquo; or &ldquo;tools&rdquo; projects) and keep them in sync with your binaries. When debugging, Visual Studio will query symbol servers for PDB files matching exactly the binary file you are debugging. A public symbol server is provided by Microsoft for most of the Windows libraries. When configured, this will give you meaningful stack traces, among other things. You can read more about it in [KB311503](http://support.microsoft.com/kb/311503).

## What is a source server?

Having appropriate symbol files is only half of the solution. To be able to debug code, you still need source files used for compiling the binary file. When working on your own projects, you already have the source and symbol files (be aware though that source file paths are hardcoded into PDB files as absolute paths, so sharing them is not straightforward). When stepping into external dependencies, you need a means of getting the right source file. This is where a source server comes into play. It is a complementary server to a symbol server mentioned earlier. When PDB files are uploaded to a symbol server, instructions are eimbedded into them, so that Visual Studio will be able download source files. Microsoft provides a source server for the .NET Framework. You can read about it at [referencesource.microsoft.com](http://referencesource.microsoft.com).

## How can I use a symbol and server?

We won't repeat here what others have already brilliantly described. To get a good feelling of how you can use a symbol and source server, have a look at a <a href="http://blogs.msdn.com/b/camerons/archive/tags/symbol+server/">blog post series on the matter</a> by Cameron Skinner.

## How can I create a symbol and source server?

...

## What do I gain by SymbolSource instead of symstore.exe?

...

## What do I gain as a library consumer only?

...
