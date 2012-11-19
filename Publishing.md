## Creating symbol packages with NuGet

The latest information on creating symbol packages is available in NuGet's documentation, at this page that we contributed:
[http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-symbol-package](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-symbol-package)

### Pushing packages

Here we'll sum up the most important SymbolSource specific information.

When create your package set with `nuget.exe pack -symbols <nuspec-or-project-file>` you get two files:

* a regular `.nupkg` file with binaries (`.dll`, `.exe`, `.winmd` files) and optionally content, tools and scripts,
* an additional `.symbols.nupkg` file with symbols and source.

You can then push both at once by simpy running  `nuget.exe push <package-file>`:

* the regular package ends up at [nuget.org](http://nuget.org) as usual,
* the symbol package is pushed to [`nuget.gw.symbolsource.org/Public/NuGet`](http://www.symbolsource.org/Public/Metadata/NuGet) (if allowed by [nuget.org](http://nuget.org))

Of course it's possible to push to a different repository for added privacy or to test a package before making it public. Once logged in to SymbolSource, you can create a new private repository by going to the [Metadata](http://www.symbolsource.org/Public/Metadata) page.

[Learn more about the structuring of SymbolSource instances, accounts and repositories](Architecture)

### Validating packages

Before pushing a package to SymbolSource it is always great to run it through a validation process which is
 
* a [NuGet Package Explorer](http://npe.codeplex.com) plugin, which you can get it right from NPE's Plugin Manager,
* a `nuget.exe` plugin, which you can get with [Chocolatey](http://chocolatey.org):

 `cinst SymbolSource.Integration.NuGet.CommandLine`

These plugins will run the same validation process that SymbolSource does on uploads, except that it will update all problems at once with suggestions on how to fix them.

Unfortunately `nuget.exe` does not run rules on symbol packages at the moment, which kind of defeats the purpose... If you'd like that behaviour changed as much as we do, please share your thought in this NuGet Ecosystem [discussion thread](http://nuget.codeplex.com/discussions/401639). For now just use NPE.

## NuGet-SymbolSource command cheatsheet

With all the different endpoints to push to, it is very useful to store your API key. Here's a quick cheatsheet of the commands needed for that:

* Storing your [nuget.org](http://nuget.org) key, which also enables pushing to [symbolsource.org](http://symbolsource.org):

 `nuget.exe setapikey <nuget-key>`

* Storing your [myget.org](http://myget.org) key:

 `nuget.exe setapikey <myget-key> -Source https://www.myget.org/F/<feed-name>`

* Storing your [myget.org](http://myget.org) key for [symbolsource.org](http://symbolsource.org) (this one you need to do explicitly):

 `nuget.exe setapikey <myget-key> -Source https://nuget.gw.symbolsource.org/MyGet/<feed-name>`

And while we're at it, it's also worth mentioning all the push commands:

* Pushing a package to [nuget.org](http://nuget.org) (a symbol package will be detected and pushed to [symbolsource.org](http://symbolsource.org) automatically):

 `nuget.exe push <package-file>`

* Pushing a symbol package to [symbolsource.org](http://symbolsource.org) explicitly (if you want to test it first):

 `nuget.exe push <package-file> -Source https://nuget.gw.symbolsource.org/Public/NuGet`

* Pushing a package to [myget.org](http://myget.org):

 `nuget.exe push <package-file>` -Source https://www.myget.org/F/<feed-name>`

* Pushing a symbol package to [symbolsource.org](http://symbolsource.org):

 `nuget.exe push <package-file> -Source https://nuget.gw.symbolsource.org/MyGet/<feed-name>`

Unfortunately at the moment `nuget.exe` requires you to be very specific about the URLs, and the SymbolSource endpoint is only hardcoded in for [nuget.org](http://nuget.org). It would be great if it could do symbol package autodetection and pushing also in case of [myget.org](http://myget.org) feeds. If you find this a nice idea, have a look at [this discussion](http://nuget.codeplex.com/discussions/283978) and vote on [this issue](http://nuget.codeplex.com/workitem/1712). Ideally that would also enable you to set up pairs of internal [NuGet.Server](http://nuget.org/packages/NuGet.Server) and [SymbolSource.Server.Basic](http://nuget.org/packages/SymbolSource.Server.Basic) repositories.