+++
date = "2018-03-02T15:26:22-04:00"
description = ""
draft = false
slug = "installing-gnat-ada-tools"
tags = []
title = "Installing GNAT Ada Tools on Ubuntu Linux"
topics = []

+++

## Getting GNAT Tools

Now that Ubuntu is [setup for development]({{< relref "post/2018/0301-ubuntu-setup-for-gtk-dev/ubuntu-setup-for-gtk-dev.md" >}}), we’re going to install all the necessary tools to have a full GNAT Ada development environment. By “full”, I mean having a playground to work with Ada 2012, Spark, GUI apps with GTK+, and cross compiling for ARM MCUs.

> **Note, the instructions that follow assume you already have a setup capable of GTK+ development. If you don't, please reference [this previous post]({{< relref "post/2018/0301-ubuntu-setup-for-gtk-dev/ubuntu-setup-for-gtk-dev.md" >}}).**

First, let’s grab all the necessary packages from AdaCore’s web site. Download the files below:

- [gnat-gpl-2017-x86_64-linux-bin.tar.gz](http://mirrors.cdn.adacore.com/art/591c6d80c7a447af2deed1d7)
- [spark-discovery-gpl-2017-x86_64-linux-bin.tar.gz](http://mirrors.cdn.adacore.com/art/592c5299c7a447388d5c991d)
- [gtkada-gpl-2017-x86_64-linux-bin.tar.gz](http://mirrors.cdn.adacore.com/art/591af522c7a4473fcbb15524)
- [gnat-gpl-2017-arm-elf-linux-bin.tar.gz](http://mirrors.cdn.adacore.com/art/591c6413c7a447af2deed0e3)

Now, let’s extract each of the downloaded packages.

```
cd ~/Downloads
tar xvf gnat-gpl-2017-x86_64-linux-bin.tar.gz
tar xvf spark-discovery-gpl-2017-x86_64-linux-bin.tar.gz
tar xvf gtkada-gpl-2017-x86_64-linux-bin.tar.gz
tar xvf gnat-gpl-2017-arm-elf-linux-bin.tar.gz
```

## Picking Install Location

There are multiple options when picking a base path for the GNAT tools. The typical ones are:

```
/usr/local/gnat
/opt/gnat
/usr/gnat
/home/<your user name>/gnat
```

Because we're installing the full GNAT Ada development tools distributed by [AdaCore](https://www.adacore.com/), we’ll use `/opt/adacore` as the root path for all our installations.

## Installing Main GNAT Tools

```
cd ~/Downloads/gnat-gpl-2017-x86_64-linux-bin
sudo ./doinstall
```

When prompted to select a directory to install, change the presented default of `/usr/gnat` to `/opt/adacore/gnat`.

![GNAT Install Location](/posts/2018/0303-installing-gnat-ada-tools/gnat_install_location.png)

After the install script completes, update your PATH environment variable in `~/.bashrc` so the new GNAT tools are available.

```
PATH=“/opt/adacore/gnat/bin:$PATH”
export PATH
```

## Launching GNAT Programming Studio (GPS)

The main GNAT IDE is called GNAT Programming Studio or GPS. The first time I attempted to launch via terminal by simply executing gps, I got a few error messages related to the canberra-gtk-module and the initial GPS splash screen presented itself blank and unresponsive.

![GPS Launch Failure](/posts/2018/0303-installing-gnat-ada-tools/gps_launch_failure.png)

The error message on the console is:

```
Gtk-Message: Failed to load module "canberra-gtk-module”
```

After exiting the terminal session, the second launch still complained about the canberra module failing to load, but GPS launch as expected.

![GNAT GPS Environment](/posts/2018/0303-installing-gnat-ada-tools/gnat_gps.png)

## Installing Spark 2014

```
cd ~/Downloads/spark-discovery-gpl-2017-x86_64-linux-bin
sudo ./doinstall
```

When prompted to select a directory to install, change the presented default of `/opt/spark2014` to `/opt/adacore/spark2014`.

After the install script completes, update your PATH environment variable in `~/.bashrc` so the new SPARK 2014 tools are available.

```
PATH=“/opt/adacore/gnat/bin:/opt/adacore/spark2014/bin:$PATH”
export PATH
```

To verify that all is well, start a new terminal session and execute

```
gnatprove --version
```

## Installing GtkAda

```
cd ~/Downloads/gtkada-gpl-2017-x86_64-linux-bin
sudo "PATH=$PATH" ./doinstall
```

The doinstall script will automatically locate the gnat installation and ask if you want to also install gtkada in the same location. In keeping with the established separation of the tools under the `/opt/adacore` path, let’s change the recommended `/opt/adacore/gnat` to `/opt/adacore/gtkada`.

This steps takes a few minutes as all the sources are built. If everything goes well, a success message from the GtkAda package is presented.

![GtkAda Success](/posts/2018/0303-installing-gnat-ada-tools/gtkada_success.png)

For actual GtkAda development, several environment variables such as LD_LIBRARY_PATH, PKG_CONFIG_PATH, etc. need to be set. For now, the easiest way to handle this is to run the `/opt/adacore/gtkada/bin/gtkada-env.sh` script. In a future post, I’ll get more into actual GtkAda development.

One thing we’ll want to do now however, is update our PATH environment variable so the gtkada tools are accessible.

```
PATH=“/opt/adacore/gnat/bin:/opt/adacore/spark2014/bin:/opt/adacore/gtkada/bin:$PATH”
export PATH
```

To verify that everything installed as expected, run the following testgtk executable that was built and installed during the above step.

```
/opt/adacore/gtkada/share/examples/gtkada/testgtk/testgtk
```

![GtkAda Test App](/posts/2018/0303-installing-gnat-ada-tools/testgtkada.png)

## Installing GNAT for ARM

```
cd ~/Downloads/gnat-gpl-2017-arm-elf-linux-bin
sudo ./doinstall
```

When prompted to select a directory to install, change the presented default of `/usr/gnat` to `/opt/adacore/gnat-arm`.

After the install script completes, update your PATH environment variable in `~/.bashrc` so the new GNAT ARM tools are available.

```
PATH=“/opt/adacore/gnat/bin:/opt/adacore/spark2014/bin:/opt/adacore/gtkada/bin:/opt/adacore/gnat-arm/bin:$PATH”
export PATH
```

![GNAT ARM Install](/posts/2018/0303-installing-gnat-ada-tools/gnat-arm-install.png)

As a sanity check that the GNAT ARM cross compiler is working, let’s build the sample led_flasher project included with the install. Before this can be done however, we first need to resolve the issue that the GNAT ARM tools are built for 32-bit Linux and our Ubuntu setup is 64-bit. When trying to run any of the binaries in `/opt/adacore/gnat-arm/bin`, the misleading message “No such file or directory” is given. Ubuntu introduced [MultiArch](https://help.ubuntu.com/community/MultiArch) support so the fix is to execute the commands below.

```
sudo dpkg --add-architecture i386
sudo apt-get update
# For CLI binaries
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
# For GPS included with GNAT ARM package
sudo apt-get install libbz2-1.0:i386 libsm6:i386
```

Now let’s get back to verifying the GNAT ARM tools. Start out by copying an example project to some directory within your home folder.

```
cp -R /opt/adacore/gnat-arm/share/examples/gnat-cross/led_flasher-stm32f4 ~/Desktop
```

Open the project in GPS by launching gps and selecting the option to “Open project”. When the file browser is presented, navigate to the led_flasher-stm32f4 folder on the Desktop and open flasher.gpr. After the project loads within GPS, select Build—>Project—>Build All and then click the “Execute” button on the dialog. The results should be similar to the screenshot below.

![GNAT ARM Install](/posts/2018/0303-installing-gnat-ada-tools/gps_arm_build.png)

**Congratulations!** We are now ready to rock-n-roll with any kind of Ada development (e.g., CLI, GUI/GTK+, Spark, and cross-compile for ARM). Code safe my friend :-)

