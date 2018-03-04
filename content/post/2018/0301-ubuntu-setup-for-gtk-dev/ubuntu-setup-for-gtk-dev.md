+++
date = "2018-03-01T16:07:22-04:00"
description = ""
draft = false
slug = "ubuntu-setup-for-gtk-dev"
tags = []
title = "How To Setup Ubuntu for GTK / GUI Development"
topics = []

+++

## Basic Setup

Running Ubuntu 17.10 within VMWare Fusion 10.1.1. The installation of Ubuntu is pretty straight forward and basically requires creating a new VM and pointing to the downloaded [Ubuntu ISO image](http://releases.ubuntu.com/17.10.1/ubuntu-17.10.1-desktop-amd64.iso). Once the installation is complete, ensure the system is up-to-date by clicking on "Activities" in the upper left hand corner of the screen and using the search box to find the "Software Updater" utility.

Next, run the following commands:
```
sudo apt-get update
sudo apt-get upgrade
```

Now that Ubuntu is installed and all updated, let's install some general utilities that will be used. Of course, this can be customized to better fit your use case.

```
sudo apt-get install htop
sudo apt-get install neovim
sudo apt-get install git
```

I'm not a big fan of the default color scheme in Ubuntu's Terminal app, so the colors were changed so the terminal window has a white background. This is done with the menu option Edit --> Profile Preferences. Select the "Colors" tab and change the "Text and Background Color".

![Terminal Color Scheme](/post/2018/0301-ubuntu-setup-for-gtk-dev/terminal_colors.png)

Now that the terminal window's background is white, the green prompt color needs to change as it's unreadable (at least for me).

![Green Prompt](/post/2018/0301-ubuntu-setup-for-gtk-dev/green_prompt.png)

To change the terminal prompt color, edit the `~/.bashrc` file as shown below.

```
# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=no;
esac
```

## Install Google Chrome

[Download](https://www.google.com/chrome/) the latest Google Chrome package for Linux.

```
cd ~/Downloads
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt-get install -f
```

## Install Core Development Tools

```
sudo apt-get install build-essential
sudo apt-get install manpages-dev man-db manpages-posix-dev
```

## Install GTK Development Tools

```
sudo apt-get install libgtk-3-dev
sudo apt-get install glade
```

## GTK Hello World

Now that all the necessary dev tools are installed, let's verify everything by building a GTK-based helloworld application.

helloworld.c
```
#include <gtk/gtk.h>

static void
print_hello (GtkWidget *widget,
             gpointer   data)
{
  g_print ("Hello World\n");
}

static void
activate (GtkApplication *app,
          gpointer        user_data)
{
  GtkWidget *window;
  GtkWidget *button;
  GtkWidget *button_box;

  window = gtk_application_window_new (app);
  gtk_window_set_title (GTK_WINDOW (window), "Window");
  gtk_window_set_default_size (GTK_WINDOW (window), 200, 200);

  button_box = gtk_button_box_new (GTK_ORIENTATION_HORIZONTAL);
  gtk_container_add (GTK_CONTAINER (window), button_box);

  button = gtk_button_new_with_label ("Hello World");
  g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);
  g_signal_connect_swapped (button, "clicked", G_CALLBACK (gtk_widget_destroy), window);
  gtk_container_add (GTK_CONTAINER (button_box), button);

  gtk_widget_show_all (window);
}

int
main (int    argc,
      char **argv)
{
  GtkApplication *app;
  int status;

  app = gtk_application_new ("org.gtk.example", G_APPLICATION_FLAGS_NONE);
  g_signal_connect (app, "activate", G_CALLBACK (activate), NULL);
  status = g_application_run (G_APPLICATION (app), argc, argv);
  g_object_unref (app);

  return status;
}
```

Makefile
```
NAME=helloworld
CFLAGS=-g -Wall -o $(NAME)
GTKFLAGS=-export-dynamic `pkg-config --cflags --libs gtk+-3.0`
SRCS=helloworld.c
CC=gcc
 
# top-level rule to create the program.
all: main
 
# compiling the source file.
main: $(SRCS)
    $(CC) $(CFLAGS) $(SRCS) $(GTKFLAGS)
 
# cleaning everything that can be automatically recreated with "make".
clean:
    /bin/rm -f $(NAME)
```

Both both the helloworld.c and Makefile into a directory called helloworld. To build, change directory (cd) into the helloworld folder and run make. If all goes well, the helloworld executable will be generated and ready to run.

![Hello World](/post/2018/0301-ubuntu-setup-for-gtk-dev/helloworld.png)

