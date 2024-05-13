# Linux4Space Yocto

Linux4Space is a collaborative open source project founded with the intention of creating an open source Yocto based Linux distribution suitable for space applications. The project brings together all the stakeholders: the software and hardware developers, the suppliers, and technology companies. Linux4Space is designed to be compliant with ESA (European Space Agency) Standards and it is based on community defined requirements.

## About repository
This repository serves as a simple Linux4space distro project, which is prepared to be cloned, built and tested on the QEMU machine or BeagleBone.

The repository contains reference to 3 repositories:

- meta-linux4space - Linux4Space core layer
- meta-linux4space-apps - Linux4Space application layer
- poky - Reference Yocto distro layer

## How to start

### Before you attempt to create your own image, certain conditions need to be met:

- You need at least 90 Gb of free space.
- You need at least 8 Gb of RAM, preferably more.
- You need a supported Linux distribution (recent versions of Fedora, openSUSE, CentOS, Debian, Ubuntu, AlmaLinux, Rocky). For a list of supported Linux distributions, please visit the section [Supported Linux Distributions](https://docs.yoctoproject.org/ref-manual/system-requirements.html#supported-linux-distributions) of Yocto Project manual.
- You need a recent version of :
    - GCC
    - GNU make
    - Python
    - tar
    - Git

You also need to install essential host packages and properly set your locale. Use this command.
```
$ sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 python3-subunit zstd liblz4-tool file locales libacl1
$ sudo locale-gen en_US.UTF-8
```

### The following section describes how you can clone and try this distro on your own.

For the purposes of this tutorial, we will be using default machine qemux86-64. If you want to create a distribution for BeagleBone, you need to modify the local.conf file later down the line.

The first part is cloning. To clone repositories with all submodules, use this command.
```
    git clone https://gitlab.com/linux4space/linux4space-yocto
    cd linux4space-yocto
    git submodule init
    git submodule update
```

After that, thhis following command will setup environment and add meta-linux4space and meta-linux4space-apps layers to your bblayers.conf.
```
    source oe-init-build-env
    bitbake-layers add-layer ../meta-linux4space
    bitbake-layers add-layer ../meta-linux4space-apps
```

Now navigate to linux4space-yocto/build/conf and open a file named local.conf. Find a variable named DISTRO and change it's value into linux4space like this.
```
DISTRO ?= "linux4space"
```

If you are ok with building an image for quemux86-64, feel free to skip this part. If you want to build an image for BeagleBone or a different machine, you need to modify the MACHINE variable in local.conf. Simply find the machine you want to use and uncomment the line where it's mentioned.
```
# You need to select a specific machine to target the build with. There are a selection
# of emulated machines available which can boot and run in the QEMU emulator:
#
#MACHINE ?= "qemuarm"
#MACHINE ?= "qemuarm64"
#MACHINE ?= "qemumips"
#MACHINE ?= "qemumips64"
#MACHINE ?= "qemuppc"
#MACHINE ?= "qemux86"
#MACHINE ?= "qemux86-64"
#
# There are also the following hardware board target machines included for 
# demonstration purposes:
#
#MACHINE ?= "beaglebone-yocto"
#MACHINE ?= "genericx86"
#MACHINE ?= "genericx86-64"
#MACHINE ?= "edgerouter"
#
# This sets the default machine to be qemux86-64 if no other machine is selected:
MACHINE ??= "qemux86-64"
```

Now we are ready to run bitbake and run QEMU.
```
    bitbake core-image-minimal
    # Build is going to take couple of hours so get yourself a cup of coffee and relax.
    # After the build is finished, you can try the Linux4Space distro with the runqemu command.
    runqemu
```
Username is root, no password is set.


## Mailing list
If you have any question, please feel free to contact us by using mailing list.
More information can be found here https://wiki.linux4space.org/wiki/Mailing_List