# How to set up an old MacBook as a server

# Why do such a crazy thing? 

## because you can host your own website

## because you can host your own media server

## because the internet is broken 

## because you can make a mean machine 

### Specifications of the BlackServer 

# 0. Start with a fresh computer

For my purposes, I installed Snow Leopard onto a BlackBook (the last black MacBook), updated to the last version of Snow Leopard, and then upgraded to Lion, and then patched Lion with all the updates I could lay my hands on. 

# 1. Install Stuff 

### 1.0 First, install the XCode Command Line Tools 

The last version I could install is `Command Line Tools (OS X Lion) for Xcode - April 2013`

Here's a [link](http://adcdownload.apple.com/Developer_Tools/command_line_tools_os_x_lion_for_xcode__april_2013/xcode462_cltools_10_76938260a.dmg) to the disk image. You'll need a free Apple Developer account to get this. 

### 1.1 Install `brew`

`brew` is a fantastic pckage manger for Mac OS X. You will definitely need this. Install with this single line:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

It will ask you for your password. Enter one. 

### 1.2 Install things using `brew`

Now that brew is installed, it is super easy to install a huge bunch of things. Note that some of these depend on the order you install them. 

```bash
brew install youtube-dl
brew install ruby
brew install ffmpeg
```
Wow. we just installed a bunch of useful command line utilities. What about a browser and some GUI apps? `brew cask` has you covered. 

```bash
brew install caskroom/cask/brew-cask
brew cask install firefox
brew cask install mamp
brew cask install sublime-text
brew cask install torbrowser
brew cask install carbon-copy-cloner
```


