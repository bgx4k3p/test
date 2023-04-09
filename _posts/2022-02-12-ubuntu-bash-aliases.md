---
date: 2022-02-12 12:26
layout: post
title: Useful BASH Aliases for Ubuntu
subtitle: Quick notes
description: How to add useful BASH aliases for root user on Ubuntu.
image: https://bgx4k3p.github.io/test/assets/img/bash.jpg
category: linux
tags: git bash ubuntu
author: bgx4k3p
paginate: true
---

## 1. Switch to root user

Any user with SUDO privileges can be used as well, it's a matter of preference.

```bash
sudo su
```

## 2. Create .bash_aliases file under the root user home folder

```bash
# Updates
echo 'alias update="apt-get update && apt-get -y upgrade && apt-get -y dist-upgrade && apt-get autoclean && apt-get -y autoremove && apt-get clean"' | sudo tee /root/.bash_aliases

# Install Kernel Headers
echo 'alias install-headers="sudo apt-get install linux-headers-$(uname -r)"' | sudo tee -a /root/.bash_aliases

# Cleanup Old Kernels
echo 'alias remove-old-kernels="echo $(dpkg --list | grep linux-image | awk '\''{ print $2 }'\'' | sort -V | sed -n '\''/'\''`uname -r`'\''/q;p'\'') $(dpkg --list | grep linux-headers | awk '\''{ print $2 }'\'' | sort -V | sed -n '\''/'\''"$(uname -r | sed "s/\([0-9.-]*\)-\([^0-9]\+\)/\1/")"'\''/q;p'\'') | xargs sudo apt -y purge && update-grub"' |  sudo tee -a /root/.bash_aliases
```

## 3. Refresh profile

```bash
source ~/.bashrc
```

## 4. Try it
