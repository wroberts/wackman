# WACKMAN - Will's Package Manager

Wackman is a package manager designed to coordinate builds of packages
from source.  Wackman is also designed to install to a local directory
tree under the user's home directory, for use on systems where the
user does not have root access.

## Design

`prefix` is the path under which the Wackman system is installed.
This defaults to `$HOME/.wackman`.

`prefix/src` is a cache directory where source packages are downloaded
to.  This can be safely emptied at any time.

`prefix/usr`, `prefix/lib`, `prefix/man`, etc. are the directories
containing binaries, libraries, and include files.

The actual installed package contents are stored under
`prefix/wackman/pkgs`.  This path contains one directory for each
installed package on the system (e.g.,
`prefix/wackman/pkgs/bash-4.4.12_1`).  The files under these
directories are then symlinked into the relevant directories under
`prefix`.

Package metadata are stored in `prefix/wackman/Wackfiles`.  These
files record instructions for installation and patching packages to
work on the given system.

Execute `$prefix/initenv.sh` from your `.bash_profile` to set up the
Wackman environment (configure PATH, CPPFLAGS, LDFLAGS, etc.).

## Synopsis

`wackman install bash`
`wackman uninstall bash`

    Install or uninstall a package.  During uninstallation, if the
    name `bash` is ambiguous (e.g., if there are versions 4.4.12 and
    4.4.12_1 installed), the command will fail unless the fully
    qualified name is given (`bash-4.4.12`).

`wackman link bash`
`wackman unlink bash`

    Manually link or unlink an installed package into the main
    `prefix` directories.
    
`wackman dev http://ftp.gnu.org/gnu/bash/bash-4.3.tar.gz`

    Downloads the source archive from the specified URL, guesses the
    name and version of the package, initialises a new `Wackfile` for
    the new pacakge, uncompresses the archive into a temporary
    directory under `prefix/src`, and initialises that directory as a
    git archive.  You can then attempt to install the package
    manually; if the downloaded archive requires edits, these can be
    saved as a patch using git.

## Further Reading

- [nix](https://nixos.org/nix/) - The Purely Functional Package
  Manager.
    - [blog post about this](https://invalidmagic.wordpress.com/2011/01/21/running-the-nix-package-manager-in-a-prefix-as-the-home-directory/)
- [linuxbrew](https://github.com/Homebrew/linuxbrew)
- [the linux port of homebrew](https://github.com/rubiojr/homebrew)
- [pkgsrc](http://www.pkgsrc.org/) - NetBSD's package manager, but
  designed to be portable so that it supports Linux, and can install
  packages without privileges, just like Homebrew.
