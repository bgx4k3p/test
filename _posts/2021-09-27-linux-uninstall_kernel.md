---
date: 2019-04-22 12:20
layout: post
title: Uninstall Old Kernels on Ubuntu
subtitle: Quick notes
description: How to clean up old kernels on Ubuntu linux.
image: https://bgx4k3p.github.io/test/assets/img/bash.jpg
category: linux
tags: linux ubuntu bash
author: bgx4k3p
paginate: true
---


### One liner:

```bash
echo $(dpkg --list | grep linux-image | awk '{ print $2 }' | sort -V | sed -n '/'`uname -r`'/q;p') $(dpkg --list | grep linux-headers | awk '{ print $2 }' | sort -V | sed -n '/'"$(uname -r | sed "s/\([0-9.-]*\)-\([^0-9]\+\)/\1/")"'/q;p') | xargs sudo apt-get -y purge
```
