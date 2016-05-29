---
layout: post
title: Start a Web Server in Any Folder
categories: cloud
---

## Python

    $ python -m SimpleHTTPServer 8080

Replace `8080` with another number if you want it to listen on a different port.

For ports < 1024 it needs to run with root privileges.

The python 3.x equivalent of this is:

    $ python3 -m http.server


## PHP

    $ php -S 0.0.0.0:8080

## Ruby

Without `gem`

     $ ruby -run -e httpd . -p8080

With `serve`

     $ gem install serve
     $ serve 8080

## Node.js

Use `Connect` and `ServeStatic` with `Node.js`:

1. Install connect and serve-static with NPM

        $ npm install connect serve-static

2. Create server.js file with this content:

        var connect = require('connect');
        var serveStatic = require('serve-static');
        connect().use(serveStatic(__dirname)).listen(8080, function(){
            console.log('Server running on 8080...');
        });

3. Run with Node.js

        $ node server.js

Simplest Node.js server:

    $ npm install http-server -g
    $ http-server

## Reference

- [How to easily start a webserver in any folder?](http://askubuntu.com/questions/377389/how-to-easily-start-a-webserver-in-any-folder)
- [Using node.js as a simple web server](http://stackoverflow.com/questions/6084360/using-node-js-as-a-simple-web-server)

