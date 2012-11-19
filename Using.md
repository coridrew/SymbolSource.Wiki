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
 
<ol> 
    <li>Go to Tools -> Options -> Debugger -> General.</li> 
    <li>Uncheck “Enable Just My Code (Managed only)”.</li> 
    <li>Uncheck “Enable .NET Framework source stepping”. Yes, it is misleading, but if you don't, then Visual Studio will ignore your custom server order (see further on) and only use it's own servers.</li> 
    <li>Check “Enable source server support”.</li> 
    <li>Uncheck “Require source files to exactly match the original version”</li> 
    <li>Go to Tools -> Options -> Debugger -> Symbols.</li> 
    <li>Select a folder for the local symbol/source cache. You may experience silent failures in getting symbols if it doesn't exist or is read-only for some reason.</li> 
    <li>Add symbol servers under “Symbol file (.pdb) locations”. Pay attention to the correct order, because some servers may contain symbols for the same binaries: with or without sources. We recommend the following setup:
        <ul> 
            <li><code>http://referencesource.microsoft.com/symbols</code></li> 
            <li><code>http://srv.symbolsource.org/pdb/Public</code> or the authenticated variant (see above)</li> 
            <li><code>http://srv.symbolsource.org/pdb/MyGet</code> or the authenticated variant (see above)</li> 
            <li>(other symbol servers with sources)</li> 
            <li><code>http://msdl.microsoft.com/download/symbols</code></li> 
            <li>(other symbol servers without sources)</li> 
        </ul>
     </li> 
</ol> 

## Configuring \_NT\_SYMBOL\_PATH (WinDbg)

...
 
## Tips &amp; Tricks
 
<ol> 
    <li><p>When using a symbol server for some of your dependencies be sure <strong>not</strong> to have any other PDB files of those libraries present in the same directory as the DLL files. When debugging code using Library.dll, Visual Studio will always first load Library.pdb from the same directory, which will disable symbol and source server usage.</p></li> 
    <li><p>To more effectively debug .NET Framework code it is useful to disable optimizations using the following command (environment variable): <code>set COMPLUS_ZapDisable=1</code></p></li> 
    <li><p>To speed up debug session startup it is recommended to go to Tools -> Options -> Debugger -> Symbols, select “Only specified modules” and enter appropriate assembly names in the dialog provided.</p></li>
</ol> 