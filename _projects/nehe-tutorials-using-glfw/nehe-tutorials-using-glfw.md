---
title: "NeHe Tutorials Using GLFW"
description: "NeHe has some of the best tutorials out there for OpenGL. However, all of his original code is in Visual C++ using native Windows window management. I've been playing around with GLFW to do window management and set up the OpenGL context and I wasn't able to find any examples of the NeHe tutorials using GLFW so once I got mine working I decided to post it in case anyone else finds it useful. Enjoy!"
layout: projects
order: 30
permalink: /projects/nehe-tutorials-using-glfw/
---

[NeHe](http://nehe.gamedev.net/) has some pretty awesome tutorials for learning OpenGL. The original code is all in Visual C++ using native Windows windowing. This is my port of that code to C using GLFW for windowing.

## Setup

This project is hosted on [GitHub!](https://github.com/pjreddie/NeHe-Tutorials-Using-GLFW)

To get this running you will need to install the [Simple OpenGL Image Library (SOIL)](http://www.lonesock.net/soil.html) for texture loading and [GLFW](http://www.glfw.org/) for window management.

To install SOIL you will probably have to modify the given Makefile included in `projects/makefile`. On OS X I had to modify the `CXX` line to read:

    CXX = gcc -arch i386 -arch x86_64

Once you have the dependancies involved you can clone the git repository:

    git clone https://github.com/pjreddie/NeHe-Tutorials-Using-GLFW.git

Then you are ready to go!

## Usage

Each individual tutorial is in its own file. There is a Makefile in the top-level directory that should compile everything just by using `make` (it is written for OS X but you should be able to change the libraries to get it to work on your system).

The finished binaries can be run from the tutorial's folder or from the top-level folder since a data directory exists in both places (files in the code are opened with relative pathing). When you try it out you should see something like this:

    $ make
        cc -Wall  -lglfw ... 1/tutorial_1.c -o 1/run

            ... [A few more lines of this] ...

        cc -Wall  -lglfw ... 10/tutorial_10.c -o 10/run

    $ ./9/run
![stars](stars.png)

If you want to compile the code yourself (without make) you should be able to do something like this:

    $ cd 11
    $ ls
    data        tutorial_11.c
    $ cc -framework OpenGL -framework CoreFoundation\
         -lglfw -lSOIL tutorial_11.c -o program
    $ ./program
![wave](wave.png)


There are some differences in my code vs. NeHe's original code so this is really just meant as a refenece as opposed to a direct port. I was having trouble locating any sample code for doing NeHe's tutorials using GLFW so once I got it working I decided to post what I had so others could use it as well. I hope you find it useful!
