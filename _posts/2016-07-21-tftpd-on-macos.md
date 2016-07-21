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

Test tftpd server:

```
# put a file into tftpd folder first of all
# tftpd root folder:
# Linux : /var/lib/tftpboot/
# Mac OS: /private/tftpboot/

$ tftp localhost
tftp> status
Connected to localhost.
Mode: netascii Verbose: off Tracing: off
Rexmt-interval: 5 seconds, Max-timeout: 25 seconds
tftp> get m1
Received 4225655 bytes in 0.6 seconds
tftp> quit
```

> https://wiki.openwrt.org/doc/howto/generic.flashing.tftp#tftpd_server_on_mac_os_x_lion

