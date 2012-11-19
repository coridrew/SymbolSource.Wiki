## Using SymbolSource as a NuGet package repository

Just pass URLs below to every `nuget.exe` command with `-source` or configure them in Visual Studio or any other NuGet-compatible software.

[Learn more about the structuring of SymbolSource instances, accounts and repositories](Architecture)

### Private repositories

* NuGet push URL: `http://nuget.gw.symbolsource.org/Public/<repository-name>`
* NuGet package feed: `http://nuget.gw.symbolsource.org/Public/<repository-name>/FeedService.mvc`

### Company account

* NuGet push URL: `http://nuget.gw.symbolsource.org/<company-name>/<repository-name>`
* NuGet package feed: `http://nuget.gw.symbolsource.org/<company-name>/<repository-name>/FeedService.mvc`

## Using SymbolSource as an OpenWrap package repository

### Preparing OpenWrap

1. OpenWrap 2.0.2 is required to use our repositories. You can skip this entire section if you already have it.

1. Make sure you have OpenWrap 1.0.2 before upgrading, otherwise bad things may happen:

   `o update-wrap openwrap -system`

1. Add the official beta repository:

   `o add-remote -name beta -href http://wraps.openwrap.org/beta`

1. Update OpenWrap in the system repository (or skip `-system` if you're running from a test project):

   `o update-wrap openwrap -system`

### Configuring repositories

It might be good idea to start up [Fiddler](http://www.fiddler2.com) at this point to verify that o.exe is contacting remote repositories. It should, but this is all still beta stuff. Also, you will need an account on SymbolSource to complete some of the steps, so go ahead and [register](/Public/Account/Register) now, if you don't have one yet. From now on `<login>` will be, you guessed it, your login, and `<key>` will be your [Visual Studio access key](/Public/Home/VisualStudio).

1. Add the public OpenWrap repository:

   `o add-remote -name public -href http://openwrap.gw.symbolsource.org/Public/OpenWrap`

1. Go to [Metadata](/Public/Metadata) and create a private repository for upload testing. 'OpenWrap' is a nice name. Also make note of our Visual Studio key while you're there, it will be used as the password for authentication.

1. Register the private repository with your install of OpenWrap:

   `o add-remote -name private -href http://openwrap.gw.symbolsource.org/Public/Private.<login>.OpenWrap -user <login> -pwd <key>`

1. Try listing packages (`o.exe` should access `index.wraplist` from both locations, plus `wraps.openwrap.org`):

    `o list-wrap -remote`

1. If everything goes fine, you'll see a package from the public repository:

    ` - symbolsource-repositorytest (available: 0.0.1.84582059)`

### Using the repositories

1. Go to a new directory and initialize a new wrap:

    `o init-wrap`

1. Now you can add the test library from SymbolSource:

    `o add-wrap symbolsource-repositorytest`

### Uploading your packages

1. Take a package from somewhere or just build your test wrap:

   `o build-wrap`

2. Publish package to SymbolSource:

   `o publish-wrap -path test-1.0.wrap -remote private`

3. Verify that the package was uploaded:

   `o list-wrap -remote`

### Repository URLs

In general, you should use these repository URLs with `add-remote`:

* for private repositories: `http://openwrap.gw.symbolsource.org/Public/<repository-name>`
* for company accounts: `http://openwrap.gw.symbolsource.org/<company-account>/<repository-name>`

### Symbol package support

With a SymbolSource repository for OpenWrap you also get the entire on-demand debugging experience. Just make sure, before uploading, that your package contains PDB files in `bin-*` folders and all the sources you used to build the package from in an `src` folder - you will need to add those files manually (with a ZIP tool) for now. The package will be then processed on the SymbolSource servers. The version for download will be stripped from these files, but they will be available on-demand for Visual Studio. Once OpenWrap gets flavor support, these files will be included automatically during `o build-wrap`.
