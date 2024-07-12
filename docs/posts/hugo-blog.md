--- 
date: 2020-04-13
slug: hugo-blog
title: Setting up Personal Blog with Hugo (Go)

authors:
  - kono94 
categories:
  - Programming
tags:
  - Go
  - Hugo
---

1. Install GO on your machine and edit your PATH variable. Add this to your .bashrc or .zshrc:
```
export GOPATH="${HOME}/.go"
export GOROOT="$(brew --prefix golang)/libexec"
export PATH="$PATH:${GOPATH}/bin:${GOROOT}/bin"
```
 and use brew to install go and some dev tools (optional):
 <!-- more -->
```
brew install go

go get golang.org/x/tools/cmd/godoc

go get github.com/golang/lint/golint
```
2. Install hugo:

```
brew install hugo
```
3. Create a new hugo website:
```
hugo new site example-blog
```
4. Add a theme of your choice, this site uses ``hugo-goa``
```
mkdir -p themes
cd themes
git clone https://github.com/shenoybr/hugo-goa
``` 
5. Check out the config.toml examples of the theme and customize your own config. My config as example:
```
## Basic Configuration

baseurl = "https://jan.lwenstrom.net/"
builddrafts = false
canonifyurls = false
languageCode = "en-US"

contentdir = "content"
layoutdir = "layouts"
publishdir = "public"

author = "Jan Löwenstrom"
title = "Jan Löwenstrom"

theme = "hugo-goa"

## Hugo Built-in Features
# disqusShortname = "https-shenoybr-github-io-hugo-goa-demo" # Comment out to disable Disqus.
googleAnalytics = "XYZ"
enableRobotsTXT = true

## Site Settings
[params]
author = "Jan Löwenstrom"
intro = "Software Engineer"
description = "Ah ja!"
authorimage = "headshot.png"
dateformat = "Jan 2, 2019"

## Site Meta Settings
[params.meta]
description = "Person Blog"
keywords = "portfolio,blog,developer"

## Social Accounts
[params.social]
github = "kono94"
xing = "<username>"
linkedin = "<username>"
#twitter = "<username>"
#stackoverflow = "<username>"
#bitbucket = "<username>"
#steam = "<username>"
email = "jan@lwenstrom.net"

## Extras
[params.extra]
#copyright = "© 2016. Erlich Bachman. [Some Rights Reserved](http://creativecommons.org/licenses/by/3.0/)."
poweredby = true
highlightjs = true
socialmarkup = true
toc = true
displayrssicon = true

## Main Menu
[[menu.main]]
    name = "blog"
    weight = 100
    identifier = "blog"
    url = "/blog/"
[[menu.main]]
    name = "about"
    identifier = "about"
    weight = 200
    url = "/about/"
[[menu.main]]
    name = "coderag"
    identifier = "coderag"
    weight = 300
    url = "/coderag/"
```

6. Add content:
```
hugo new blog/my-first-post.md
hugo new about/about.md
hugo new coderag/code-stuff.md
```
7. Start the hugo server and visit ``localhost:1313`` in your browser:
```
hugo server
```
8. Build your project and upload it to your host:
```
hugo && rsync -avz  public/ ssh-alias:/home/jan/public
```

<b> Done :D </b>
