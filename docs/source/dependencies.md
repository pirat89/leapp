# How to deal with dependencies

RPM dependencies of Leapp packages can be grouped into two groups:
1. *inner dependecies* between Leapp-related packages
1. *outer dependencies* on packages not related to Leapp (e.g. python)

The first group is pretty clear and you don't need to care about that so much
anymore. But the *outer dependencies* are really tricky. This point, when
a Leapp-related package need such dependency, it has to depend on special
capability (below) and required dependency has to be set in the special
metapackage - that provides this capability. From this point, the metapackage
installed together with leapp packages has name **leapp-deps**.

The capability (in case of the leapp tool, leapp framework, and snactor) is
called **leapp-framework-dependencies**. Anytime the *outer* dependencies
are changed, value of capability has to be incremented by one - so the
information about change is provided to other leapp related packages.

This metapackage can be simply replaced by different one, that will handle
outer dependencies of the target system. That means, that the metapackage
with dependencies for a target system is not part of this git repository
and it is completely on a leapp repository using the leapp tool (or framework)
to provide such package in case it is needed.


## Why do I need to have a special way to define a dependency

One possible use of the Leapp framework is for do the in-place upgrade (IPU).
In that case it is expected Leapp needs to be executed in two different
versions of system - it starts with the source system and then continues with
the target (upgraded) system. It is quite common that some packages are renamed
between those two systems (or in case of leapp repository, you would like to
use technology on the new system that is not provided on the original one).
In such cases it is hard to satisfy dependencies on both systems - when you
want to proceed the upgrade process with the same Leapp packages.

As the solution we are using the metapackage that contains those dependencies.

## What to do when dependencies needs to be changed

It's easy to change *outer dependencies* for Leapp. Open the
`packaging/leapp.spec` file and change dependencies as it is needed for
`%package deps`. The right place is pretty highlighted (you cannot miss it):

```spec
##################################################
# Real requirements for the leapp HERE
##################################################
...
Requires: findutils
Requires: your-dependency
```

And do not forget to increment value of `leapp-framework-dependencies`:

```spec
# IMPORTANT: everytime the requirements are changed, increment number by one
# - same for Provides in deps subpackage
Requires: leapp-framework-dependencies = 1
```

**IMPORTANT** Do the same thing for all `Requires` and `Provides` of the
`leapp-framework-dependencies` capability in the SPEC file. Ensure that value
is consistent across the SPEC file.

### Some more unusual steps

**TODO** : should be mentioned that metapackage used for for leapp on a target
system should reflect this change and mention that there is now one official
leapp-repository - send link to the correct spec file (there are two, one is for
target deps)

