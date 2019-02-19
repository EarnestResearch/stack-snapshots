# stack-snapshots
A repository to house useful custom stack snapshots, c.f.,https://docs.haskellstack.org/en/stable/custom_snapshot/ and  https://www.fpcomplete.com/blog/2017/07/stacks-new-extensible-snapshots.

The hope is that this will be useful as our internal dependencies grow.

# Usage

In your `stack.yaml` set resolver to

``` yaml
resolver: https://raw.githubusercontent.com/EarnestResearch/stack-snapshots/master/snapshots/er-lts-13.8.0.yaml
```

# Approach

Custom snapshots are derived from [upstream](https://www.stackage.org/) and include additional packages we wish to include
as well as updated versions of official packages (generally bugfixes that are not yet visible upstream).

# Add a new custom snapshot

Review the the status of the latest available, see if any of the customized dependencies got any updates and bump those up or
remove them if the fixes are already upstream. Add a new snapshot by suffixing our patch number at the end of `lts-x.y` official version.

# Motivation
The point of custom snapshots is to turn, e.g., this

``` yaml
# old stack.yaml
resolver: lts-12.7
extra-deps:
- git: git@github.com:brendanhay/amazonka
  commit: 248f7b2a7248222cc21cef6194cd1872ba99ac5d
  subdirs:
    - amazonka
    - core
    - amazonka-s3
- named-2.0
```
into its own snapshot that can be referenced thusly (or even from a URL)

``` yaml
# new stack.yaml
resolver: my-custom-snapshot.yaml
```
where

``` yaml
# my-custom-snapshot.yaml
resolver: lts-12.7
name: my-custom-snapshot.yaml
packages:
- git: git@github.com:brendanhay/amazonka
  commit: 248f7b2a7248222cc21cef6194cd1872ba99ac5d
  subdirs:
    - amazonka
    - core
    - amazonka-s3
- named-2.0
```
In doing so we're saying that we'd like our snapshot to start off from the package set (and GHC version) defined in lts-12.7, and in addition, add the specified packages.
