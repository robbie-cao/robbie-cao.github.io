---
layout: post
title: Operation Not Permitted in Mac OS X
categories: mac osx
---

## Problem

When I install FTDI D2XX driver on Mac OSX 10.11, it doesn't work.
After investigation, I found it was caused by USB Product ID and Vendor ID not
included in the driver.

I had a try to add USB ID to `/System/Library/Extensions/AppleUSBFTDI.kext/Contents/Info.plist`,
but got **"Operation not permitted"** error.

## Why

On Mac OS X and other UNIX-like operating systems, including Linux, there’s a “root” account that traditionally has full access to the entire operating system. Becoming the root user — or gaining root permissions — gives you access to the entire operating system and the ability to modify and delete any file. Malware that gains root permissions could use those permissions to damage and infect the low-level operating system files.

System Integrity Protection — also known as “rootless” — functions by restricting the root account. The operating system kernel itself puts checks on the root user’s access and won’t allow it to do certain things, such as modify protected locations or inject code into protected system processes. All kernel extensions must be signed, and you can’t disable System Integrity Protection from within Mac OS X itself. Applications with elevated root permissions can no longer tamper with system files.

You’re most likely to notice this if you attempt to write to one of the following directories

- /System
- /bin
- /usr
- /sbin

OS X just won’t allow it, and you’ll see an “Operation not permitted” message. OS X also won’t allow you to mount another location over one of these protected directories, so there’s no way around this.

The full list of protected locations is found at /System/Library/Sandbox/rootless.conf on your Mac.

## Solution

1. Boot into recovery mode by restarting your Mac and hold `Command+R` as it boots.
2. You'll enter the recovery environment. Click the "Utilities" menu and select "Terminal" to open a terminal window.
3. You'll see whether System Integrity Protection is enabled or not.

    ```
    $ csrutil status
    System Integrity Protection status: enabled.
    ```

4. Enter the following command:

    ```
    $ csrutil disable
    ```

## Reference

- [StackOverflow - Operation Not Permitted when on root El capitan](http://stackoverflow.com/questions/32659348/operation-not-permitted-when-on-root-el-capitan-rootless-disabled)
- [StackExchange - Why does chown report "Operation not permitted" on OS X?](http://superuser.com/questions/279235/why-does-chown-report-operation-not-permitted-on-os-x)
- [Mac Developer Library - Configuring System Integrity Protection](https://developer.apple.com/library/mac/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html)
- [System Integrity Protection on your Mac](https://support.apple.com/en-us/HT204899)
- [How to Disable System Integrity Protection on a Mac (and Why You Shouldn't)](http://www.howtogeek.com/230424/how-to-disable-system-integrity-protection-on-a-mac-and-why-you-shouldnt/)

