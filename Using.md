Hopefully by now you already know what a symbol and source server is, and how SymbolSource can be your one-and-only solution to all of the problems that come with them.

There or two things you need to figure out to use SymbolSource during your debugging sessions: the symbol/source server URL and your recommended configuration.

## Symbol/source server URL

If you only want to use the symbols and sources provided publicly, e.g for NuGet packages pushed to [nuget.org](http://nuget.org), you can just use the anonymous URL, which will search everything made public in the [Public](http://www.symbolsource.org/Public/Metadata) instance: 
 
**`http://srv.symbolsource.org/pdb/Public`**

You might probably also want to include stuff from the [MyGet](http://www.symbolsource.org/MyGet/Metadata) instance, as more and more nightly builds have symbols and sources published there:

**`http://srv.symbolsource.org/pdb/MyGet`**

The non-authenticated URLs allow you to use the service without registration, but we advise you to create an account on [SymbolSource](http://www.symbolsource.org/Public/Account/Register) or MyGet and and use the authenticated URLs, as it enables us to improve our service by tracking usage and potential problems. Should you come across any issues when using SymbolSource, we will be able to find the source of the problem quicker.

An authenticated URL will have your Visual Studio access key appended. You can check the API key after logging in at the Authentication page. The entire URL is also listed on the repository index pages [here (Public)](http://www.symbolsource.org/Public/Metadata) and [here (MyGet)](http://www.symbolsource.org/MyGet/Metadata). It will have this form:

**`http://srv.symbolsource.org/<company-name>/<login>/<key>`

Authenticated URLs also let you access symbols and sources stored in private repositories and company accounts.

## Configuring Visual Studio

To configure Visual Studio for symbol/server use, follow these instructions:
 
1. Go to Tools -> Options -> Debugger -> General.

 * Uncheck *Enable Just My Code (Managed only)*.
 * Uncheck *Enable .NET Framework source stepping*. Yes, it is misleading, but see below for an explanation.
 * Check *Enable source server support*.
 * Uncheck *Require source files to exactly match the original version*.

1. Go to Tools -> Options -> Debugger -> Symbols.
1. Select a folder for the local symbol/source cache. You may experience silent failures in getting symbols if it doesn't exist or is read-only for some reason.</li> 
1. Add symbol servers under “Symbol file (.pdb) locations”. Pay attention to the correct order, because some servers may contain symbols for the same binaries: with or without sources. We recommend the following setup:

 * Uncheck  *Microsoft Symbol Servers*. Another counterintuitive step, but see below for an explanation.
 * `http://referencesource.microsoft.com/symbols`
 * `http://srv.symbolsource.org/pdb/Public` or the authenticated variant (see above)
 * `http://srv.symbolsource.org/pdb/MyGet` or the authenticated variant (see above)
  * (other symbol servers with sources)
  * `http://msdl.microsoft.com/download/symbols` 
  * (other symbol servers without sources)

### Tips &amp; Tricks
 
1. To speed up debug session startup it is recommended to go to Tools -> Options -> Debugger -> Symbols, select *Only specified modules* and enter appropriate assembly names in the dialog provided.
1. When using a symbol server for some of your dependencies be sure **not** to have any other PDB files of those libraries present in the same directory as the DLL files. When debugging code using Library.dll, Visual Studio will always first load Library.pdb from the same directory, which will disable symbol and source server usage.
1. To more effectively debug .NET Framework code it is useful to disable optimizations using the following command (environment variable): `set COMPLUS_ZapDisable=1`

### Buts &amp; Whys

#### Why do I need to uncheck *Enable .NET Framework source stepping*? 

This is a very special feature of Visual Studio implemented a bit differently than the regular symbol loading. If you leave it on, Visual Studio will try downloading symbols for each project reference when you hit F5, before even starting the debugging session. Think of it as a precaching feature.

Unfortunately it completly ignores your server order, and only goes to `http://referencesource.microsoft.com/symbols` for PDBs. What's more, it also caches negative hits (symbols not found, e.g. for NuGet packages), so you don't get to wait on the next F5 run, but it does so based on the assembly filename, not hash. So if you have an identical assembly in a different path, you'll need to wait again while Visual Studio confirms that Microsoft doesn't have its symbols. This feature also ignores the *Only specified modules* setting while loading symbols. 

Then, during the debugging, you'll see this behaviour, depending on symbol loading settings:
* *All modules, unless excluded* - precached symbols will load instantenously, but Visual Studio will still try downloading symbols for modules loaded into the process dynamically, that weren't references (e.g. with `Assermbly.Load` or the `Microsoft.VisualStudio`.* helpers),
* *Only selected modules* - you will need to load the precached symbols manually from the Call Stack or Modules windows.

All in all we find *Enable .NET Framework source stepping* a very confusing and pretty much useless feature.

#### Why do I need to uncheck *Microsoft Symbol Servers*? 

This is equivalent to `http://msdl.microsoft.com/download/symbols`, which is a symbol server that hosts source-less PDBs for a lot of Microsoft products, like Windows system libraries. It also hosts symbols for the .NET Framework, but not source-enabled, contrary to `http://referencesource.microsoft.com/symbols`. You might notice that this item cannot be reordered, it will always be the first, so it will get you the wrong symbols. Perhaps that's why Visual Studio has that *Enable .NET Framework source stepping* options, that downloads the right symbols out of order. Anyway, it's better to just use our entire recommended configuration than to try guessing what Microsoft had in mind here.

## Configuring \_NT\_SYMBOL\_PATH (WinDbg)

...