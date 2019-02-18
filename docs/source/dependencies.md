# How to deal with dependencies

Dependencies in Leapp are different from other RPM packages, so to work with dependencies in Leapp, you need to first know why it is different from other packages.

## Why do I need to have a special way to define a dependency

One of the uses of the Leapp framework is for do the in-place upgrade, in that case, is expected Leapp run in two Linux version, the old one and the upgraded version. To ensure RPM will keep dependencies of Leapp in upgrade process you need to add this dependency in a Leapp meta-package.

## How to do that

It's easy to add new dependencies to Leapp, you need to edit the file: `packaging/leapp.spec` and add you dependencies in this section:

```
##################################################
# Real requirements for the leapp HERE
##################################################
...
Requires: findutils
Requires: your-dependency
```

### Some more unusual steps

How it is a meta-package you need to increment in more than one place some variables:

Change `Requires` incrementing by one (exists more than one line with this variable):

`Requires: leapp-framework-dependencies = 1`

Change the `Provides` incrementing by one too (to match with `Requires` value):

`Provides: leapp-framework-dependencies = 1`

Finally you need to update de Changelog.