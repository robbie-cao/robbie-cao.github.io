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

### Problem

Even with direct driver installed without any error, My Mac (OS X 10.11) doesn't recognize FTDI device.
The device does not appear in the /dev directory.

### Investigation

Found an article with the similar issue: [Empty serial port details on OS X 10.11 (El Capitan) Beta 3](https://github.com/voodootikigod/node-serialport/issues/552).

And another article - [How to Install FTDI Drivers](https://learn.sparkfun.com/tutorials/how-to-install-ftdi-drivers/mac).

Yet another article - [OS X Mavericks (10.9): USB/Serial Driver Setup](http://ewen.mcneill.gen.nz/blog/entry/2014-05-25-os-x-mavericks-usb-serial/).

Yet another article with detailed steps on OSX Leopard (10.5.8) - [Installing FTDI USB Serial Driver on Mac](http://dfusion.com.au/wiki/tiki-index.php?page=Installing+FTDI+USB+Serial+Driver+on+Mac)

### Solution

The problem comes from FTDI USB productID and vendorID is not listed in driver plist file. Will need add its ID manually.

Steps refer to [FTDI chip and OS X 10.10](http://www.mommosoft.com/blog/2014/10/24/ftdi-chip-and-os-x-10-10/).

If using AppleUSBFTDI.kext, steps as below:

1. Get FTDI device productID and vendorID (System Information -> Hardware -> USB)
2. Disable `System Integrity Protection`. Steps refer to [Operation Not Permitted in Mac OS X](http://robbie-cao.github.io/blog/2016/05/18/operation-not-permitted-in-mac-osx)
3. Edit `/System/Library/Extensions/AppleUSBFTDI.kext/Contents/Info.plist`, add productID and vendorID of your device

        <key>AppleUSBEFTDI-FT2232HQ-TI</key>
        <dict>
        <key>CFBundleIdentifier</key>
        <string>com.apple.driver.AppleUSBFTDI</string>
        <key>IOClass</key>
        <string>AppleUSBFTDI</string>
        <key>IOProviderClass</key>
        <string>IOUSBHostInterface</string>
        <key>InputBuffers</key>
        <integer>8</integer>
        <key>OutputBuffers</key>
        <integer>16</integer>
        <key>bConfigurationValue</key>
        <integer>1</integer>
        <key>bInterfaceNumber</key>
        <integer>1</integer>
        <key>idProduct</key>
        <integer>49962</integer>
        <key>idVendor</key>
        <integer>1105</integer>
        </dict>

4. Load driver

        $ sudo kextunload /System/Library/Extensions/AppleUSBFTDI.kext
        $ sudo kextload /System/Library/Extensions/AppleUSBFTDI.kext

5. Plug your FTDI device and check if USB device can be listed:

        $ ls -l /dev/cu.* /dev/tty.*
        crw-rw-rw-  1 root  wheel   17,   5 May 18 13:43 /dev/cu.usbserial-cc3101B
        crw-rw-rw-  1 root  wheel   17,   4 May 18 13:43 /dev/tty.usbserial-cc3101B

6. Congratulations if you can see the result above.

If using driver by FTDI, same steps except the kext is located at `/Library/Extensions/FTDIUSBSerialDriver.kext`.

## Reference

- [FTDI D2XX Drivers](http://www.ftdichip.com/Drivers/D2XX.htm)
- [FTDI Drivers Installation guide for MAC OS X](http://www.ftdichip.com/Support/Documents/AppNotes/AN_134_FTDI_Drivers_Installation_Guide_for_MAC_OSX.pdf)
- [D2XX Programmer's Guide](http://www.ftdichip.com/Support/Documents/ProgramGuides/D2XX_Programmer's_Guide(FT_000071).pdf)
- [Introducing the Apple AppleUSBFTDI kernel driver](https://developer.apple.com/library/mac/technotes/tn2315/_index.html)
- [USB Vendor ID / Product ID Guidelines](http://www.ftdichip.com/Support/Documents/TechnicalNotes/TN_100_USB_VID-PID_Guidelines.pdf)

