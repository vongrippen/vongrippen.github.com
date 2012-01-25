---
layout: post
title: "Rev Your Ruby With RVM"
date: 2011-09-29 11:06
comments: true
categories: 
- ruby
- rvm
- gem
- bundler
published: true
---

## What's this RVM thing? ##

{% pullquote %}
Simply: {" RVM is Ruby Version Manager "}.

There you go all done, see you back here next week!

Ok, in all seriousness RVM is a fantastic tool when you are working with
multiple ruby projects. It allows you to install multiple versions (and
implementations) of the ruby interpreter and use different ones for different
projects. It also extends all the way down to your gems, allowing you to have
completely different sets of gems installed for different projects.

I'm not going to go into deep technical details here. If you want to go in
depth, check out the [RVM website](https://rvm.beginrescueend.com/).
{% endpullquote %}

## Ok, but why do I need it? ##

RVM sounds kinda interesting and maybe it might conceivably be useful, but you
don't see how you could ever have an actual use for it right? Wrong!
<!-- more -->
Let's take 2 projects that I have right now:

* Shiny new Rails 3 app that acts as an OpenID provider
* Legacy Rails 2.3.8 project management app that is too big to migrate at the moment

### Rubies of many shades and varieties ###

{% pullquote %}
This Rails 3 app that I'm working on was written specifically with Ruby 1.9 in
mind and takes advantage of a couple of new things so it won't even run on 1.8.7.

Meanwhile my Rails 2.3.8 app has a lot going on in it involving time tracking
and we all know that date handling changed from 1.8 to 1.9. Some would say,
well just go fix that 2.3.8 app to run on 1.9. Unfortunately this is not an
option because there are a lot of other things going on that are different from
1.8 to 1.9 and I just don't have the time right now.

{" So I clearly have to have both 1.8.7 and 1.9.2 installed, but how? "} Here is where RVM comes in.
{% endpullquote %}

Using RVM I can install both versions and switch between them. Here's what I mean:

	# The system-provided ruby
	$ ruby -v
	ruby 1.8.7 (2010-01-10 patchlevel 249) [universal-darwin11.0]

	# Now let's use RVM's ruby 1.8.7
	$ rvm use 1.8.7
	Using /Users/vongrippen/.rvm/gems/ruby-1.8.7-p334
	$ ruby -v
	ruby 1.8.7 (2011-02-18 patchlevel 334) [i686-darwin10.7.0]

	# Now lets use ruby 1.9.2
	$ rvm use 1.9.2
	Using /Users/vongrippen/.rvm/gems/ruby-1.9.2-p180
	$ ruby -v
	ruby 1.9.2p180 (2011-02-18 revision 30909) [x86_64-darwin11.1.0]
	
	# I could even switch to another variety of ruby even, like JRuby
	$ rvm use jruby
	Using /Users/vongrippen/.rvm/gems/jruby-1.6.2
	$ ruby -v
	jruby 1.6.2 (ruby-1.8.7-p330) (2011-05-23 e2ea975) (Java HotSpot(TM) 64-Bit Server VM 1.6.0_26) [darwin-x86_64-java]

As I work I can change ruby versions at will, pretty useful isn&#39;t it?

### Gems of every shape and size ###

{% pullquote %}
Rubygems allows me to install multiple versions of a gem so I should be good,
right? Alas, no.

My Rails 3 app has a dependency on rubygems 1.7 or higher, but the rails 2.3
app will only run if rubygems is version 1.4.2 or lower! This is more than just
a slight problem for me since I'm doing development work in both and can't be
reinstalling rubygems all day long when I switch. Not to mention that they
require different versions of rack that interfere with each other.

Here, {" RVM saves the day again with gemsets. "} RVM includes another feature
called gemsets, which are complete rubygems environments all bottled up and
separated.

Let me show you an example:

``` bash The gemset for my Rails 2.3.8 app
$ rvm use 1.8.7@rails2.3.8
Using /Users/vongrippen/.rvm/gems/ruby-1.8.7-p334 with gemset rails2.3.8
$ gem -v
1.4.2
$ gem list -l

*** LOCAL GEMS ***

actionmailer (2.3.8)
actionpack (2.3.8)
activerecord (2.3.8)
activeresource (2.3.8)
activesupport (2.3.8)
rack (1.1.2)
rails (2.3.8)
rake (0.8.7)
sqlite3 (1.3.4)
sqlite3-ruby (1.3.3)
```

``` bash Using the rails3.1 gemset I've created
$ rvm use 1.9.2@rails3.1
Using /Users/vongrippen/.rvm/gems/ruby-1.9.2-p180 with gemset rails3.1
$ gem -v
1.8.10
$ gem list -l

*** LOCAL GEMS ***

actionmailer (3.1.0)
actionpack (3.1.0)
activemodel (3.1.0)
activerecord (3.1.0)
activeresource (3.1.0)
activesupport (3.1.0)
arel (2.2.1)
bcrypt-ruby (3.0.1)
builder (3.0.0)
bundler (1.0.20)
erubis (2.7.0)
hike (1.2.1)
i18n (0.6.0)
mail (2.3.0)
mime-types (1.16)
multi_json (1.0.3)
polyglot (0.3.2)
rack (1.3.3)
rack-cache (1.0.3)
rack-mount (0.8.3)
rack-ssl (1.3.2)
rack-test (0.6.1)
rails (3.1.0)
railties (3.1.0)
rake (0.8.7)
rdoc (3.9.4)
sprockets (2.0.0)
thor (0.14.6)
tilt (1.3.3)
treetop (1.4.10)
tzinfo (0.3.29)
```

So now I can even switch the entire set of gems from one project to the next
without them interfering! Life just got a little bit easier when doing ruby
development.

{% endpullquote %}

## Sounds useful, let's install it! ##

Installing it is pretty easy, especially if you are on a Mac. Even still it's
easy on Linux too, you just have to install some extra libraries that most
distributions don't include by default. If you are on Windows? Sorry, but I'm
not much help there. I don't use Windows at all anymore, so I haven't the
slightest idea how to install rvm there or if it is even possible. Your best
bet is RVM's [official documentation](https://rvm.beginrescueend.com/).

### Got a shiny Mac here, where to start? ###

First things first, do you have [Xcode](http://developer.apple.com/xcode/) installed?

If not you will need to install it. On 10.6 (Snow Leopard) or earlier, look
for Xcode 3 since it is free. 10.6 can also run Xcode 4.0, but it costs $5
from the App Store. I'm using OS X 10.7 Lion, which means I'll be using Xcode
4.1 which is free from the [Mac App Store](http://itunes.apple.com/us/app/xcode/id448457090?mt=12).

Also, if you are running Xcode 3 you will need to install [Git](http://code.google.com/p/git-osx-installer/).

Now that you have Xcode installed it's time to get our hands dirty in a terminal.

Let's download and run the RVM installer all in one simple command

	bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)
	
Now, we need to add RVM to the end of your bash .profile using your favorite
text editor (if you are using zsh, this file is ~/.zshrc)

``` bash ~/.profile
  [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.
```

Now, either reload your profile by running `source ~/.profile` or restart your shell to load RVM.

Alright, RVM is installed but we still need to install a few rubies, skip past
the part on installing RVM in Linux and lets get some rubies.

### Sometimes I like to get down with a little Linux ###

There is a decent sized list of dependencies that may or may not be installed
on your system already, I've listed the install commands for some of the most
common distributions below. If your distro is not on the list, after running
the base RVM install (before installing any ruby versions), run `rvm requirements`
to see the list of dependencies.

The absolute bare minimum just to get RVM installed is a system copy of ruby
and git, but that's still not a necessarily functional RVM.

``` bash openSUSE
sudo zypper install -y patterns-openSUSE-devel_basis ruby git gcc-c++ bzip2 readline-devel zlib-devel libxml2-devel libxslt-devel libyaml-devel libopenssl-devel libffi45-devel libtool bison
```

``` bash Debian/Ubuntu
sudo apt-get install ruby-full build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison
```

``` bash Fedora/CentOS
yum install -y ruby git-core gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel
yum install -y make bzip2 autoconf automake libtool bison
yum install -y iconv-devel
```

``` bash Arch
pacman -Sy --noconfirm ruby git gcc patch curl zlib readline libxml2 libxslt git autoconf diffutils make libtool bison
```

``` bash Gentoo
emerge ruby libiconv readline zlib openssl curl git libyaml sqlite libxslt libtool bison
```

Now that you have all of your depedencies and libraries all installed we can
go about installing RVM.

	bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
	
Now, we need to add RVM to the end of your bash .profile using your favorite
text editor (if you are using zsh, this file is ~/.zshrc)

``` bash ~/.profile
  [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.
```

Now, either reload your profile by running `source ~/.profile` or restart your shell to load RVM.

## Let's pick out some rubyies ##

Now that you have RVM itself installed we still need to install at least 1 ruby
interpreter. Since I'm using OS X Lion, I'm going to install 1.8.7, 1.9.2, and
macruby. MacRuby is a little bit special, because it is Mac only and requires
that the LLVM compiler suite be installed.

*A full list of available rubies for installation can be found by running `rvm list known`*

	$ rvm install 1.8.7
	Installing Ruby from source to: /Users/vongrippen/.rvm/rubies/ruby-1.8.7-p352, this may take a while depending on your cpu(s)...

	ruby-1.8.7-p352 - #fetching 
	ruby-1.8.7-p352 - #downloading ruby-1.8.7-p352, this may take a while depending on your connection...
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100 4108k  100 4108k    0     0   180k      0  0:00:22  0:00:22 --:--:--  235k
	100 4108k  100 4108k    0     0   178k      0  0:00:22  0:00:22 --:--:--  178kruby-1.8.7-p352 - #extracting ruby-1.8.7-p352 to /Users/vongrippen/.rvm/src/ruby-1.8.7-p352
	ruby-1.8.7-p352 - #extracted to /Users/vongrippen/.rvm/src/ruby-1.8.7-p352
	ruby-1.8.7-p352 - #configuring 
	ruby-1.8.7-p352 - #compiling 
	ruby-1.8.7-p352 - #installing 
	Removing old Rubygems files...
	Installing rubygems-1.8.10 for ruby-1.8.7-p352 ...
	Installation of rubygems completed successfully.
	ruby-1.8.7-p352 - adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
	ruby-1.8.7-p352 - #importing default gemsets (/Users/vongrippen/.rvm/gemsets/)
	Install of ruby-1.8.7-p352 - #complete

	$ rvm install 1.9.2
	Installing Ruby from source to: /Users/vongrippen/.rvm/rubies/ruby-1.9.2-p290, this may take a while depending on your cpu(s)...

	ruby-1.9.2-p290 - #fetching 
	ruby-1.9.2-p290 - #downloading ruby-1.9.2-p290, this may take a while depending on your connection...
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100 8604k  100 8604k    0     0   695k      0  0:00:12  0:00:12 --:--:--  939k
	ruby-1.9.2-p290 - #extracting ruby-1.9.2-p290 to /Users/vongrippen/.rvm/src/ruby-1.9.2-p290
	ruby-1.9.2-p290 - #extracted to /Users/vongrippen/.rvm/src/ruby-1.9.2-p290
	Fetching yaml-0.1.4.tar.gz to /Users/vongrippen/.rvm/archives
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100  460k  100  460k    0     0   409k      0  0:00:01  0:00:01 --:--:--  435k
	Extracting yaml-0.1.4.tar.gz to /Users/vongrippen/.rvm/src
	Configuring yaml in /Users/vongrippen/.rvm/src/yaml-0.1.4.
	Compiling yaml in /Users/vongrippen/.rvm/src/yaml-0.1.4.
	Installing yaml to /Users/vongrippen/.rvm/usr
	ruby-1.9.2-p290 - #configuring 
	ruby-1.9.2-p290 - #compiling 
	 ruby-1.9.2-p290 - #installing 
	Removing old Rubygems files...
	Installing rubygems-1.8.10 for ruby-1.9.2-p290 ...
	Installation of rubygems completed successfully.
	ruby-1.9.2-p290 - adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
	ruby-1.9.2-p290 - #importing default gemsets (/Users/vongrippen/.rvm/gemsets/)
	Install of ruby-1.9.2-p290 - #complete

	$ rvm install macruby
	Retrieving MacRuby 0.10 ...
	Archive:  /Users/vongrippen/.rvm/archives/MacRuby%200.10.zip
	  inflating: /Users/vongrippen/.rvm/src/macruby-0.10/MacRuby 0.10.pkg  
	Password: <Your administrative password>
	
Pretty easy wasn't it? Now you have several versions of ruby installed and
available to you.

## But wait, there's more! ##

You've only gotten started on the capabilities here, there's still gemsets, 
rvmrc, and more that I'm not going to cover in depth here. For more information
on those huge timesavers, checkout RVM's [website](http://rvm.beginrescueend.com).