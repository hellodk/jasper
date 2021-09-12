---
layout: post
title: Upgrading Sublime Text
date: 2021-09-02 22:48:00
tags: devops
subclass: 'post tag-devops'
author: hellodk
categories: hellodk
cover: false
navigation: True
---
### Upgrade your Sublime Text with the below steps
![](assets/images/upgrade-sublime-text/sublime.jpeg)

#### Environment Details
```
OS Ubuntu-20.04
```

###### From the Sublime official website, download the 64-bit tarball
```bash
wget https://download.sublimetext.com/sublime_text_build_4113_x64.tar.xz
```
You can point the URL to the latest available Sublime Text package.  

###### Backup your existing Sublime Installation
```bash
sudo cp -r /opt/sublime_text/ /opt/sublime_text_backup
```

###### Copy the newly downloaded Sublime files
```bash
sudo cp -r ~/Downloads/sublime_text_build_4113_x64/sublime_text /opt/
```

###### Remove the downloaded files
```bash
rm -rf ~/Downloads/sublime_text_build_4113_x64 ~/Downloads/sublime_text_build_4113_x64.tar.xz
```

###### Remove the backup after a few days
```bash
sudo rm -rf /opt/sublime_text_backup
```

And we're done - Enjoy the new upgrade!!

