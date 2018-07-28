---
layout: post
title: Ubuntu安装Jenkins
author: Kehao Wu
tags:
 - Ubuntu
 - Jenkins
---

```
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt-get update

sudo apt-get install openjdk-8-jdk

sudo apt-get install jenkins
```

[https://blog.csdn.net/u013360850/article/details/78245039](https://blog.csdn.net/u013360850/article/details/78245039)
