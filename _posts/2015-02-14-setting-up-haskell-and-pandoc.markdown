---
layout: post
title: "Setting up Haskell (and Pandoc)"
date: 2015-02-14 19:03:42
tags: computers writing haskell programming
---

Today I spent a good amount of time fighting with [Slackware][slack]
to get [Haskell][haskell] installed. Before I forget how I did it, I want to
record the process here... so in the future, it will be easy.

### Getting Ready ###

Before You can install [Haskell][haskell], You need [Haskell][haskell]
installed on Your machine. This means that You need to find a generic,
binary distribution of the compiler (et al) to stand in for the time
being.

You also need [cabal-install][cabal-web], which is used as a part of
the buildchain and as a package installer for [Haskell][haskell].

Finally, You'll need a source distribution of [Haskell][haskell], so
that You can compile it from source.

Here are some links for the most up-to-date versions of the above (for
GNU/Linux, as that is what I use. If You use Windows or OS/X, please
see the main [Haskell-Platform][platform] website.

| [binary-dist][bin86] | [cabal-install][cab86] | [source][src86] |

### Building and Installing ###

1. The first step is to install the binary distribution. I followed
   the [Slackwiki's][slackwiki] instructions to create a package which
   I could easily remove later. I then installed it, using `installpkg`
   as root.
2. Next, You need to put the cabal-install program (which is actually
   just `cabal`) somewhere in Your path. I have a directory
   `~/bins/bin/` where I put all binaries I use on a general basis,
   that I don't need to be installed system wide. Setting that up will
   probably be the topic of another post, which (if and when it is
   written) I will link to __here__.
3. Now that Your build environment is set up, You can extract the
   `haskell-platform` file into a new directory, cd into it, and run
   the following command: `./platform.sh <Bindist Tarball>`. Replace
   '<Bindist Tarball>' with the path to Your previously downloaded
   binary distribution tarball. For instance, I use an x86 distro, and
   so mine was: `./platform.sh
   ~/Downloads/ghc-7.8.3-i386-unknown-linux-centos65.tar.xz`.
4. When that command finishes (it takes a while), You'll have a new
   tarball in the `./build/product/` directory. As a good sysadmin,
   You should take the time to untar it into its own directory, add a
   slack-desc, and package it up. Then, remove the binary
   distribution package, and take `cabal` out of Your path.
5. Now, simply run (as root): `installpkg haskell_platform.txz`. Once
   it installs, the last bit of set up is to run the following
   program: `/usr/local/bin/activate-hs`.

Congratulations, [Haskell][haskell] is installed.


### Cabal and Pandoc ###

To take Your newly installed Haskell and install pandoc, it is really
as simple as: `cabal install pandoc`. Once that finishes, You will
have pandoc fully available to You.

But, as [others][cabalnotpkg] have said, Cabal is not a Package
Manager. Upgrading Pandoc will not be elegant, and uninstalling it
will have to be manual, as cabal-install does not handle either of
those two things well (or at all, really).

I digress, though. That is for another post.


[slack]: http://www.slackware.com " "
[haskell]: https://www.haskell.org/ " "
[cabal-web]: https://www.haskell.org/cabal/ " "
[platform]: https://www.haskell.org/platform/  " "
[bin86]: https://www.haskell.org/ghc/download_ghc_7_8_3 " "
[cab86]: https://www.haskell.org/cabal/release/cabal-install-latest/ " "
[src86]: https://www.haskell.org/platform/linux.html " "
[slackwiki]: http://www.slackwiki.com/Building_A_Package " "
[cabalnotpkg]: https://ivanmiljenovic.wordpress.com/2010/03/15/repeat-after-me-cabal-is-not-a-package-manager/ " "
