[![CircleCI](https://circleci.com/gh/weavenet/bashdot/tree/master.svg?style=svg)](https://circleci.com/gh/weavenet/bashdot/tree/master)

# Summary

I am **bashdot**.

I am a minimalist dotfile management framework focused on supporting multiple
profiles, providing different configurations based on the profiles installed
on a given system.

I am written **100% in bash**, require **no dependencies** outside of bash, and am [tested](https://circleci.com/gh/weavenet/bashdot/tree/master) using [bats](https://github.com/sstephenson/bats).

## Overview

Bashdot works by symlinking all files and directions in **profiles**, within
the current directory where bashdot is run, to files in the users home.

One or more profiles can be installed on a specific computer to provide
the desired dotfiles for it's purpose (work, home, etc.), operating
system (Linux, MacOS, Solaris, etc.) and version (Debian, RedHat, etc.).

Using a combinations of profiles, you can remove conditional logic from your bash
scripts. For example, create a linux profile for Linux specific commands or
aliases, or an identity profile for those specific to the identity organization. Only
install what you need on a given system, at a specific time.

## Quick Start

To setup your own bashdot managed dotfiles:

* Install Bashdot

MacOS Homebrew

```
brew tap weavenet/tap
brew install bashdot
```

Manual Installation

```
cd $TMPDIR
curl -s https://codeload.github.com/weavenet/bashdot/tar.gz/2.0.0 > bashdot-2.0.0.tar.gz
tar xf bashdot-2.0.0.tar.gz
sudo cp bashdot-2.0.0/bashdot /usr/local/bin
chmod a+x /usr/local/bin/bashdot
```

* Clone the starter bashdot profiles repo

```
git clone https://github.com/weavenet/bashdot_profiles
```

* Change into the bashdot_profiles directory and run the below command to setup the
**default** and **home** profiles on this instance.

```
cd bashdot_profiles
bashdot install default home
```

## Managing Multiple Profiles

Bashdot works by symlinking files within the given profile directory into your home directory.

For example, if you run:

```
bashdot install default work
```

Bashdot will symlink all the files in the default and work directories within profiles
into your home directory while prepending a "period".

The above command would create the following symlinks:

```
lrwxrwxrwx 1 brett brett   28 Mar  8 09:03 .bashrc -> /brett/bashdot/profiles/default/bashrc
lrwxrwxrwx 1 brett brett   40 Mar  8 09:03 .profilerc_work -> /brett/bashdot/profiles/work/profilerc_work
```

You can then make changes to files in the **default** or **work** profiles, or
add additional profiles as necessary.  If you use different files in different
environments you can create a profile for each environment with the appropriate dotfiles.

Since the files are symlinked into your home directory, if you keep the bashdot directory
on a shared drive, changes to files on one instance will automatically be reflected on all
instances with that profile installed.

The default **.bashrc** will load anything in a file prepended with **profilerc** to
allow for running shell commands for that profile's setup. See
[profiles](https://github.com/weavenet/bashdot/tree/master/profiles)
for examples of profiles with different variables initialized.

## Frequently Asked Questions

**Q:** Does bashdot support templates or user input for configuring dotfiles during installation?

**A:** No, if you have different configurations of a single dotfile, bashdot handles that
by supporting multiple profiles. For example if you want to setup differnet **.gitconfig**
for your **home** and **work** environments, create a home and work profile, and within each of
those profiles add the appropriate gitconfig, then use bashdot to install the approriate profile
on each system.

**Q:** What if I have secrets or other private information to install in my dotfile?

**A:** Never check in sensitive information in your dotfiles. To remove sensitive information,
either a) pull that information from an external system or b) encrypt it and read the decryption
key from a location not in your dotfiles. See [here](https://gist.github.com/weavenet/f3af28350f07176674a5474b2d891102) for examples.

**Q:** How can I share my bashdot profiles?

**A:** Bashdot only manages dotfiles installation, not their distribution. To share your
bashdot profile, make it available via source control or a file share, then consumers can
download them locally to install. For example to install the public profiles from a git repo:

```
git clone https://github.com/weavenet/bashdot_public_profile
cd bashdot_public_profile && bashdot install public
```

**Q:** Does bashdot work with zsh?

**A:** Yes

## Bashdot Development

### Test

Only requirement to run tests is [docker](https://docs.docker.com/install/). Once installed run:

```
make test
```
