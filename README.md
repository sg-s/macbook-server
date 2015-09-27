# How to set up an old MacBook as a server

![](images/macbook.png)

The `BlackServer`

<!-- MarkdownTOC -->

- [0. Why do such a crazy thing?](#0-why-do-such-a-crazy-thing)
    - [0.1 because you can host your own website](#01-because-you-can-host-your-own-website)
    - [0.2 awesome backups](#02-awesome-backups)
    - [0.3 because you can host your own media server](#03-because-you-can-host-your-own-media-server)
    - [0.4 because the internet is broken](#04-because-the-internet-is-broken)
    - [0.5 because you can make a mean machine](#05-because-you-can-make-a-mean-machine)
- [1. What this document ...](#1-what-this-document-)
    - [1.1 What this document is](#11-what-this-document-is)
    - [1.2 What this document is **not**](#12-what-this-document-is-not)
- [2. Basic Configuration](#2-basic-configuration)
    - [2.1 Operating System](#21-operating-system)
    - [2.2 XCode Command Line Tools](#22-xcode-command-line-tools)
    - [2.3 Install `brew`](#23-install-brew)
    - [2.4 Install things using `brew`](#24-install-things-using-brew)
    - [2.4 Install some things that brew can't](#24-install-some-things-that-brew-cant)
    - [2.5 Set up a local static IP](#25-set-up-a-local-static-ip)
    - [2.6 Enable remote access](#26-enable-remote-access)
    - [2.7 Configure MAMP](#27-configure-mamp)
    - [2.4 Configure a global name](#24-configure-a-global-name)
    - [2.5 composer](#25-composer)
- [3. Install applications on your server](#3-install-applications-on-your-server)
    - [3.0 A File upload service](#30-a-file-upload-service)
    - [3.1 wallabag](#31-wallabag)
        - [Modify your `$PATH` variable](#modify-your-path-variable)
        - [Modify your php.ini file](#modify-your-phpini-file)
        - [Install wallabag](#install-wallabag)
- [Caveats, Workarounds, and Bugs](#caveats-workarounds-and-bugs)
    - [CPU usage jumps to 100%, fans spin](#cpu-usage-jumps-to-100%-fans-spin)
        - [Fix 1](#fix-1)
        - [Fix 2](#fix-2)
- [References](#references)

<!-- /MarkdownTOC -->


# 0. Why do such a crazy thing? 

### 0.1 because you can host your own website

Why pay `$$` to rent a crappy computer somewhere else? Host your own website on your terms. 

### 0.2 awesome backups 

Wouldn't it be awesome to have your computer backup wirelessly, automatically, no matter where you are? 

### 0.3 because you can host your own media server

Imagine having a personalised YouTube, filled with your own collection of awesome movies and TV shows and what have you. 

### 0.4 because the internet is broken 

Don't let [*your* personal information be the currency with which you pay for essential services](http://www.bostonglobe.com/business/2015/01/23/snowden-nsa-face-off-over-privacy-harvard/7S0HX1SaCO1MlZL70JC2mK/story.html) on the internet. You shouldn't have to relinquish privacy just to read a stupid email.

Here is a handy table listing services you probably use, alternatives you can host yourself, and the status of the offered solution (if any)

<table class="center">
  <tr>
    <td>
        <b>Service</b>
    </td>
    <td>
        <b>Example</b>
    </td>
    <td>
        <b>Alternative</b>
    </td>
    <td>
        <b>Status</b>
    </td>
  </tr>

  <tr>
    <td>
        email
    </td>
    <td>
        GMail
    </td>
    <td>
        ??
    </td>
    <td>
        ??
    </td>
  </tr>

 <tr>
    <td>
        static webserver
    </td>
    <td>
        <a href="https://en.wikipedia.org/wiki/DreamHost"> DreamHost</a>
    </td>
    <td>
        <a href = "http://www.mamp.info/en/">MAMP</a> + your own server
    </td>
    <td>
        working
    </td>
  </tr>

  <tr>
    <td>
        file upload
    </td>
    <td>
        <a href="https://mega.co.nz">Mega</a>
    </td>
    <td>
        <a href = "https://github.com/sg-s/upload">upload</a> + your own server
    </td>
    <td>
        working
    </td>
  </tr>

 <tr>
    <td>
        git server
    </td>
    <td>
        <a href="https://github.com/">GitHub</a>
    </td>
    <td>
        <a href = "https://about.gitlab.com/">gitlab</a> + your own server
    </td>
    <td>
        ??
    </td>
  </tr>

  <tr>
    <td>
        A read-it-later service
    </td>
    <td>
        <a href="https://getpocket.com/">Pocket</a>
    </td>
    <td>
        <a href = "https://www.wallabag.org/">wallabag</a> + your own server
    </td>
    <td>
        working
    </td>
  </tr>

  <tr>
    <td>
        web-based YouTube downloader
    </td>
    <td>
        <a href= "https://www.google.com/#safe=off&q=youtube+download">Your favourite spammy site</a>
    </td>
    <td>
        <a href = "https://github.com/sg-s/video">video.php</a> + your own server
    </td>
    <td>
        working
    </td>
  </tr>

  <tr>
    <td>
        Cloud backup
    </td>
    <td>
        <a href= "http://www.drop-dropbox.com/">Dropbox</a>
    </td>
    <td>
        rsync + your own server
    </td>
    <td>
        ??
    </td>
  </tr>

 <tr>
    <td>
        torrent tracker
    </td>
    <td>
        <a href= "https://thepiratebay.se/">The Pirate Bay</a>
    </td>
    <td>
        ??
    </td>
    <td>
        ??
    </td>
  </tr>

 <tr>
    <td>
        personal organiser
    </td>
    <td>
        <a href= "https://evernote.com/">Evernote</a>
    </td>
    <td>
        <a href="https://www.mediawiki.org/wiki/MediaWiki">mediawiki</a> + your server
    </td>
    <td>
        working
    </td>
  </tr>

</table>

### 0.5 because you can make a mean machine 

This is the configuration of my MacBook server:

- 2GB RAM (upgradable to 6GB)
- 128GB SSD
- .5TB of additional on-machine storage (I swapped out my optical drive)
- 4TB of additional, external storage (1 FireWire port, 2 USB ports)

# 1. What this document ...

### 1.1 What this document is

- the way I did this
- a way for me document what I did

### 1.2 What this document is **not**

- The *best* way to do this
- A *secure* way to do this


# 2. Basic Configuration 

### 2.1 Operating System

Because my computer was rather long in the tooth, and didn't have anything on it I was very attached to, I booted from a `Snow Leopard` install disc, and did a fresh install. I then updated to the last version of Snow Leopard and then upgraded to Lion, and then patched Lion with all the updates I could lay my hands on. My final configuration is:

```
Mac OS X 10.7.5 (Lion)
```

I couldn't update to `Mountain Lion` because that was the limit of the `Black MacbookBook`

### 2.2 XCode Command Line Tools 

You need the XCode Command Line Tools to install a number of other things. The last version I could install is `Command Line Tools (OS X Lion) for Xcode - April 2013`

Here's a [link](http://adcdownload.apple.com/Developer_Tools/command_line_tools_os_x_lion_for_xcode__april_2013/xcode462_cltools_10_76938260a.dmg) to the disk image. You'll need a free Apple Developer account to get this. 

### 2.3 Install `brew`

`brew` is a fantastic pckage manger for Mac OS X. You will definitely need this. Install with this single line:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

It will ask you for your password. Enter one. 

### 2.4 Install things using `brew`

Now that brew is installed, it is super easy to install a huge bunch of things. Note that some of these depend on the order you install them. 

```bash
brew install youtube-dl     # download YouTube videos nicely
brew install ruby           # the ruby programming language 
brew install git            # the best version control software
brew install ffmpeg         # the best way to convert video
brew install node           # server-side JavaScript
brew install wget           # download things easily 
brew install syncthing      # libre file synchronization 
brew install moreutils      # installs [moreutils](https://rentes.github.io/unix/utilities/2015/07/27/moreutils-package/)
```

*You can also*  `brew install thing1 thing2`

Wow. we just installed a bunch of useful command line utilities. What about a better browser and some GUI apps? `brew cask` has you covered. 

```bash
brew install caskroom/cask/brew-cask
brew cask install firefox               # the best free browser
brew cask install mamp                  # Apache, mySQL and PHP
brew cask install sublime-text          # the best text editor
brew cask install torbrowser            # simple anonymity 
brew cask install carbon-copy-cloner    # backups made easy
brew cask install caffeine              # keep your screen on
```

One of the complications(features?) of using `brew` is that it installs its own versions of things that Mac OS X already has (like `git`). To make sure that we always use the `brew` version, and not the OS X version, set your `$PATH` so that the `brew` path occurs before the system path:

```
echo export PATH='/usr/local/bin:$PATH' >> ~/.bash_profile
```

Why do this? Because the `brew` versions are more secure, more recent, and are going to be maintained. 

### 2.4 Install some things that brew can't

#### NoSleep

The latest (v1.40) version of NoSleep is broken on Lion, and we'll have to install an older one. See this [issue](https://github.com/integralpro/nosleep/issues/5) on their repo page. You will have to download v1.3.3 [here](https://code.google.com/p/macosx-nosleep-extension/downloads/detail?name=NoSleep-1.3.3.dmg&can=2&q=) and manually install it. Make sure you enable "don't check for updates" because as of writing, the latest version is broken, and it will break itself trying to update. 

### 2.5 Set up a local static IP

Go to `System Preferences > Network` and set up a static IP as shown:

![](images/static-ip.png)

What we're doing here is making sure that `BlackServer` has the same IP address on the local network (your router's network). This means it's at the same place on the network, and that your router can be told to send messages to the `BlackServer` easily. 

### 2.6 Enable remote access 

We'll need to get in and out of this computer from all over the world soon. And in the beginning at least, it would be nice to have a way to log into the computer and see the screen. Let's set up all this in `System Preferences > Sharing`

![](images/remote.png)

But we're not done yet! For added security, let's configure `BlackServer` so that we can SSH into it only using **public key authentication**. This way, there are no passwords to type in, and it's much harder for someone to break into our computer. 

Assuming you have made your SSH keys on your local machine (*not* `BlackServer`!), copy them to `BlackServer`

```bash
scp ~/.ssh/id_rsa.pub user@blackserver:~/Desktop/
```

Now, on `BlackServer`, run this:


```bash
mkdir ~/.ssh
cat ~/Desktop/id_rsa.pub >> ~/.ssh/authorized_keys
```

OK. `BlackServer` *can* now verify your local machine using public key cryptography. Now we need to configure `BlackServer` to make sure it only does this:

Fire up `Sublime Text` to edit the SSH config file:  

```bash
subl /etc/sshd_config
```
and uncomment these lines 

```bash
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile	.ssh/authorized_keys
```

Also, let's prevent authentication using passwords. Change this file to:

````bash
# To disable tunneled clear text passwords both PasswordAuthentication and
# ChallengeResponseAuthentication must be set to "no".
PasswordAuthentication no
#PermitEmptyPasswords no
# Change to no to disable s/key passwords
ChallengeResponseAuthentication no
````

**WARNING** At this point, you can *only* get into `BlackServer` via public key authentication. If you lose your SSH keys, or if someone steals them, you have a problem.  

### 2.7 Configure MAMP

Start MAMP (be careful not to run MAMP Pro, which will also be installed) and set it up as follows:

![](images/mamp-1.png)
![](images/mamp-3.png)
![](images/mamp-2.png)

OK, let's see if this works (you will have to enter the local IP address of of your computer instead of `black.server`): 

![](images/server-1.png)

Excellent. It looks like MAMP works, and we have an Apache server running. Now, MAMP by default does [not](https://sites.google.com/site/mamppro/en/mamp/faq/where-can-i-find-the-logs/how-can-i-enable-the-apache-access-logs) enable *access logging*. Access logging allows us to see who is trying to connect to the `BlackServer`, and is also a useful debugging tool. To enable this, 

```bash
subl /Applications/MAMP/conf/apache/httpd.conf 
```

And remove the comment (the `#`) from this line

```bash
# CustomLog "/Applications/MAMP/logs/apache_access.log" common
```

Restart your server for this to take effect. You can view the access log using 

``` bash
$ tail -f /Applications/MAMP/logs/apache_access_log
```

### 2.4 Configure a global name 

It would be nice to access `BlackServer` from anywhere in the world. Right now, we can't do that, for two reasons:

1. we don't know where in the internet `BlackServer` is 
2. the router that `BlackServer` is on doesn't know what to do with packets coming from and going to `BlackServer`. In fact, it is designed to ignore everything by default.

Let's address both problems. 

To tell your router how to pass on messages to/from `BlackServer`, you need to do something called **port forwarding**. Unfortunately, every router is different, and some routers are so stupid they don't allow you to do that. You have to figure out how to do this on yours. Go to your router's admin page (usually 192.168.1.1) and enter your username and password (usually admin/admin or something silly)

Find a page that looks like this, and add entries as follows:

![](images/port-forward.png)

The specific ports you forward depend on the applications and services you will install on your server, but you get the idea. 

Now, we need a service that translates a short name into the IP address of `BlackServer` (or more precisely, the IP address of the router `BlackServer` is on). For this, we use a bit of software from no-ip:

```bash
brew cask install no-ip-duc
```

Configuring this piece of software is easy, and you can get it to update a URL you control with the IP address `BlackServer` is on. Let's assume that the domain name you control is `black.server`.

### 2.5 composer

[composer](https://getcomposer.org/) is a dependency manager for PHP. You will need it if you install wallabag. 

Because of the chaos we have unleashed with MAMP, a PHP configuration file called `php.ini` will not be in the "right" place. To fix that, run

```bash
sudo cp /etc/php.ini.default /etc/php.ini
```

And fire up Sublime text and add this line 
```php
detect_unicode = Off
```

using 
```bash
subl /etc/php.ini
```

*hat tip to [Tony Lea](http://www.tonylea.com/2013/os-x-and-composer-error-php-ini-does-not-exist/)*

Finally, we can install composer using the installer:

```bash
curl -s http://getcomposer.org/installer | php
```


# 3. Install applications on your server

## 3.0 A File upload service

Wouldn't it be nice if you could operate your own file upload service? If people wanted to send you documents, they could simply upload it to your computer. No more messing around with [Condi's Dropbox](http://www.drop-dropbox.com/). 

I've written a small file [upload](https://github.com/sg-s/upload) service that works straight out of the box. Grab it with 

```bash
cd /Library/Webserver/Documents/
git clone https://github.com/sg-s/upload
chmod 777 uploads
```

To check if you've done the right thing, trying going to `black.server/upload` from another computer. You should see this:

![](images/upload.png)

Try uploading something. You should get a green message telling you it worked. You can go see the file for yourself on `BlackServer` in `/Library/Webserver/Documents/upload/uploads/`

## 3.1 [wallabag](http://wallabag.org)

Wallabag is a read-it-later service, like [Pocket](http://getpocket.com/) or [Instapaper](https://www.instapaper.com/), but with 100% less data stored on other people's servers, because your data is stored on your BlackServer. 

I found installation [extremely](https://github.com/wallabag/wallabag/issues/1024) [difficult](https://github.com/wallabag/wallabag/issues/1023), but here is a easy way to get everything working. 

### Modify your `$PATH` variable 

Your `$PATH` variable tells `bash` where to look for commands. Edit your ~/.bash_profile to look like:

```bash
export PATH=/Applications/MAMP/bin/php/php5.6.2/bin:/usr/local/bin:$PATH
```

This tells the computer to use MAMP's PHP, instead of the crappy version built into OS X. 

### Modify your php.ini file

Assuming you followed the instructions [here](#25-composer) about the location of your `php.ini` file, add the following lines to your `php.ini`

```
extension_dir = "/Applications/MAMP/bin/php/php5.6.2/lib/php/extensions/no-debug-non-zts-20131226/"
extension = pdo_mysql.so
```

This tells php to load the PDO extension, and points to the right location. 

Check that everything works using `php --version`. You should see this:

```
PHP 5.6.2 (cli) (built: Oct 20 2014 16:21:27) 
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2014 Zend Technologies
```

If you see an error message about missing PDO, you probably haven't fixed your `$PATH`. 

### Install wallabag

Navigate to the Documents folder of your webserver (here, `/Library/WebServer/Documents/`) and run

```bash
curl -s http://getcomposer.org/installer | php
composer create-project wallabag/wallabag wallabag
mv composer.phar wallabag/
cd wallabag
php composer.phar install
```

That's it. Now go to `localhost/wallabag/`

and choose a SQLite database, and a user name and password. 

# Caveats, Workarounds, and Bugs

## CPU usage jumps to 100%, fans spin

This is probably due to runaway perl processes. Check `top` to see which processes are hogging the CPU in this case. 

### Fix 1

Upgrade perl to the latest version using `brew`

```
brew install perl
brew link --force perl
```

### Fix 2

Use [this script](https://github.com/sg-s/auto-bots/blob/master/kill-high-cpu) to kill high-CPU processes. Run this using `cron`. 

# References

1. [Getting started with dotfiles](https://medium.com/@webprolific/getting-started-with-dotfiles-43c3602fd789)
2. [How to run your own e-mail server with your own domain](http://arstechnica.com/information-technology/2014/02/how-to-run-your-own-e-mail-server-with-your-own-domain-part-1/)
3. [The Free Software Foundationn](http://www.fsf.org/)




