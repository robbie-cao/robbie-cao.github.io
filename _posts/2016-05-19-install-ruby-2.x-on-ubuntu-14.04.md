---
layout: post
categories: mac osx
---

## Problem

Install ruby on Ubuntu 14.04 with `apt-get` will get ruby 1.9.
But some ruby program will need ruby 2.0+ (eg, `jekyll` for my case).


## Solution

    $ sudo apt-get install curl
    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
    $ curl -L get.rvm.io | bash -s stable
    # config shell login option, refer to https://rvm.io/integration/gnome-terminal
    # add rvm path into .bashrc/.zshrc: `export PATH=$PATH:~/.rvm/bin`
    $ source ~/.bashrc
    # change rvm source server to taobao (optional)
    $ sed -i -e 's/ftp\.ruby-lang\.org\/pub\/ruby/ruby\.taobao\.org\/mirrors\/ruby/g' ~/.rvm/config/db
    $ sudo reboot
    # now you can use rvm after reboot
    $ rvm list known


## Reference

- [Integrating RVM with gnome-terminal](https://rvm.io/integration/gnome-terminal)
- [Ubuntu 14.04中安装ruby on rails环境](http://www.cnblogs.com/piaolingzxh/p/4217480.html)

