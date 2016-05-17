---
layout: post
title: Install FTDI D2XX Driver and Library on Mac
categories: embedded
---

## Download D2XX Drivers for Mac

```
$ wget http://www.ftdichip.com/Drivers/VCP/MacOSX/FTDIUSBSerialDriver_v2_3.dmg
$ wget http://www.ftdichip.com/Drivers/D2XX/MacOSX/D2XX1.2.2.dmg
```

## Direct Driver Installation

Simply to open FTDIUSBSerialDriver_v2_3.dmg and install the `FTDIUSBSerial.pkg`.

## Driver Library Installation

1. Open a Terminal window (Finder->Go->Utilities->Terminal).
2. If the /usr/local/lib directory does not exist, create it (sudo mkdir /usr/local/lib)
3. if the /usr/local/include directory does not exist, create it (sudo mkdir /usr/local/include)
4. Copy the dylib file to /usr/local/lib (sudo cp Desktop/D2XX/bin/libftd2xx.1.2.2.dylib /usr/local/lib/libftd2xx.1.2.2.dylib)
5. Make a symbolic link (sudo ln -sf /usr/local/lib/libftd2xx.1.2.2.dylib /usr/local/lib/libftd2xx.dylib)
6. Copy the D2XX include file (sudo cp Desktop/D2XX/Samples/ftd2xx.h /usr/local/include/ftd2xx.h
7. Copy the WinTypes include file (sudo cp Desktop/D2XX/Samples/WinTypes.h /usr/local/include/WinTypes.h)
8. You have now successfully installed the D2XX library.)

```
# mount D2XX1.2.2.dmg (assume mounted to /Volumes/release)
$ cd /Volumes/release
$ sudo cp -f D2XX/bin/libftd2xx.1.2.2.dylib /usr/local/lib/
$ sudo ln -sf /usr/local/lib/libftd2xx.1.2.2.dylib /usr/local/lib/libftd2xx.dylib
$ sudo cp -f Samples/ftd2xx.h /usr/local/include/
$ sudo cp -f Samples/WinTypes.h /usr/local/include/
```

## Issue

Even with direct driver installed without any error, My Mac (OS X 10.11) doesn't recognize FTDI device.
The device does not appear in the /dev directory.

Found an article with the similar issue: [Empty serial port details on OS X 10.11 (El Capitan) Beta 3](https://github.com/voodootikigod/node-serialport/issues/552).
Not investigate this discussion yet.

And another article - [How to Install FTDI Drivers](https://learn.sparkfun.com/tutorials/how-to-install-ftdi-drivers/mac).

Yet anther article - [OS X Mavericks (10.9): USB/Serial Driver Setup](http://ewen.mcneill.gen.nz/blog/entry/2014-05-25-os-x-mavericks-usb-serial/).

> TODO

## Reference

- [FTDI D2XX Drivers](http://www.ftdichip.com/Drivers/D2XX.htm)
- [FTDI Drivers Installation guide for MAC OS X](http://www.ftdichip.com/Support/Documents/AppNotes/AN_134_FTDI_Drivers_Installation_Guide_for_MAC_OSX.pdf)
- [D2XX Programmer's Guide](http://www.ftdichip.com/Support/Documents/ProgramGuides/D2XX_Programmer's_Guide(FT_000071).pdf)

