1. Fiddler shows symbols being downloaded but Visual Studio does not load them
* check write access to the symbols cache directory.

1. Downloading symbols does not work when debugging Silverlight projects
* either [try disabling RequireFullTrustForSourceServer in the registry](http://stackoverflow.com/questions/3739079/vs2010-debugging-sl-4-cant-load-source-code-from-source-server)
* or upgrade to Visual Studio 2012 and select *Allow source server for partial trust assemblies*

