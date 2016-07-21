---
layout: post
title: tftpd Server on Mac OS X
categories: mac osx
---

OS X Lion comes with a tftpd but its disabled by default. Like most services in OS X, `tftpd` is controlled by `launchctl`.
The configuration with which the daemon is lauched is in `/System/Library/LaunchDaemons/tftp.plist` and the the identifier is `com.apple.tftpd`.

Before you make changes to the config:

```
sudo launchctl unload -F /System/Library/LaunchDaemons/tftp.plist
```

Then:

```
sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
```

To stop tftpd:

```
sudo launchctl stop com.apple.tftpd
```

To start tftpd:

```
sudo launchctl start com.apple.tftpd
```

> https://wiki.openwrt.org/doc/howto/generic.flashing.tftp#tftpd_server_on_mac_os_x_lion

