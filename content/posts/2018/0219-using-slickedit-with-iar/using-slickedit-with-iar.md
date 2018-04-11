+++
date = "2018-02-19T11:33:22-04:00"
description = ""
draft = false
slug = "using-slickedit-with-iar"
tags = []
title = "Using Slickedit with IAR"
topics = []

+++

# Intro

Find myself using several different Winows-based embedded development tools such as Keil’s [uVision][1], [IAR Embedded Workbench][2], TI’s [Code Composer Studio][3], Renesas’s [e2studio][4], Freescale’s [CodeWarrior][5], Rowley’s [CrossWorks][6], etc. Some have “standardized” on [Eclipse CDT][7], but I’m not crazy about how any of them function as an advanced code editor and find myself much more productive with [Slickedit][8].

Below is how I got Slickedit and the IAR tools for Renesas RX to play nice together. Note, Slickedit is only used for code editing and building a “dummy” image. I wanted the ability to compile and link so I could squash compile issues before using the IAR tools for onboard debug with the target. So bottom line is write / edit code in Slickedit and build project to check for compile issues. Use IAR to download to target and actually perform debug.

# IAR External Editor Support

Even though I’m known to write perfect code all the time, there are rare occasions where things don’t work as expected (this is almost always the fault of the tools themselves or the hardware, but I’ll leave that to another post).

In the event that I’m in IAR and need to quickly jump over to Slickedit for code edits, The “Use External Editor” feature of IAR works nicely. Below are the specified options to launch Slickedit to a specific file and line number. IAR has several modes where clicking on search results, compile messages / warnings, etc., invoke the external editor feature leaving me right where I need to be in Slickedit.

     Editor: C:\Program Files\SlickEditV18.0.1 x64\win\vs.exe
     Arguments: $FILE_PATH$ -#$CUR_LINE$

This looks like the below screen shot in the IDE options dialog.

![IDE Options](/posts/2018/0219-using-slickedit-with-iar/iar_use_external_editor.png)

# Slickedit Project Properties

Slickedit is pretty flexible in allowing you go add new comiplers with included header directories for indexing. The real setup though happens in the tools tab of the Project Properties.

![Project Properties](/posts/2018/0219-using-slickedit-with-iar/slickedit_project_properties.png)

The tools of importance are “Compile”, “Link”, and “Clean”, whihch I had to add since it’s not standard.

## Clean

This command is run from the configuration build directory (e.g., ./Debug) and simply deletes all the artifacts of the build process (e.g., all the object files).

     del *.o *.vpb

## Compile

I used the option in IAR to show all build messages to see what the IDE was feeding into the compiler. I then replicated this using Slickedit’s escape sequences resulting in the following command line feeding the compiler. This is referenced from the project working directory.

     iccrx.exe %i %defd "%f" -e -Ol --no_cse --no_unroll --no_inline --no_code_motion --no_tbaa --no_cross_call --no_clustering --debug -o "%bd%n.o" --dlib_config "C:\Program Files (x86)\IAR Systems\Embedded Workbench 7.0\rx\LIB\dlrxfllsn.h" --double=32 --data_model=f --align_func=1 --endian l --core rxv1 --eec++ --int=32 --no_fpu

## Linker

A similar thing was done here to learn what IAR’s linker wanted to see. The difference here is that I didn’t care about generating a working target image since my goal was just to make it through the compile and link stage for coding purposes (e.g., get a “clean” build). This means many of the options for image size, start address, etc. are  omitted. The final command line for the linker ended up being:

     ilinkrx.exe %f -o %bdExe\%rn --entry __iar_program_start --debug_lib

This is run from the configuration build directory perspective.

# Output

When all is said and done, the output in Slickedit’s build window looks like:

     C:\Workspace\x-1>echo VSLICKERRORPATH="C:\Workspace\x-1"
     VSLICKERRORPATH="C:\Workspace\x-1"
     C:\Workspace\x-1>"C:\PROGRA~1\SLICKE~1.1X6\win\vsbuild" build "C:\Workspace\x-1\x-1.vpw" "C:\Workspace\x-1\x-1.vpj" -signal 51805
     ---------- 'build' Project: 'x-1.vpj' - 'Debug' ---------- VSLICKERRORPATH="C:\Workspace\x-1"
     main.cpp
        IAR C/C++ Compiler V2.60.3.1921 for Renesas RX
        Copyright 2009-2014 IAR Systems AB.
        Standalone license - IAR Embedded Workbench for Renesas RX 2.60
      7 bytes of CODE memory
     Errors: none
     Warnings: none
     Linking...
        IAR ELF Linker V2.60.3.1921 for Renesas RX
        Copyright 2009-2014 IAR Systems AB.
         147 bytes of readonly  code memory
       1 152 bytes of readonly  data memory
       4 096 bytes of readwrite data memory
     Errors: none
     Warnings: none
     Link time:   0.06 (CPU)   0.11 (elapsed)
     Build successful
     C:\Workspace\x-1>

[1]:     http://www.keil.com/uvision/
[2]:     http://www.iar.com/Products/IAR-Embedded-Workbench/
[3]:     http://www.ti.com/tool/ccstudio
[4]:     http://am.renesas.com/products/tools/ide/ide_e2studio/
[5]:     http://www.freescale.com/webapp/sps/site/homepage.jsp?code=CW_HOME
[6]:     http://www.rowley.co.uk/
[7]:     http://www.eclipse.org/cdt/
[8]:     http://www.slickedit.com/products/slickedit
