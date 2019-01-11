# stack-snapshots
A repository to house useful custom stack snapshots, c.f.,https://docs.haskellstack.org/en/stable/custom_snapshot/ and  https://www.fpcomplete.com/blog/2017/07/stacks-new-extensible-snapshots.

The hope is that this will be useful as our internal dependencies grow.

# Usage
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
