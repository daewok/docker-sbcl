- [Supported Tags](#org317f682)
  - [Simple Tags](#orgcbbee6f)
  - [Shared Tags](#orgaeaa946)
- [Quick Reference](#org4d8592d)
- [What is SBCL?](#org07276a6)
- [What's in the image?](#org386b700)
  - [Patches](#orgaafe255)
    - [Removal of `-Wimplicit-fallthrough`](#org18df56a)
    - [Removal of `-march=armv5`](#orge7f3bb8)
  - [`-fancy` images](#org44e4d7a)
  - [`-build` images](#orgb4acb2f)
- [License](#org9d59e77)



<a id="org317f682"></a>

# Supported Tags


<a id="orgcbbee6f"></a>

## Simple Tags

-   `2.0.10-alpine3.12`, `2.0.10-alpine`, `alpine3.12`, `alpine`
-   `2.0.10-alpine3.12-fancy`, `2.0.10-alpine-fancy` `alpine3.12-fancy`, `alpine-fancy`
-   `2.0.10-alpine3.12-build`, `2.0.10-alpine-build`, `alpine3.12-build`, `alpine-build`
-   `2.0.10-alpine3.11`, `alpine3.11`
-   `2.0.10-alpine3.11-fancy` `alpine3.11-fancy`
-   `2.0.10-alpine3.11-build`, `alpine3.11-build`
-   `2.0.10-buster`, `buster`
-   `2.0.10-buster-fancy`, `buster-fancy`
-   `2.0.10-buster-build`, `buster-build`
-   `2.0.10-stretch`, `stretch`
-   `2.0.10-stretch-fancy`, `stretch-fancy`
-   `2.0.10-stretch-build`, `stretch-build`
-   `2.0.10-windowsservercore-1809`, `windowsservercore-1809`
-   `2.0.10-windowsservercore-1809-build`, `windowsservercore-1809-build`
-   `2.0.10-windowsservercore-ltsc2016`, `windowsservercore-ltsc2016`


<a id="orgaeaa946"></a>

## Shared Tags

-   `2.0.10`, `latest`
    -   `2.0.10-debian-buster`
    -   `2.0.10-windowsservercore-1809`
    -   `2.0.10-windowsservercore-ltsc2016`
-   `2.0.10-fancy`, `latest-fancy`
    -   `2.0.10-debian-buster-fancy`
-   `2.0.10-build`, `latest-build`
    -   `2.0.10-debian-buster-build`
    -   `2.0.10-windowsservercore-1809-build`
    -   `2.0.10-windowsservercore-ltsc2016-build`
-   `2.0.10-windowsservercore`, `windowsservercore`
    -   `2.0.10-windowsservercore-1809`
    -   `2.0.10-windowsservercore-ltsc2016`
-   `2.0.10-windowsservercore-build`, `windowsservercore-build`
    -   `2.0.10-windowsservercore-1809-build`


<a id="org4d8592d"></a>

# Quick Reference

-   **SBCL Home Page:** [http://sbcl.org](http://sbcl.org)
-   **Where to file Docker image related issues:** <https://gitlab.common-lisp.net/cl-docker-images/sbcl>
-   **Where to file issues for SBCL itself:** [https://bugs.launchpad.net/sbcl](https://bugs.launchpad.net/sbcl)
-   **Maintained by:** [Eric Timmons](https://github.com/daewok) and the [MIT MERS Group](https://mers.csail.mit.edu/) (i.e., this is not an official SBCL image)
-   **Supported platforms:** `linux/amd64`, `linux/arm64`, `linux/arm/v7`, `windows/amd64`


<a id="org07276a6"></a>

# What is SBCL?

From [SBCL's Home Page](http://sbcl.org):

> Steel Bank Common Lisp (SBCL) is a high performance Common Lisp compiler. It is open source / free software, with a permissive license. In addition to the compiler and runtime system for ANSI Common Lisp, it provides an interactive environment including a debugger, a statistical profiler, a code coverage tool, and many other extensions.


<a id="org386b700"></a>

# What's in the image?

This image contains SBCL binaries built from the latest source code released by the SBCL devs for a variety of OSes and architectures.


<a id="orgaafe255"></a>

## Patches

The goal is to track upstream as closely as possible. Thus, patches are kept to a minimum (and ideally kept for only as long as it takes for them to be upstreamed). The only exception is trivial patches to things like test timeouts (as the stock timeouts can frequently be too short when cross-building images with QEMU).

In addition to the trivial patches, the following patches are applied when building specific tags.


<a id="org18df56a"></a>

### Removal of `-Wimplicit-fallthrough`

The version of gcc distributed in Debian Stretch does not recognize this option. Remove if on the only affected configuration (Debian Stretch, amd64).


<a id="orge7f3bb8"></a>

### Removal of `-march=armv5`

GCC version 9 removed the `armv5` architecture target used by SBCL's build configuration for armhf. The affected images (Alpine 3.11+ and Ubuntu Focal for arm32v7) have had the target architecture changed to `armv7-a`. This issue has been [reported upstream](https://bugs.launchpad.net/sbcl/+bug/1839783).


<a id="org44e4d7a"></a>

## `-fancy` images

The tags with a `-fancy` suffix have SBCL built by passing `--fancy` to SBCL's `make.sh`. This results in an image that has additional features added, such as core compression and internal xrefs.


<a id="orgb4acb2f"></a>

## `-build` images

While the build configuration follows upstream's default set of build features, SBCL is very configurable at build time and it would be a shame to not expose this somehow. Therfore, in addition to the standard images, a set of "build" images (tags with the `-build` suffix) are provided.

These build images have SBCL already installed in them and include the SBCL source code and any packages needed to build SBCL from scratch. This allows a customized SBCL to be easily built. To customize the feature set, place a file at `/usr/local/src/sbcl-$SBCL_VERSION/customize-target-features.lisp` or `C:\sbcl-$SBCL_VERSION\customize-target-features.lisp`. See the SBCL build instructions for more details on what this file should contain. To patch SBCL, place any number of patch files (ending in ".patch") in `/usr/local/src/sbcl-${SBCL_VERSION}/patches/` or `C:\sbcl-$SBCL_VERSION\patches\`. To build and install SBCL, execute `rebuild-sbcl`. This script will apply the patches, build, install, and remove the previous copy of SBCL.

While these build images give a lot of flexibility, it results in the images being much larger than the non-build images. Therefore, it is recommended that you use them in [multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/).

Note that the Windows build images do not ship with the full toolchain needed to build SBCL as I have not yet finished my due diligence to understand all the licenses for the tools used (I'm not a Windows developer and don't spend much time on that OS). Until then, the Windows builds will download and install the toolchain as part of the rebuild process.


<a id="org9d59e77"></a>

# License

SBCL is licensed using a mix of BSD-style and public domain licenses. See SBCL's [COPYING](http://sbcl.git.sourceforge.net/git/gitweb.cgi?p=sbcl/sbcl.git;a=blob_plain;f=COPYING;hb=HEAD) file for more info.

The Dockerfiles used to build the images are licensed under BSD-2-Clause.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
