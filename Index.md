Welcome to our new help pages, refreshed and consolidated from our various blog posts. Be hope they will be usefull both as a sequential read for newcomers and as a reference for our existing users.

## [Genesis of SymbolSource - *PDBs, debugging, symstore.exe and pain*](Genesis)

When source code is compiled on Microsoft platforms, whether into native machine code (as with C or C++) or an intermediate language (as with C#), information that would normally be lost in this process, such as line-to-instruction mappings used by debuggers for source-stepping, is saved into PDB files, also known as symbol files. Symbols are normally placed alongside compiled DLL and EXE files and read from there, but Visual Studio, WinDbg and other programs implement the SRCSRV protocol, by which they can download matching symbols and sources on demand from remote servers. This lets them get those missing mappings for assemblies only provided in binary form.

SymbolSource was developed to solve two major problems with symbol files:

1. Creating and maintaining a symbol store using tools provided by Microsoft is insanely painful when you're not using TFS.
1. There was no symbol store for the growing number of open-source libraries, which made debugging them a lot harder.

[Learn more about the problems with symbol files and how SymbolSource solves them](Genesis)

## [SymbolSource Architecture - *instances, accounts, repositories*](Architecture)

SymbolSource is an online repository of packages, symbols and sources, compatible with NuGet, OpenWrap and SRCSRV clients such as Visual Studio and WinDbg. 

SymbolSource was designed to host:

* publicly available repositories of symbols and sources matching official releases of open-source libraries, such as NHibernate, available on nuget.org, chocolatey.org or their respective websites;
* private, secure repositories of NuGet and OpenWrap packages with their matching symbols and sources;
* company instances in a software-as-a-service model, with separate databases of users and repositories, configurable by a company delegated administrator.

Because of its flexibility, SymbolSource can provide repositories that remain in sync with nuget.org, chocolatey.org, myget.org and your own package feeds. Depending on the actual use case, this can include sharing users, repositories, projects and permissions, or any combination thereof.

[Learn more about the structuring of SymbolSource instances, accounts and repositories](Architecture)

## [Using SymbolSource - *configuring Visual Studio, WinDBG and debugging*](Using)

In most cases, you will want to use SymbolSource to download PDB files and sources on-demand in your debugging sessions. This will make tracking down elusive bugs much easier, as you'll be able to step into many of the third-party libraries that you reference in binary form, without the need to replace them with locally compiled versions manually. SymbolSource also makes it easier to do post-mortem analysis of crash dumps, in which case replacing references in this way isn't even possible. Configuring you debugger to use Visual Studio consists of just a few simple steps, and it's usually a one time thing only.

We recommend SymbolSource as the next tool to learn right after NuGet or OpenWrap.

[Learn more about configuring Visual Studio and using SymbolSource during debugging](Using)

## [Publishing to SymbolSource - *packaging and pushing with NuGet and OpenWrap*](Publishing)

Symbols and sources are only available on SymbolSource once published by the authors of their respective binaries. Thankfully, a lot of open-source groups and individual developers already do so as part of their regular release procedures.

Publishing symbols and sources is really easy and can be done with specially structured NuGet and OpenWrap packages. Symbol package support is even built into nuget.exe's pack command, which makes creating one as simple as adding a -symbols command-line arguments.

Publishing to SymbolSource is a win-win situation. Users of your libraries get a chance to step into your source effortlessly, which helps them better understand how the libraries work and track down bugs more easily. This lets them get their job quicker and leads to less support requests and better, more detailed bug reports for you.

[Learn more about the methods, tools and best practises of publishing to SymbolSource](Publishing)

## [Packages in SymbolSource - *support for serving NuGet and OpenWrap packages*](Packages)

SymbolSource not only accepts incoming packages with symbols and sources, but it can also host all sorts of NuGet and OpenWrap packages. In this respect it is similar to nuget.org, except that it lets you create many separate package feeds, either public, or private - with read or write access restricted to certain users. SymbolSource is to date also the only implementation of an OpenWrap package server widely available.

[Learn more about using SymbolSource as a package store with NuGet and OpenWrap](Packages)

## [SymbolSource Pricing - *real-world's foothold in SymbolSource*](Pricing)

Running servers and maintaining software isn't free, but because of the value that SymbolSource brings for all of the developers out there, and the countless times it has helped us ourselves on our day jobs, we always have and always will provide it for free, as long as you decide to publish symbols and sources openly.

If you'd like host private files with SymbolSource and secure them from third-party access, we'll ask you to have a look at our pricing plans and pay a fair price for the service level that suites you. You can also decide to use the Community edition outlined below.

[Learn more about SymbolSource editions and pricing](Pricing)

## [SymbolSource for free - *sharing the idea despite the costs*](Basic)

Becasue our mission at SymbolSource is most of all promoting our vision of debuggable third-party dependencies and widening symbol server adoption, we maintain a simplyfied, open-source and free-for-all version of our software: SymbolSource.Server.Basic. It lacks many of the security and managment features of the full edition, but you are free install as many instances of it as you like, on premise or in the cloud, and secure them in any way possible with IIS. Most importantly though, SymbolSource.Server.Basic shares all of the core NuGet, OpenWrap and SRCSRV code with the online edition, so you can use it with the same packaging and publishing procedures.

[Learn more about installing and using the community edition of SymbolSource](Basic)

## Troubleshooting SymbolSource - *we're here should anything go wrong*

Should you have any questions or issues while using SymbolSource, there are a few ways to get help:

1. [Consult the SymbolSource FAQ](FAQ)
1. [Start a discussion in our Google Group](http://groups.google.com/group/symbolsource)
1. [E-mail us a support request directly](mailto:symbolsource@symbolsource.org)
1. Ask us on Twitter:  [@TripleEmcoder](http://twitter.com/TripleEmcoder) or [@BenekDlugonogi](http://twitter.com/BenekDlugonogi)



