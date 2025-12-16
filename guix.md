# Guix

[Guix](https://guix.gnu.org/) is the ultimate reproducible package manager. Small changes in software versions, or the underlying libraries, compilers, or even the compilation commands used, can cause surprisingly large differences in the output. To guarantee reproducibility, you need a package manager that is capable of tracking software all the way down the dependency tree. Guix is the **only**[1] package manager that meets this requirement. Conda does not. HPC admins installing software manually also does not qualify since you don't know and therefore cannot exactly reproduce the compilation commands they used, the compilers they used, the libraries they linked to, etc.

[1]: Also [NixOS](https://nixos.org/), a close relative of Guix.

## guix shell

Run `guix shell` followed by a list of package names, and you will be dropped into a subshell in which those packages are installed. When you exit from the subshell, the packages will *disappear*. Here is a demo below for the `samtools` package.
```
$ which samtools
/usr/bin/which: no samtools in (/home/aisaac/.local/bin:/home/aisaac/bin:/home/aisaac/.config/guix/current/bin:/usr/share/Modules/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/puppetlabs/bin)
$ guix shell samtools
[env]$ which samtools
/gnu/store/bi5mwgahwwbana2h305v39fn43h38r57-profile/bin/samtools
[env]$ exit
$ which samtools
/usr/bin/which: no samtools in (/home/aisaac/.local/bin:/home/aisaac/bin:/home/aisaac/.config/guix/current/bin:/usr/share/Modules/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/puppetlabs/bin)
```

The same works for R packages.
```
$ guix shell r r-atacseqqc
[env]$ R
> library(ATACseqQC)
> […]
```

Or, for Python packages! (really, it works for any language)
```
$ guix shell python python-pandas
[env]$ python3
>>> import pandas as pd
>>> […]
```

## Finding package names—guix search

To find package names, run `guix search <search-keywords>`. For example, `guix search wavefront aligner`.

## Building singularity images

In an ideal world, `guix shell` should be all you need. Unfortunately, UCL HPC admins (both CS and myriad) are not *yet* convinced of Guix's value, and won't install it on the cluster. So, we can't simply run `guix shell` on our HPC machines. Fortunately, we can work around this limitation by building singularity images on our Guix machine and copying it over to the HPC machine. For example, to build a singularity image with samtools and a bunch of other common command-line utilities, run
```
guix pack -f squashfs bash coreutils findutils gawk grep sed samtools
```
Then, copy the built singularity image to baddiel or elsewhere using `scp`. These images will work on the cluster too.

You can run commands in the singularity container using `singularity exec`. For example, to run `samtools` from an image `samtools.squashfs`,
```
singularity exec samtools.squashfs samtools --help
```

## Guix channels—when base Guix does not have the packages you need

Base Guix may not always have the packages you need. Additional Guix packages are available in other repositories known as *channels*. Some channels of interest include

- [guix-science](https://codeberg.org/guix-science/guix-science/)
- [guix-science-nonfree](https://codeberg.org/guix-science/guix-science-nonfree)
- [guix-bioinformatics](https://git.genenetwork.org/guix-bioinformatics/about/)

To use all these channels, you'd first put the following in your `~/.config/guix/channels.scm` file.
```scheme
(list (channel
        (name 'guix-science)
        (url "https://codeberg.org/guix-science/guix-science.git")
        (introduction
         (make-channel-introduction
          "b1fe5aaff3ab48e798a4cce02f0212bc91f423dc"
          (openpgp-fingerprint
           "CA4F 8CF4 37D7 478F DA05  5FD4 4213 7701 1A37 8446"))))
      (channel
        (name 'guix-science-nonfree)
        (url "https://codeberg.org/guix-science/guix-science-nonfree.git")
        (introduction
         (make-channel-introduction
          "58661b110325fd5d9b40e6f0177cc486a615817e"
          (openpgp-fingerprint
           "CA4F 8CF4 37D7 478F DA05  5FD4 4213 7701 1A37 8446"))))
      (channel
        (name 'gn-bioinformatics)
        (url "https://git.genenetwork.org/guix-bioinformatics")
        (branch "master"))
      %default-guix-channel)
```
Then, run
```
guix pull
```
After this, when you run guix, you will have access to packages from all these channels.

Later, whenever you wish to update to the latest packages from all your channels, run `guix pull` again.

## When no channel has the packages you need or they are broken

Sometimes, it may so happen that Guix does not have the packages you need or they fail to build at the moment. The beauty of Guix is that, unlike other package managers, you can define and build your packages locally and they are first-class citizens just the same as any packages you fetch from upstream repositories. So, it is always possible to locally define your own packages or fix packages broken upstream. Contributing your local changes upstream, while desirable, is not required. So, you can make rapid progress on your project without having to wait for upstream to review and accept your changes.

Sadly, a description of the packaging process is beyond the scope of this guide. The [packaging tutorial](https://guix.gnu.org/cookbook/en/html_node/Packaging-Tutorial.html) in the Guix cookbook is suggested further reading.

## Further reading

- [Using Guix for Reproducible Research](https://guix.gnu.org/cookbook/en/html_node/Reproducible-Research.html)
