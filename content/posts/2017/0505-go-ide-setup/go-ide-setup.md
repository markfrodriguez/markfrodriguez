+++
date = "2017-05-05T10:39:22-04:00"
description = ""
draft = false
slug = "go-ide-setup"
tags = []
title = "Go IDE Setup using Visual Studio Code"
topics = []

+++

It's assumed that Go is already installed and configured. Reference this pervious post for details.

1. [Download](https://code.visualstudio.com/) Microsoft Visual Studio Code and install by copying into the `Applications` directory.
2. Launch Visual Studio Code
3. From the `Debug` menu, select `Install Additional Debuggers...`
![VSC Debug Menu](/posts/2017/0505-go-ide-setup/vsc-debug-menu.png)
4. Install the the Go extension.
![VSC Extension](/posts/2017/0505-go-ide-setup/vsc-extensions.png)
5. Reload Visual Studio Code as instructed.
6. From the File menu, open a Go project directory.. Use the hello project from the initial setup.
7. When first opening up a *.go source file, Visual Studio Code prompts to install several Golang specific utilities. Select the option to install all.
![VSC Install Go Extension](/posts/2017/0505-go-ide-setup/vsc-install-go-ext.png)
![VSC Intall Go Log](/posts/2017/0505-go-ide-setup/vsc-install-go-log.png)
8. Now [install](https://github.com/derekparker/delve/blob/master/Documentation/installation/README.md) the delve debugger simply by running the `brew` command below.

```
brew install go-delve/delve/delve
```

![Delve Install](/posts/2017/0505-go-ide-setup/delve-install.png)
