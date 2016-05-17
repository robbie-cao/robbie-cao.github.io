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
$ git checkout -b v0.9.0-rc1 v0.9.0-rc1
$ ./bootstrap
$ ./configure --enable-maintainer-mode --disable-werror --disable-shared --enable-ft2232_ftd2xx
$ make
$ make install
```

## Issue

There's error (as below) when building v0.9.0, v0.9.0-rc1 is okay.

> https://sourceforge.net/p/openocd/mailman/openocd-devel/thread/555B82B7.7010800%40suse.de/#msg34126255

```
Making all in doc
restore=: && backupdir=".am$$" && \
        am__cwd=`pwd` && CDPATH="${ZSH_VERSION+.}:" && cd . && \
        rm -rf $backupdir && mkdir $backupdir && \
        if (makeinfo --version) >/dev/null 2>&1; then \
          for f in openocd.info openocd.info-[0-9] openocd.info-[0-9][0-9] openocd.i[0-9] openocd.i[0-9][0-9]; do \
            if test -f $f; then mv $f $backupdir; restore=mv; else :; fi; \
          done; \
        else :; fi && \
        cd "$am__cwd"; \
        if makeinfo   -I . \
         -o openocd.info openocd.texi; \
        then \
          rc=0; \
          CDPATH="${ZSH_VERSION+.}:" && cd .; \
        else \
          rc=$?; \
          CDPATH="${ZSH_VERSION+.}:" && cd . && \
          $restore $backupdir/* `echo "./openocd.info" | sed 's|[^/]*$||'`; \
        fi; \
        rm -rf $backupdir; exit $rc
openocd.texi:8463: Unknown command `raggedright'.
openocd.texi:8468: Bad argument `raggedright' to `@end', using `table'.
openocd.texi:8468: @item found outside of an insertion block.
openocd.texi:8470: @item found outside of an insertion block.
openocd.texi:8472: @item found outside of an insertion block.
openocd.texi:8475: @item found outside of an insertion block.
openocd.texi:8478: Unmatched `@end'.
makeinfo: Removing output file `openocd.info' due to errors; use --force to preserve.
make[2]: *** [openocd.info] Error 1
make[1]: *** [all-recursive] Error 1
make: *** [all] Error 2
```


## Reference

- [Open On-Chip-Debugger Official Site](http://openocd.org)
- [OpenOCD Readme](http://repo.or.cz/w/openocd.git)
- [OpenOCD Documentation](http://openocd.org/doc-release/html/index.html)
- [OpenOCD Developer's Guide](http://openocd.org/doc/doxygen/html/index.html)

