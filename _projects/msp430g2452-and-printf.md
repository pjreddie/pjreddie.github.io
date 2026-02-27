---
title: "MSP430g2452 and printf Using MSPGCC Toolchain"
description: "Why did LaunchPad ever think it was a good idea to ship a microcontroller without hardware UART? No one knows...."
layout: projects
order: 22
permalink: /projects/msp430g2452-and-printf/
---

So you got your computer and your msp430g2452 and you want to be able to program it, and you want it to be able to communicate back to you. This guide is an amalgamation of several other guides and code snippets from around the internet that took a few hours for me to pull together. I want to spare others the trouble.

The msp430g2452 doesn't have hardware support for UART so to communicate with it you have to do UART in software, and it's also a good idea to use a custom printf so you don't use up 2kB of your precious flash storage.

For programming the msp I use the [MSPGCC](http://sourceforge.net/apps/mediawiki/mspgcc/index.php?title=MSPGCC_Wiki) toolchain. Let's get that set up:

    $ sudo apt-get install binutils-msp430 gcc-msp430 msp430-libc mspdebug

Then download the source files from github:

    $ git clone https://github.com/pjreddie/msp430g2452-hello-world

Make and install the software using the `mspgcc` toolchain and `mspdebug` (this is all built into the makefile):

    $ cd msp430g2452-hello-world
    $ make
    $ make install

Open a serial console to the device using `screen`:

    $ screen /dev/ttyACM0 9600

And that's it! The console will probably be blank, hit reset a few times to see it print stuff out. Good luck!

Like I said, I did almost none of this myself, I got the setup from [here](http://www.mycontraption.com/programming-the-msp430-launchpad-on-ubuntu/), I found [oPossum's printf](http://forum.43oh.com/topic/1289-tiny-printf-c-version/) and made some minor updates, tidied up [this](https://gist.github.com/RickKimball/1220877) port of [this](http://forum.43oh.com/topic/706-compact-async-serial-tx/) assembly code for the serial communication, and stole the makefile from who knows where.
