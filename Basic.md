For some time now, our dream has been for everyone to use and publish symbols in their daily development. Hopefully with a free-for-all on-premise hosting solution this dream is a step closer.

## Setting it up your own symbol and source server

Have a look at Xavier Decoster's [great, illustrated guide to setting up SymbolSource.Server.Basic](http://www.xavierdecoster.com/setting-up-your-own-symbolsource-server-step-by-step). It's just a few simply steps, not much more than installing a NuGet package in an empty ASP.NET MVC project.

## SymbolSource.Server.Basic versus SymbolSource

At the core, SymbolSource.Server.Basic uses the same processing as hosted SymbolSource does, so it should handle packages equally well. It's just a scaled down, integrated version, without the web interface, cloud storage support, etc.

We are still providing [private feeds and company accounts](Pricing) at SymbolSource.org for anyone interested.

## Schedule/Roadmap/Direction

SymbolSource.Server.Basic is open-source, so anyone is welcome to send pull requests for bug fixes and new features. Source code is available at GitHub: [SymbolSource.Community](http://github.com/SymbolSource/SymbolSource.Community), together with our other open-source projects, including the [NuGet Package Explorer](http://npe.codeplex.com) plugin, which we highly recommend.

The Basic edition will continue to be developed in sync with SymbolSource, as they share some core modules. Our next goal will be to release the commercial edition of the full SymbolSource suite.