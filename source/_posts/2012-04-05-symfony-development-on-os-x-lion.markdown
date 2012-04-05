---
layout: post
title: "Symfony Development on OS X Lion"
date: 2012-04-05 09:34
comments: true
categories: 
- php
- symfony
- programming
- apache
- mac os x
- lion
- mysql
published: true
---

So, I've got my first tiny little hiccup in my learning of PHP through Symfony...
I'm used to running [Debian](http://www.debian.org) web servers and having the
convenience of apt-get to install and magically configure a basic LAMP stack for
me. But, I'm going to be doing my development on OS X Lion which doesn't have that.
Luckily, Lion does have Apache and PHP included (but not MySQL) they just need
to be configured and in the case of MySQL, installed.

Well, lets get started with: Apache.

## Setting up Apache and PHP ##

OS X Lion comes with Apache by default, we just need to turn it on and configure
it. On my machine I have Server.app installed making it an OS X server which
changes things just a little bit, so I'll document for both Server.app and
non-Server.app

### Without Server.app ###

First, we have to enable Web Sharing

1. Open System Preferences
2. Goto Sharing, in the Internet & Wireless category
3. Put a check next to Web Sharing

Now, we have to enable PHP support and userdir support so that we can use the
Sites/ directory in your home folder.

1. Open Terminal
2. Run: `sudo nano /private/etc/apache2/httpd.conf`
3. Find and uncomment the following lines: (remove the # from the beginning)
        LoadModule php5_module libexec/apache2/libphp5.so
        LoadModule apple_userdir_module libexec/apache2/mod_userdir_apple.so
4. Restart apache: `sudo apachectl restart`

### With Server.app ###

First, we have to enable Web Server

1. Open Server.app
2. Connect to your local machine
3. Goto Web
4. Check "Enable PHP web applications"
5. Flip the switch to On

Now, we have to enable userdir support so that we can use the Sites/ directory
in your home folder.

1. Open Terminal
2. Run: `sudo nano /private/etc/apache2/httpd.conf`
3. Find and uncomment the following line: (remove the # from the beginning)
        LoadModule apple_userdir_module libexec/apache2/mod_userdir_apple.so
4. Restart apache: `sudo apachectl restart`

### Let's Test It! ###

Let's just test our PHP and userdir support with a simple phpinfo().

1. Open ~/Sites/
2. Create a file: info.php with a phpinfo() statement
3. Open a web browser to http://localhost/<username>/info.php
4. If you see the phpinfo screen then everything worked perfectly, otherwise go back and retry.
``` php info.php
<?php phpinfo(); ?>
```

## Setting up MySQL ##

OS X Lion unfortunately does not bundle MySQL with it as well. So we'll have
to go download and install it ourselves. We'll also grab a couple of other nice
GUI tools from the MySQL project that can come in handy later.

1. [Download](http://www.mysql.com/downloads/mysql/) the MySQL DMG (Mac OS X ver. 10.6 (x86, 64-bit), DMG Archive)
2. Install MySQL Community Server with the default values from the DMG you just downloaded

While it is possible to run the MySQL server from a command line by typing in
`/usr/local/mysql/bin/mysqld_safe &`, it gets pretty old fast. So, let's install
the startup item and PrefPane.

1. Open the MySQL Community Server DMG
2. Install MySQLStartupItem.pkg
3. Double click on MySQL.prefPane, choosing to "Install for All Users"
4. Using the new MySQL Preferences Pane in the category Other, you can start and stop MySQL and choose whether or not it should run on startup

Now there are a few handy tools that I like to install for MySQL, but they are
entirely optional. All of them are free:

* [MySQL Workbench](http://www.mysql.com/downloads/workbench/)
* [Sequel Pro](http://www.sequelpro.com/)
* [phpMyAdmin](http://www.phpmyadmin.net/home_page/) _Only if you are comfortable enough installing this by hand_
