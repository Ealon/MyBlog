---
title: Deploy your blog on Github Pages
date: 2017-08-24 23:57:42
tag:
 - Hexo
 - github-pages
 - blog
photos:
 - http://bobochen.com/images/2015/hexo_github.png
---
![](http://bobochen.com/images/2015/hexo_github.png)
## 1. Create your Github Pages
Follow this [intro](https://pages.github.com/) to set up your Github Pages. For example, I created my own repo [ealon.github.io](https://ealon.github.io/).

## 2. Edit and preview your post on your local Hexo Server
run 
```
$ hexo server
```
or simply
```
$ hexo s
```
to start your local Hexo server, then edit your post and preview on the local server.

## 3. Generate your files
Once you finish editting your post, before you publish, run 
```
$ hexo generate
```
or simply
```
$ hexo g
```
to generate files, the generated files will be in the folder `/public`.

## 4. Publish
Move all the files generated in the folder `/public` to your local `username.github.io` repo folder, then commit the update and push to `origin/master`.

## Enjoy !


