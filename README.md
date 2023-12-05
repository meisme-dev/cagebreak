# Cagebreak: A Wayland Tiling Compositor

[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/6532/badge)](https://bestpractices.coreinfrastructure.org/projects/6532) [![Packaging status](https://repology.org/badge/tiny-repos/cagebreak.svg)](https://repology.org/project/cagebreak/versions) [![AUR package](https://repology.org/badge/version-for-repo/aur/cagebreak.svg?minversion=2.2.3)](https://repology.org/project/cagebreak/versions)

## Quick Introduction

Cagebreak is a [Wayland](https://wayland.freedesktop.org/) tiling compositor
based on [Cage](https://github.com/Hjdskes/cage) and inspired by [ratpoison](https://www.nongnu.org/ratpoison/).

### Purpose

This project provides a successor to ratpoison for Wayland.
However, this is no reimplementation of ratpoison.

#### New Features, Bugs and Contact Information

You can [open an issue](https://github.com/project-repo/cagebreak/issues/new)
or write an e-mail (See [SECURITY.md](SECURITY.md) for details.).

The Roadmap section outlines our plans.

#### Compatibility & Development Distribution

Cagebreak supports [Arch Linux](https://archlinux.org/) and uses the libraries
and versions from extra and core at the time of release.
Most other setups work with a bit of luck.

### Quick Installation

This assumes Arch Linux:

1. Use the [cagebreak PKGBUILD](https://aur.archlinux.org/packages/cagebreak).
2. Add an example config such as [config](examples/config) to `$USER/.config/cagebreak/config`
3. Execute cagebreak like any other binary.

See the [ArchWiki](https://wiki.archlinux.org/title/Cagebreak#Getting_started) for
details on getting started and the documentation for everything else.

### Documentation

  * the man pages: [cagebreak](man/cagebreak.1.md), [configuration](man/cagebreak-config.5.md) & [socket](man/cagebreak-socket.7.md)
  * the [README](README.md), [FAQ](FAQ.md) & [SECURITY.md](SECURITY.md)

#### What's new?

Check the [Changelog](Changelog.md).

### Uninstallation

`pacman -R cagebreak` should be sufficient.

### Contributing

  * [Open an issue](https://github.com/project-repo/cagebreak/issues/new) and state your idea.
    We will get back to you.
  * Ask before you open a pull request. We might not accept your code and
    it would be sad to waste the effort.
  * Respect the [Code of Conduct](CODE_OF_CONDUCT.md) (To date, we never
    had to intervene - Keep it that way!)

### Name

Cagebreak is based on [Cage](https://github.com/Hjdskes/cage), a Wayland kiosk
compositor. Since it breaks the kiosk into tiles the name
Cagebreak seemed appropriate.

## Installation

On Arch Linux, just use the PKGBUILDs from the [AUR](https://aur.archlinux.org/):

  * Using [cagebreak](https://aur.archlinux.org/packages/cagebreak), Cagebreak is
    compiled on the target system (since release 1.3.0)
  * Using [cagebreak-bin](https://aur.archlinux.org/packages/cagebreak-bin),
    the pre-built binaries are extracted to
    appropriate paths on the target system (since release 1.3.2)

See [cagebreak-pkgbuild](https://github.com/project-repo/cagebreak-pkgbuild) for details.

### Obtaining Source Code

There are different ways to obtain cagebreak source:

  * [git clone](https://github.com/project-repo/cagebreak) (for all releases)
  * [download release asset tarballs](https://github.com/project-repo/cagebreak/releases) (starting at release 1.2.1)

#### Verifying Source Code

There are corresponding methods of verifying that you obtained the correct code:

  * our git history includes signed tags for releases
  * release assets starting at release 1.2.1 contain a signature for the tarball

## Building Cagebreak

You can build Cagebreak with the [meson](https://mesonbuild.com/) build system. It
requires wayland, wlroots and xkbcommon to be installed. Note that Cagebreak is
developed against the latest tag of wlroots, in order not to constantly chase
breaking changes as soon as they occur.

Simply execute the following steps to build Cagebreak:

```
$ meson setup build
$ ninja -C build
```

#### Release Build

By default, this builds a debug build. To build a release build, use `meson
setup build --buildtype=release`.

##### Xwayland Support

Cagebreak comes with compile-time support for XWayland. To enable this,
first make sure that your version of wlroots is compiled with this
option. Then, add `-Dxwayland=true` to the `meson` command above. Note
that you'll need to have the XWayland binary installed on your system
for this to work.

##### Man Pages

Cagebreak has man pages. To use them, make sure that you have `scdoc`
installed. Then, add `-Dman-pages=true` to the `meson` command.

### Running Cagebreak

You can start Cagebreak by running `./build/cagebreak`. If you run it from
within an existing X11 or Wayland session, it will open in a virtual output as
a window in your existing session. If you run it in a TTY, it'll run with the
KMS+DRM backend. Note that a configuration file is required. For more
configuration options, see the man pages.

Please see `example_scripts/` for example scripts and a basis to customize
from.

#### Usage Philosophy

Cagebreak was originally built to suit the needs of its creators. This section outlines
how we intended some parts of cagebreak and might ease learning how to use cagebreak a
little bit. Please note that this does not replace the man pages or the FAQ.
Also, this is in no way intended as a guide on how cagebreak must be used but rather
as a source of inspiration and explanations for certain particularities.

1. Cagebreak is keyboard-based. Everything regarding cagebreak can be done
   through the keyboard and it is our view that it should be. This does not mean
   that pointers, touchpads and such are not available for the few applications
   that do require them.

2. Cagebreak is a tiling compositor. Every view takes up as much screen space
   as possible. We believe this is useful, as only very few programs are typically
   necessary to complete a task. To manage multiple tasks concurrently, we use
   workspaces.

3. Each task deserves its own workspace. Any given task (the sort of thing you
   might find in your calendar or on your todo list) probably requires very few
   views and ideally, these take up as much of the screen as possible.

> Combining 2. and 3. might look like this in practice:

>  * Task 1: Edit introduction section for paper on X
>  * Task 2: Coordinate event with person Y

>  * split screen vertically
>  * open web browser or pdf viewer to read literature
>  * focus next
>  * open editor
>  * change to a different workspace
>  * split screen vertically
>  * open calendar application
>  * focus next
>  * open chat application

> Now each task has its own workspace and switching between tasks is possible
> by switching between workspaces.

> Note that, for example by using the socket, more advanced setups are
> possible. But the user is warned that excessive tweaking eats into the work
> to be done.

4. Use keybindings and terminal emulators for the right purpose. Given the
   philosophy outlined above you probably launch the same few programs very
   often and others are very rarely used. We believe that commonly
   used programs should have their own keybindings together with the most
   important cagebreak commands. All the rarely used programs should be launched
   from a terminal emulator as they probably require special flags, environment
   variables and file paths anyway.

> In practice this means thinking about the applications and cagebreak commands
> you use and taking your keyboard layout into account when defining keybindings for
> your individual needs.

5. Cagebreak can't do everything, but with scripting you can do most things.
   Through the socket and with a bit of scripting, you can use the internal state
   of cagebreak in combination with cagebreak commands and the full power of
   a scripting language of your choice to do almost whatever you want.

> Example scripts can be found in the repository under `example_scripts/`.

## Contributing

  * Read this document.
  * [Open an issue](https://github.com/project-repo/cagebreak/issues/new) and state your feature request.
    We will get back to you.
  * Ask before opening a pull request. We might not accept your
    code and it would be sad to waste the effort.
  * Respect the [Code of Conduct](CODE_OF_CONDUCT.md) (To date, we never
    had to intervene - Please keep it that way!)

### Good First Contributions

  * Reviews are always welcome.
    * Read the code.
    * Read the documentation.
    * Test whether the documentation matches the code.
    * Test Cagebreak in more esoteric setups (many monitors, for instance).
    * Compile the code.
  * Ideas on improving the testing and quality assurance are particularly
    welcome.
  * You can share your cagebreak scripts and we might include them with Cagebreak
    provided you agree to release them under MIT and we agree with the
    use case and coding style.
  * If you are happy with Cagebreak under Arch Linux, you may vote for
    [Cagebreak in the AUR](https://aur.archlinux.org/packages/cagebreak).
  * The points from the Contributing section above still apply.

### Philosophy

Cagebreak is currently developed to fit the needs of its creators.

The feature set is intentionally limited - we removed
support for a desktop background image for example.

Nonetheless, don't be intimidated by any other part of this file.
Do your best and we will collaborate toward a solution.

## Development

### Compatibility & Development Distribution

Cagebreak supports [Arch Linux](https://archlinux.org/) and uses the libraries
(and software versions) as they are obtained through [pacman](https://wiki.archlinux.org/title/Pacman)
at the time of release. Any other use is out of scope.

However, Cagebreak may also work on other distributions given the
proper library versions (Some package maintainers have done this and it
seems to work (To date, we dealt with a few Issues and never felt the
need to ask for the distribution the user was having the issue on.)).

[![Packaging status](https://repology.org/badge/vertical-allrepos/cagebreak.svg)](https://repology.org/project/cagebreak/versions)

You should use Arch Linux if you want to modify Cagebreak
for yourself.

### Review Requirements

Project-repo will review your proposal before your implementation for feasibility
and desirability. After your pull request, the code will be reviewed in conjunction
with all other changes before the release as per the release procedure.

All reviews performed by project-repo are verified by at least two people internally.

### Developer Certificate of Origin (DCO)

On any pull requests please include a

```
signed-off-by: YOUR IDENTIFIER OR NAME
```

DCO statement.

By doing this you claim that you are legally allowed to contribute the
code and agree to let project-repo publish it under the MIT License.

#### Development Environment

CAVEAT: This script works exclusively on Arch Linux, which, as outlined above,
is the development distribution of Cagebreak.

Cloning the Cagebreak repository and building it is sufficient as a starting point.

All other dependencies can be installed by invoking

```
meson compile devel-install -C build
```

if meson is already available or

```
./scripts/install-development-environment
```

otherwise.

#### Scripts

Cagebreak provides a few convenience tools to facilitate development.

##### Fuzzing

If your fuzzing corpus is located in the directory `fuzz_corpus` you can
just call:

```
meson compile fuzz -C build
```

If you want to use a different directory, configure cagebreak with
`-Dcorpus=OTHERDIRECTORY` or call `./scripts/fuzz OTHERDIRECTORY`.

##### Adjusting Epoch

To facilitate the creation of reproducible man pages an arbitrary release
time has to be set in `meson.build`:

```
meson compile adjust-epoch -C build
```

or

```
./scripts/adjust-epoch
```

##### Git tag

If you are on the master branch, everything is ready and you want to create
a release tag you can call:

```
meson compile git-tag -C build
```

If you want to use another signing key than the prespecified one, configure
Cagebreak with `-Dgpg_id=GPGID`.

```
./scripts/git-tag GPGID CBVERSION
```

can be used alternatively.

##### Output Hashes

Hashes of release versions of all binaries can be output to `local-hashes.txt`
via:

```
meson compile output-hashes -C build
```

Or

```
./scripts/output-hashes VERSION
```

if meson is unavailable.

##### Create Signatures

Creation of signatures for releases can be achieved through:

```
meson compile create-sigs -C build
```

Configure Cagebreak with `-Dgpg_id=GPGID` for a different gpg signing
key.

Without meson use:

```
./scripts/create-signatures GPGID
```

##### Set Version Number

Once the version number is set within meson.build, you can use

```
meson compile set-ver -C build
```

to set the version number in the man pages and README repology minversion.

Use of the script without meson is discouraged because meson.build is
not touched by the script.

##### Create Release Artefacts

The following command generates the release artefacts which must be created
once a release is completely ready to be published (the commit is tagged with
the version of the master branch, etc.):

```
meson compile create-artefacts -C build
```

Use of the script version is discouraged.

### GCC and -fanalyzer

Cagebreak should compile with any reasonably new gcc or clang. Consider
a gcc version of at least [10.1](https://gcc.gnu.org/gcc-10/changes.html) if
you want to get the benefit of the brand-new
[-fanalyzer](https://gcc.gnu.org/onlinedocs/gcc/Static-Analyzer-Options.html)
flag. However, this new flag sometimes produces false-postives and we
selectively disable warnings for affected code segments as described below.

Meson is configured to set `CG_HAS_FANALYZE` if `-fanalyzer` is available.
Therefore, to maintain portability, false-positive fanalyzer warnings are to be
disabled using the following syntax:

```
#if CG_HAS_FANALYZE
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "WARNING OPTION"
#endif
```
and after

```
#if CG_HAS_FANALYZE
#pragma GCC diagnostic pop
#endif
```

### Test Suite

```
meson test -C build
```

invokes all tests. This is required for a release to occur.

There are four test suites:

  * basic: tests actual outward-facing functionality
    * Note optional dependencies for efficient socket interaction
      * nc (openbsd-netcat)
      * jq
  * devel: tests internal properties of the repository
    * Note potentially heavier dependencies such as
      * shellcheck
      * clang-format
  * devel-long: applies more costly testing
    * Note potentially heavier dependencies such as
      * scan-build (static analysis (including security-relevant issues))
  * release: tests release specific considerations
    * Note that this is only expected to pass just before
      a release. This checks mostly administrative things
      to check that a release is ready.
    * Note that non-auto tests are files in `release-non-auto-checks`
      and have to contain the release version and current date in
      YYYY-mm-dd format on seperate lines. This is our imperfect attempt
      to guarantee some hard-to-automate checks are carried out before
      a release is undertaken.

Every commit should pass at least the basic and devel suites.

It is expected that cagebreak passes at least the
basic, devel and devel-long suites when commits are pushed:

```
meson test -C build --suite basic --suite devel
```

The basic suite can be used to test a binary. This is
useful for PKGBUILDs and their equivalents in other
systems.

```
meson test -C build --suite basic
```

### Fuzzing

Along with the project source code, a fuzzing framework based on `libfuzzer` is
supplied. This allows for the testing of the parsing code responsible for reading
the `cagebreak` configuration file. When `libfuzzer` is available (please
use the `clang` compiler to enable it), building the fuzz-testing software can
be enabled by passing `-Dfuzz=true` to meson. This generates a `build/fuzz/fuzz-parse`
binary according to the `libfuzzer` specifications. Further documentation on
how to run this binary can be found [here](https://llvm.org/docs/LibFuzzer.html).

Here is an example workflow:

```
rm -rf build
CC=clang meson setup build -Dfuzz=true -Db_sanitize=address,undefined -Db_lundef=false
ninja -C build/
mkdir build/fuzz_corpus
cp examples/config build/fuzz_corpus/
WLR_BACKENDS=headless ./build/fuzz-parse -jobs=12 -max_len=50000 -close_fd_mask=3 build/fuzz_corpus/
```

You may want to tweak `-jobs` or add other options depending on your own setup.
We have found code path discovery to increase rapidly when the fuzzer is supplied
with an initial config file. We are working on improving our fuzzing coverage to
find bugs in other areas of the code.

#### Caveat

Currently, there are memory leaks which do not seem to stem from our code but rather
the code of wlroots or some other library we depend on. We are working on the problem.
In the meantime, add `-Db_detect-leaks=0` to the meson command to exclude memory leaks.

### Reproducible Builds

Cagebreak offers reproducible builds given the exact library versions specified
in `meson.build`. Should a version mismatch occur, a warning will be emitted. We have
decided on this compromise to allow flexibility and security. In general we will
adapt the versions to the packages available under Arch Linux at the time of
release.

There are reproducibility issues up to and including release `1.2.0`. See
`Issue 5` in [Bugs.md](Bugs.md).

#### Reproducible Build Instructions

All hashes and signatures are provided for the following build instructions.

```
meson setup build -Dxwayland=true -Dman-pages=true --buildtype=release
ninja -C build
```

#### Hashes for Builds

For every release after 1.0.5, hashes will be provided.

For every release after 1.7.0, hashes will be provided for man pages too.

See [Hashes.md](Hashes.md)

#### GPG Signatures

For every release after 1.0.5, a GPG signature will be provided in `signatures`.

The current signature is called `cagebreak.sig`, whereas all older signatures
will be named after their release version.

Due to errors in the release process, the releases 1.7.1 and 1.7.2 did not include the release
signatures in the appropriate folder of the git repository. However, signatures were provided
as release-artefacts at the time of release. The signatures were introduced into the
repository with 1.7.3. The integrity of cagebreak is still the same because the signatures were
provided as release-artefacts (which were themselves signed) and the hashes in Hashes.md
are part of a signed release tag.

#### Signing Keys

All releases are signed by at least one of the following collection of
keys.

  * E79F6D9E113529F4B1FFE4D5C4F974D70CEC2C5B
  * 4739D329C9187A1C2795C20A02ABFDEC3A40545F
  * 7535AB89220A5C15A728B75F74104CC7DCA5D7A8
  * 827BC2320D535AEAD0540E6E2E66F65D99761A6F
  * A88D7431E5BAAD0B6EAE550AC8D61D8BD4FA3C46
  * 8F872885968EB8C589A32E9539ACC012896D450F
  * 896B92AF738C974E0065BF42F2576BD366156BB9
  * AA927AFD50AF7C6810E69FE8274F2C605359E31B
  * BE2DED372287BC4EB2213E13A0C743848A638955
  * 0F3476E4B2404F95EC41600683D5810F7911B020
  * 4E82C72C6B3E58A7BC4FF8554909F84CA83BB867
  * 5AEB1A2EB0D13F67E306AC59DC0CC81BE006FD85

Should we at any point retire a key, we will only replace it with keys signed
by at least one of the above collection.

We registered project-repo.co and added mail addresses after release `1.3.0`.

We now have a mail address and its key is signed by signing keys. See [SECURITY.md](SECURITY.md)
for details.

The full public keys can be found in `keys/` along with any revocation certificates.

### Versioning & Branching Strategy

Cagebreak uses [semantic versioning](https://semver.org).

There are three permanent branches in cagebreak:

  * `master` (for releases)
  * `development` (for polishing code between releases)
  * `hotfix` (for small emergent releases, usually up-to-date with master)

Releases are merged to master as per the release procedure, with
reasonable exceptions as the situation requires.

The release commit is tagged with the release version.

In the past, our git history did not perfectly reflect this scheme.

### Release Procedure

The release procedure outlines the process for a release to occur.

  * [ ] `git checkout development`
  * [ ] `git pull origin development`
  * [ ] `git push origin development`
  * [ ] Arch Build System is up to date
  * [ ] `meson test -C build/` just to get an overview
  * [ ] Update internal wiki
  * [ ] Adjust version number in meson.build
  * [ ] `meson compile set-ver -C build`
  * [ ] Add new files to meson.build or hardcoded testing variable
  * [ ] Commit changes
  * [ ] `git push origin development`
  * [ ] Complete relevant documentation
    * [ ] New features
      * [ ] tests added and old test scripts adjusted
      * [ ] man pages
        * [ ] cagebreak
        * [ ] cagebreak-config
        * [ ] cagebreak-socket
        * [ ] example config
      * [ ] FAQ.md
      * [ ] Changelog.md for major and minor releases but not patches
    * [ ] Check changes for SECURITY.md relevance (changes to socket scope for example)
      * [ ] Synchronize any socket changes to cagebreak-socket man page
    * [ ] Document fixed bugs in Bugs.md
      * [ ] Include issue discussion from github, where applicable
  * [ ] `meson compile adjust-epoch -C build`
  * [ ] Commit changes
  * [ ] `git push origin development`
  * [ ] Testing
    * [ ] Manual testing
    * [ ] `meson compile fuzz -C build` for at least one hour
  * [ ] Adjust Hashes.md - Use `meson compile output-hashes -C build` to add Hashes or aid in repro check
  * [ ] Commit changes
  * [ ] `git push origin development`
  * [ ] Complete release-non-auto-checks
  * [ ] `meson compile create-sigs -C build`
  * [ ] Commit and push signatures, hashes and non-auto-check files
  * [ ] `meson test -C build` passes everything except some release tests
  * [ ] `git add` relevant files
  * [ ] `git commit`
  * [ ] `git push origin development`
  * [ ] `git checkout master`
  * [ ] `git merge --squash development`
  * [ ] `git commit` and insert message
  * [ ] `meson compile git-tag -C build`
  * [ ] `meson compile create-artefacts -C build`
  * [ ] `meson test -C build` THIS MUST PASS WITHOUT ANY FAILURES WHATSOEVER
  * [ ] `git push --tags origin master`
  * [ ] `git checkout development` (merge to development depends on whether release was a hotfix)
  * [ ] `git merge master`
  * [ ] `git push --tags origin development`
  * [ ] `git checkout hotfix` (hotfix is to be kept current with master after releases)
  * [ ] `git merge master`
  * [ ] `git push --tags origin hotfix`
  * [ ] Upload archives and signatures as release assets
  * [ ] Delete feature branches if appropriate
  * [ ] Manage package release

## Roadmap

Cagebreak plans to do or keep doing the following things
in the future:

  * React to all issues.
  * Add or modify features, which the authors find convenient or important.
  * Improve the [OpenSSF Best Practices Badge Program](https://bestpractices.coreinfrastructure.org/en) level

## Governance

Cagebreak is managed by project-repo.

Project-repo is a pseudonym of at least two individuals acting
as benevolent dictators for the project by the others mutual consent.

The individuals comprising project-repo are not otherwise associated
by payment from any organisation or grant.

For all intents and purposes consider project-repo as a single
benevolent dictator for life that happens to occupy at least two brains.

### Roles

There are members of project-repo and those who are not.

There are no specific roles forced unto anyone.

### Bus Factor

The Bus Factor is a measure of how many people have to be
incapacitated for a project to be unable to continue.

The current bus factor for Cagebreak is: 1

Project-repo could still react to issues (even confidential
e-mails) and fix easier issues if any one individual were
incapacitated.

However, not all aspects of the code or release engineering
are fully resilient to the loss of any one individual.

We strive to improve the Bus Factor to at least two in all
aspects of Cagebreak.

### Governance Issues

Anyone can use the information in [SECURITY.md](SECURITY.md) to
contact the members of project-repo and bring governance
issues to their attention.

## Bugs

For any bug, please [create an issue](https://github.com/project-repo/cagebreak/issues/new) on
[GitHub](https://github.com/project-repo/cagebreak).

Fixed bugs are to be assigned a number and summarized inside Bugs.md for future reference
independent of github, in case this service is unavailable.

For other means of contacting the Cagebreak authors and for security issues
see [SECURITY.md](SECURITY.md).

## Accessibility

  * We use text input/output to interact with the user whenever possible. For
    example, sending text-based commands to the cagebreak sockets allows
    one to change every configurable feature of cagebreak.
  * Color is displayed but never a vital part to operating cagebreak.
  * Text size can be increased and background color adjusted using text commands.
  * There is no screen reader support per se but using a screen reader on socket output
    would work and cagebreak does not preclude the use of a screen reader
    for any software run with it.

## Contributors

  * Aisha Tammy
    * [make man pages optional](https://github.com/project-repo/cagebreak/pull/4), released
      in 1.6.0 with slight modifications
  * Oliver Friedmann
    * [Add output scaling](https://github.com/project-repo/cagebreak/pull/34), released
      in 2.0.0 with slight modifications
    * [Fix: calibration matrix](https://github.com/project-repo/cagebreak/pull/49),
      released in 2.2.1 with slight modifications
  * Tom Greig
    * Fix bug in merge_output_configs in 2.1.2

## License

Copyright (c) 2020-2023 The Cagebreak authors
Copyright (c) 2018-2020 Jente Hidskes
Copyright (c) 2019 The Sway authors

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
