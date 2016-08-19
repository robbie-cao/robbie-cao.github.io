---
layout: post
title: tftpd Server on Ubuntu
categories: linux
---

## TFTP Server Install and Setup

1. Install following packages.

```
$ sudo apt-get install xinetd tftpd tftp
```

2. Create `/etc/xinetd.d/tftp` and put this entry

```
service tftp
{
	protocol        = udp
	port            = 69
	socket_type     = dgram
	wait            = yes
	user            = nobody
	server          = /usr/sbin/in.tftpd
	server_args     = /tftpboot
	disable         = no
}
```

3. Create a folder /tftpboot this should match whatever you gave in server_args. mostly it will be tftpboot

```
$ sudo mkdir /tftpboot

$ sudo chmod -R 775 /tftpboot
-or-
$ sudo chmod -R 777 /tftpboot

$ sudo chown -R nobody /tftpboot
```

4. Restart the `xinetd` service.

Newer systems:

```
$ sudo service xinetd restart
```

Older systems:

```
$ sudo /etc/init.d/xinetd restart
```

5. Now our tftp server is up and running.

## Testing TFTP Server

6. Create a file named `test` with some content in `/tftpboot` path of the tftp server

   Obtain the ip address of the tftp server using `ifconfig` command

7. Now in some other system follow the following steps.

```
$ tftp 192.168.1.2
tftp> get test
Received 159 bytes in 0.0 seconds

tftp> quit

$ cat test
```

## Reference

- http://askubuntu.com/questions/201505/how-do-i-install-and-run-a-tftp-server

