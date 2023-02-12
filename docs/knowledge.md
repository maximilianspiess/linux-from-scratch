# Knowledge

**This file contains all information that we've collected about Linux.**

-----

## Summaries

---

### Tarred software packages

Linux source software packages most often come in an **archived** (*tarred*) and **compressed** (*gzipped*) format, the file extension being .tar.gz or .tgz, which in the jargon is known as a "tarball". Depending on the compression algorithm used, the final section of the file extension may differ. Most any of these packages can be unpacked using:

`tar tzvf <filename>` or (equivalent) `gunzip -c <filename> | tar xvf -`

or if the package as archived using bzip2 instead of gzip:

`tar xyvf <filename>` or (equivalent) `bzip -cd <filename> | tar xvf -`

While untarring, a package usually checks if it itself is located in the correct directory for installation, among other relevant factors.

> ***NOTE:*** If the untarring process returns an error, it may be because of the package location. Check the **README** or **Install** file, if present.

Usually, any necessary changes to **config files** or **Makefiles** will be referenced in the installation instructions. Most packages however will contain an **Imake** file, which is a sort of template from which corresponding Makefiles are generated. They allow the user to automate the installation process by simply typing:

`make`

On occasion, `shar` files, or **shell archives** may also be encountered. These are human readable and are decompressed as described above, and can be unarchived using:

`unshar <filename>`

---

### Make

Make is a general-purpose software building and automation tool. Makefiles are grouped into directives, which `make` calls tasks. Tasks can be executed individually by typing:

`make [task_name]`

Following are some tasks bound by convention:

- `clean` - removes all free-floating binary files
- `install` - moves compiled binaries to corresponding locations

> ***NOTE:*** The `install` directive will often have to be run as a super user, since files are moved to and potentially from system directores such as `/bin`.

Running `make` without a task argument will run a task named `all`, which will usually execute all necessary steps for proper installation.

As mentioned above, in production, Makefiles are rarely written or edited manually, instead relying on an IMakefile to automatically generate them for the target system. For this purpose, a shell script called `xmkmf` is put at the user's disposal. If an **Imake** file is present after dearchiving, usually the user is expected to run the bundled script with:

`xmkmf -a`

Instead, an **INSTALL** or a **configure** script may be present. The exact procedure can vary from one package to another and will be described in the **README** or **Install** file.

>***NOTE:*** Root permissions should not be necessary for compiling the binaries, only for moving them to their respective directories. For tools and scripts which do all in one command, the question on whether to execute as super user should answer itself.

---

### rpm > tarball?

#### Package managers

Package managers such as *rpm*, *deb* or *slp* (such files are henceforth referred to as **rpm**s) provide a similarly simple installation procedure as the Install Wizard used by Windows. While an rpm can contain any type of file, they are more often than not filled with **prepackaged and precompiled binary files**, which are simply put into their directories.

The big selling point of this mode of software distribution is the entirely **automated installation** procedure, followed directly by **dependency resolution** and mandatory **malware scans** provided by package managers, which all hugely simplify the process for end-users.

#### Tarball releases

The other main source of software are tarballs. Usually, they are the only reasonable way to acquire **sourcecode** for a given application. Everything about a tarball is more **flexible** than their prepackaged counterpart, as every step from decompression to compilation and over to installation can be manually adjusted.

This of course comes with more effort for the user, but should you find yourself having to debug, say, a deprecated library, there is no other plausible way.

That being said, rpm distributables are not made as easily as just bundling up some binaries. The process can be so laborious that, for smaller software especially, there may not even be a managed instance in a package manager, leaving tarballs as the only option. This also applies to other software, so always expect **rpms** to **lag behind tarball** releases.

---

### UNIX Links

Predominantly present on UNIX-Systems, links generally represent references to directories, files, or a file's contents, similar to a shortcut in Windows systems. 

There is one important concept to be understood here: a file path in UNIX is not a unique identifier to a given file, but instead, a file is ultimately labeled by a number called the **inode**, or the **index node**. The inode number of a file can be displayed with

`ls -i`

and the total number of references to an inode is the column between access rights and file owners using

`ls -l`

This means that files are entities separate from file paths, so in theory, multiple file paths can point to the same file. There are two primary types of links; hard links and soft links.

#### Hard links

Hard links connect a filename or filepath to an actual file in memory, which are inodes. They are created either when a new file is created, using `touch` for example, or manually using

`ln <src_path> <new_path>`

Hard links pointing to the same files will share the same inode identifier, and therefore both the file's content as well as it's attributes, such as access rights, owners, date of creation etc. They are not copies however, but links, and changing one will affect the inode, and thus all other hard links. A file is only truly deleted once all hard links and processes using them have been erased. Attempting to manually delete files via inodes will usually result in the host filesystem breaking.

That being said, inode numbers are system-specific, and so hard links cannot be used to access files on other systems. Also, hard links can only be associated to regular files, so directories or special files cannot be linked this way.

#### Soft links

Soft links, also known as **symbolic links**, or abbreviated **symlink**, are a special file type, which point to another existing file. They contain a file path, either relative or absolute. This means that they can both be used to point to files on another system, assuming the established file structure is identical. In addition, they can also point to any type of file, including special files, such as directories, devices, and other symlinks.

Soft links can be created using

`ln -s <src_path> <new_path>`

In stark contrast to hard links, the objects pointed to by soft links can easily be deleted, breaking the soft link. Such a link is called a *dangling soft link*. If a new file is created in the old one's stead, the symlink will once again have a valid target, whether intentional or not.

#### tl;dr

inodes:
- are file system specific
- identify a file in memory uniquely
- are only removed once all references are erased

Hard links:
- point to files via inodes
- several can point to one inode

Soft links:
- point to files via filepaths
- can be used cross-system

#### Sources

- [Hard links and soft links in Linux](https://www.redhat.com/sysadmin/linking-linux-explained)

---

### Special files

-----

## Links to guides and other useful stuff

- [Building and Installing Software Packages for Linux](https://tldp.org/HOWTO/Software-Building-HOWTO.html)
- [Beginner's Guide to Installing from Source](http://moi.vonos.net/linux/beginners-installing-from-source/)
