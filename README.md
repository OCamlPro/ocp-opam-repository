# OCamlPro OPAM Repository

A repository for OPAM packages produced by OCamlPro. This repository is
organized to match different needs depending on usage.

## Usage

Simply add the repository to your OPAM configuration like any other OPAM
repository with the only difference that you should specify a revision :

```sh
opam repo add ocamlpro https://www.github.com/OCamlPro/ocp-opam-repository#<revision> <priority>
```

Where `<revision>` is the revision you want to use (see below) and `<priority>`
the priority of the repository. It is highly recommanded to use an higher
priority than default opam repository to avoid conflicts.

You can then update the repository informations with :

```sh
opam update ocamlpro
```

For example, a typical usage would be :

```sh
opam repo add ocamlpro https://www.github.com/OCamlPro/ocp-opam-repository#stable+linux-2.3.0
opam update ocamlpro
```

As for any OPAM repository, you can then install packages from it with :

```sh
opam install <package>
```

Installing the repository is no revision works but is not recommended as it
points currently to `unstable+linux` by default.

## Structure

The repository has two root branches names :

- `stable`
- `unstable`

The `stable` branch contains packages that are considered stable and mature
enough to be used in industrial context. The `unstable` branch contains
packages that are still under development; they can be used but are not fully
documented nor tested.

Each root branch has a set of sub-branches according to targeted systems:

- `+linux`
- `+windows`
- `+macosx`

And each of these sub-branches may be versionned. For example, the
tag `stable+linux-2.3.0` will contain a precise set (corresponding to the
given version) of stable packages for Linux.

The default `master` branch points to `unstable+linux` branch for legacy
reasons but it is not recommended to use it for it will be likely moved
to `stable+linux` in the future.

## Versionning

Unlike the official OPAM repository, this repository is versionned. This choice
is motivated by ensuring package liveness. By using our repository we garantee
that lastest versions have no dead packages so that users of our repository
can expect a minimum support at least.

## Support

The `stable` branches are actively supported by OCamlPro and comes with a
commercial licence for commercial usage. Personnal or academic usage is free.

The licence gives reactivity, operationnal and security support :

- responses to issues within 3 business days
- bug fixes or workarounds within 5 business days
- security fixes within 3 business days

The `unstable` branches comes with reactivy support only.

## Contributions

Contributions external to OCamlPro are welcome. However, contributions may
come with some specific rules depending on branches.

Contributing to `unstable` branches is open to anyone and only requires
liveness : if no activity is detected on a package with issues or merge/pull
requests for 6 months, the package will be either removed from the next branch
version or taken over by OCamlPro.

Contributing to `stable` branches requires a commercial aggreement that will
de defined on a case by case basis. Depending on the contribution, a commercial
aggreement may be needed in a bonus form or a special
commercial support offer.

Benevolent contributions to `stable` branches are only accepted if
OCamlPro can maintain them. In this particular case, OCamlPro will propose
publicity to the contributor at least.

## Quality

`unstable` packages must compile and pass their testuite.

`stable` packages must compile, pass their testuite and be documented. However,
the code, documentation and testsuite must follow the next requirements.

### Code

Using `drom` is highly recommended. While the tool itself isn't `stable`
compliant, it's not needed to build so it's not an issue. However, it
standardizes the project managment and documentation, which is strongly
appreciable and will likely leverage the quality of the project.

Packages shall provide easily understandable and usable features. They may
be complicated in their implementation but exposed stuff should be usable
by OCaml newcommers. This mainly comes with two rules :

- don't use experimental OCaml features if it doesn't bring any significant
  value.
- Don't use utterly complicated OCaml idioms.

Let's explain the second point : OCaml is a very expressive language and
it's easy to write very complicated code. Functionnal programming is generally
good for many situations but it's not always the best solution. OCaml has the
exceptionnal strength of being multi-paradigm and it's a good thing to use
it. Use what is the most suitable to the problem you're trying to solve and
give simple and clear interfaces of it to the user.

For example, keep regular
exceptions (whith catch all handlers) instead of abusing the `result` type. The
latter seems more elegant but is rarely needed in practice and rely on
optimizations the compiler doesn't always do. Using exceptions or simple error
codes is less elegant but likely more efficient and easier to use and
understand to any OCaml programmers, especially for newcommers accustomed to
this kind of error handling.

Of course, this is not a strict rule and there are many exceptions which will
be discussed on a case by case basis (and probably published in an official
guide in the future).

### Documentation

Documentation must be written in English. However, as the OCaml community
has an important French community, having a French traduction is a plus.

Libraries must have a *clear* API documentation presenting each public
signature with an explaination of each parameters, return value and thrown
exception. Depending on function complexity, an example usage shall be
provided.

Binaries must have a *clear* user manual (and administration one if relevant).
As binaries can be invoked from command line, this interface is *mandatory*
and follows the standard usage (see `man` pages for example).

Binaries must follow the standards of their application scope. For example,
system tools must produce logs usable for common supervision tools. Context
based tools should use a configuration file in a standard human editable format
(like TOML, YAML, ...).

### Testsuites

Testsuites should be split in several kinds:

- unit tests
- functionnal tests
- regression tests

for libraries. For binaries, there is also validation tests.

Unit testing shall be done with inline test frameworks to test functions with
various parameters.

Functional testing for libraries shall be done with inline test frameworks to
test modules or function composition. For binaries, it shall use `clam` like
testing.

Validation tests shall be done with `clam` like testing.

Regression tests shall be done according to the kind of regression (unit,
functionnal or validation).


