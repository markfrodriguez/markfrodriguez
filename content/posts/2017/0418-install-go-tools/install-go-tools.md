+++
date = "2017-04-18T19:05:22-04:00"
description = ""
draft = false
slug = "install-go-tools"
tags = []
title = "Install Go Tools on Mac OS X"
topics = []

+++

## Install Go Toolchain

[Download](https://golang.org/dl/) the Mac OS X package installer, open it, and follow the prompts to install the Go tools. The package installs the Go distribution to `/usr/local/go`.

## Setup GOPATH Workspace

Create a new workspace directory under home.

```
mkdir ~/workspace/goapps
```

[Update shell environment variables]({{< relref "posts/2017/0418-mac-shell-env-vars/mac-shell-env-vars.md" >}}) so `GOPATH` points to the newly created `goapps` workspace.

In `GOPATH`, create the three folders as follows:

* `src` for source files whose suffix is .go, .c, .g, .s.
* `pkg` for compiled files whose suffix is .a.
* `bin` for executable files

## Testing Installation

Lets create a simple hello Go application to test out the environment. Create a new hello directory under $GOPATH/src

```
mkdir $GOPATH/src/hello
```

Now create a new hello.go file with the following source so it looks like:

```
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```

Build hello.go with the go tool:

```
cd $GOPATH/src/hello
go build
```

The command above builds an executable named `hello` in the directory alongside the source. Execute the application to see the greeting:

```
./hello
```
