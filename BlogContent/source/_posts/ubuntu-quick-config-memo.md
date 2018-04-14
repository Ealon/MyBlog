---
title: Ubuntu Quick Config Memo
tags:
  - linux
  - ubuntu
  - memo
  - tech
photos:
  - >-
    https://raw.githubusercontent.com/Ealon/ealon.github.io/master/images/ealon/default_post_image.png
date: 2018-04-14 10:53:32
---
A quick guide to go through the configuration of a new installed Ubuntu 
<!--more-->

## 1. Install necessary softwares and dependencies
* Chrome
* Visual Studio Code
  * including vscode extensions
  * `Git History` (vscode extension, GUI for git)
* Git
  Use commands 
  ```
  sudo apt-get install git
  ```
  to install git
  And, run
  ```
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
  ```
  to set your account's default identity.
  > Note: Omit --global to set the identity only in this repository.
  
* **Sogou** input (*note it's not 'sougou'*)
* 

## 2. Basic configuration
* Settings-> Time & Date -> Clock
* Settings-> Language Support
  * change **Keyboard input method system** to **fcitx**
* Settings-> Text Entry
  *  check **show current input source in the menu bar**
  *  add **Sogou Pinyin(Fcitx)** to input source (may need to restart system before this action)
* click the ***keyboard icon*** in the top-right corner, and then click **Configure**
  * click **Add input method**, uncheck **Only show current language**, and then choose **Sogou Pinyin**
  
## 3. Development tools
* install curl
  ```bash
  sudo apt install curl
  ```
* Node.js
  
  run 
  ```bash
  curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
  sudo apt-get install -y nodejs
  ```
* [facebook/create-react-app](https://github.com/facebook/create-react-app)
* hexo | [Docs zh-CN](https://hexo.io/zh-cn/docs/index.html)
  ```bash
  npm install -g hexo-cli
  ```
* [serve](https://github.com/zeit/serve)
  ```bash
  npm install -g serve
  ```
---

OK, well done! Now, let's have fun with Ubuntu Linux ~ ! 😃😎 

