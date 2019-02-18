# How to deal with dependencies

One of the uses of the Leapp framework is for do the in-place upgrade, in that case, is expected Leapp run in two Linux version, the old one and the upgraded version. To ensure RPM will keep dependencies of Leapp in upgrade process you need to add this dependency in a Leapp meta-package.

To add new dependencies to the framework, you should proceed as follows:

Edit the file: `packaging/leapp.spec`

Change `Requires` incrementing by one (there are two lines like this):

`Requires: leapp-framework-dependencies = 1`

Now you need to change the `Provides` incrementing by one too to match with `Requires`:

`Provides: leapp-framework-dependencies = 1`

Add you dependencies in this section:

```
##################################################
# Real requirements for the leapp HERE
##################################################
...
Requires: findutils
Requires: your-dependency
```

Finally you need to update de Changelog.