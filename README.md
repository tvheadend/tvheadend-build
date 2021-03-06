![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)

## Tvheadend - auto builds

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
 

- [Looking for tvheadend builds?](#looking-for-tvheadend-builds)
- [Reference example: Ubuntu builds](#reference-example-ubuntu-builds)
- [Docker Images](#docker-images)
- [Authoring packages for other Distros](#authoring-packages-for-other-distros)
- [Cross-compiling for ARM](#cross-compiling-for-arm)
- [Contact](#contact)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### Looking for tvheadend builds?

Regular user? Download and enjoy the official pre-built tvheadend packages. We make the auto builds on the following operating systems and architectures:

* Docker Hub build farm
* `Intel x86_64` architecture
* Ubuntu `14.04.2` 'Trusty' --> Produces all of our 'ubuntu' APT debs

They have been reported to work on all of these Linux distributions:

* Ubuntu `14.04` - 'Trusty'
* Ubuntu `14.10` - 'Utopic'
* Ubuntu `15.04` - 'Vivid'
* Mint Linux `17` - 'Qiuana' (not confirmed)
* Mint Linux `17.1` - 'Rebecca' or 'Cinnamon'
* Debian `8.0` - 'Jessie'

*NOT Supported / Don't work:*

* `Intel i386` architecture
* Debian `7.0` - 'Wheezy' (oldstable)

***Download:***

You should download the pkgs marked as **Ubuntu Apt Repository for 4.x**, from here:

https://tvheadend.org/projects/tvheadend/wiki/AptRepository

***Not supported?***

Missing linux distro? Missing architecture? ---> [See here](#contact).

### Reference example: Ubuntu builds
**_Build chain for Ubuntu .deb binary pkgs of Tvheadend server_**

This is the Reference Guide for package maintainers, it starts here:

* [Maintainers Guide - ubuntu builds](ubuntu/0. maintainers-guide.md)

### Docker Images

Docker images are available at:

https://github.com/dreamcat4/docker-images/blob/master/tvh/README.md

They are always updated with the latest versions of tvheadend coming from this `tvheadend-build` auto-builds system.

### Authoring packages for other Distros

There are now packages for ubuntu. But what about Debian, openSUSE, Fedora, CentOS, Arch and whatever other distros ? You must follow / tweak the reference example to suit your needs:

* [Maintainers Guide - ubuntu builds](ubuntu/0. maintainers-guide.md)

For this example, we use the `centos` distribution. We assume you are also a user of `centos` who is familiar with it's package manager.

* Copy the whole folder, from `ubuntu/` --> `centos/` etc.
  * `git clone https://github.com/tvheadend/tvheadend-build.git`
  * `cd tvheadend-build && cp -rf ubuntu centos`

* In the new folder, find the dependancies base image `deps/Dockerfile`
  * Change the 1st line:
    * `FROM: ubuntu-debootstrap:14.04` ---> a decent minimal official docker base image of `centos` or `debian` etc. whatever.
  * Change the `apt-get install` ---> commands specific to your distro's package manager e.g. `yum`.

* In the remaining folders: `master/`, `unstable`, `testing` and `stable/` open the `Dockerfile`: 
  * Change the 1st line:
    * `FROM: dreamcat4/tvh.build.ubuntu.deps` ---> `FROM: yourDockerUsername/tvh.build.centos.deps`
  * Change the `Autobuild.sh` line, which builds a `.deb` file, to build an rpm instead.
    * Try to keep all the same `./configure` args such as `----enable-libffmpeg_static` etc.
  * At bottom, we run a script to upload the `/out` files to a bintray repo:
    * You will need modify that script `upload-to-bintray` also.

Follow all the the other steps in the Ubuntu guide - it is mostly the same. Just with different repo names, username, and so on.

* Commit changes to git.
* Push the new `centos` folder up to github.

### Cross-compiling for ARM

Dockerhub is an `x86_64` build farm. So to use Dockerhub for compiling Tvheadend for other architectures means cross compiling on intel host.

You must follow / tweak the reference example to suit your needs:

* [Maintainers Guide - ubuntu builds](ubuntu/0. maintainers-guide.md)

Some useful links for cross-compiling:

* http://vraidsys.com/2013/06/cross-compile-tvheadend-for-arm-cortex-a9/
* https://github.com/jzerbe/jzerbe.github.com/blob/master/_posts/2013-06-22-cross-compile-tvheadend-for-arm-cortex-a9.md
* http://www.holik.at/index.php?m=06&y=13&entry=entry130627-175237

### Contact

We are interested in any maintainer(s) who wishes to set up the following:

* RPM auto builds for Centos, Fedora, etc
* ARM builds for Rpi2, Odroid-C1, etc

***Note:*** We can only justifty supporting platforms which are *sufficiently popular at this time*. Meaning that the overall user demand should clearly exceed the cost of setting up, running & maintaining a reliable automated builds system.

***Also:*** *we can only support build targets which will compile on the Docker Hub server farm. Those are exclusively `Intel x86_64` linux machines.*

You will be provided with sufficient support for setting up your build target, in conjunction with the [maintainer's reference guide](ubuntu/0. maintainers-guide.md). A working knowledge of GIT and Docker is beneficial. But more relevant is your experience with the target architecture. And a commitment to providing the results as a public builds, to be shared for all.

You can establish contact in any of the following ways:

* Open an issue on this Github repository --> https://github.com/tvheadend/tvheadend-build/issues/new
* Join the `#hts` channel on Freenode.net IRC server
* Email: `dreamcat4@gmail.com`


