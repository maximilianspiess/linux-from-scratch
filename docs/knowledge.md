# Knowledge

**This file contains all information that we've collected about Linux.**

-----

## Summaries

### Tarred software packages

Linux source software packages most often come in an **archived** (*tarred*) and **compressed** (*gzipped*) format, the file extension being .tar.gz or .tgz, which in the jargon is known as a "tarball". Depending on the compression algorithm used, the final section of the file extension may differ. Most any of these packages can be unpacked using:

`tar tzvf <filename>` or (equivalent) `gunzip -c <filename> | tar xvf -`

or if the package as archived using bzip2 instead of gzip:

`tar xyvf <filename>` or (equivalent) `bzip -cd <filename> | tar xvf -`

While untarring, a package usually checks if it itself is located in the correct directory for installation, among other relevant factors.

> ***NOTE:*** If the untarring process returns an error, it may be because of the package location. Check the `README` or `Install` file, if present.

Usually, any necessary changes to **config files** or **Makefiles** will be referenced in the installation instructions. Most packages however will contain an **Imake** file, which is a sort of template from which corresponding Makefiles are generated. They allow the user to automate the installation process by simply typing:

`make install`

On occasion, `shar` files, or **shell archives** may also be encountered. These are human readable and are decompressed as described above, and can be unarchived using:

`unshar <filename>`


### Make

-----

## Links to guides and other useful stuff

- [Building and Installing Software Packages for Linux](https://tldp.org/HOWTO/Software-Building-HOWTO.html)
- [Beginner's Guide to Installing from Source](http://moi.vonos.net/linux/beginners-installing-from-source/)
