---
layout: post
title: Build OpenOCD with FTD2XX on Mac
categories: embedded
---

## Dependencies

Toolchain:

- gcc -or- clang
- make
- libtool
- pkg-config >= 0.23 (or compatible)
- autoconf >= 2.64
- automake >= 1.9
- texinfo

USB-Blaster, ASIX Presto, OpenJTAG and ft2232 interface adapter
drivers need either one of:

- libftdi: http://www.intra2net.com/en/developer/libftdi/index.php
- ftd2xx: http://www.ftdichip.com/Drivers/D2XX.htm (proprietary,
  GPL-incompatible)

> Refer to [Installed FTDI D2XX Driver and Library on Mac](http://robbie-cao.github.io/blog/2016/05/17/install-ftdi-d2xx-driver-on-mac).

USB-based adapters depend on libusb-1.0 and some older drivers require
libusb-0.1 or libusb-compat-0.1. A compatible implementation, such as
FreeBSD's, additionally needs the corresponding .pc files.

```
$ brew install libusb
```


## Download Source

```
$ git clone git://repo.or.cz/openocd.git
-or-
$ git clone git://git.code.sf.net/p/openocd/code openocd
```

## Build

```
$ cd openocd
$ git checkout -b v0.9.0 v0.9.0
$ ./bootstrap
$ ./configure --enable-maintainer-mode --disable-werror --disable-shared --enable-ft2232_ftd2xx
$ make
$ make install
```


## Reference

- [Open On-Chip-Debugger Official Site](http://openocd.org)
- [OpenOCD Readme](http://repo.or.cz/w/openocd.git)
- [OpenOCD Documentation](http://openocd.org/doc-release/html/index.html)
- [OpenOCD Developer's Guide](http://openocd.org/doc/doxygen/html/index.html)

