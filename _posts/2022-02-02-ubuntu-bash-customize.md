---
date: 2022-02-02 12:26
layout: post
title: Customize BASH shell
subtitle: Add color prompt, commands autocomplete and aliases for BASH shell
description: Add color prompt, commands autocomplete and aliases for BASH shell on Ubuntu. This is a time saver if you tend to build and re-build your servers all the time, like in a home lab.
image: https://bgx4k3p.github.io/blog/assets/img/code-large.jpg
category: linux
tags: linux ubuntu bash
author: bgx4k3p
paginate: true
---

## 1. Switch to root user

I usually create it under **root** and then replicate to any other users on the system. It's a matter of preference.

```bash
sudo su
cd ~
```

## 2. Back up the original files

```bash
mv /root/.bashrc /root/.bashrc_bak
mv /root/.inputrc /root/.inputrc_bak
mv /root/.bash_aliases /root/.bash_aliases_bak
```

Repeat the same for any other user. 

## 3. Download config files

```bash
wget https://raw.githubusercontent.com/bgx4k3p/ansible-playbooks/main/custom-configs/bashrc -O /root/.bashrc
wget https://raw.githubusercontent.com/bgx4k3p/ansible-playbooks/main/custom-configs/inputrc -O /root/.inputrc
wget https://raw.githubusercontent.com/bgx4k3p/ansible-playbooks/main/custom-configs/bash_aliases -O /root/.bash_aliases
```

## 5. Load configs

```bash
source ~/.bashrc
```

## 6. Copy the config to other users

```bash
cp -f /root/.bashrc /home/bgx4k3p/.bashrc
cp -f /root/.inputrc /home/bgx4k3p/.inputrc

cp -f /root/.bashrc /home/ansible/.bashrc
cp -f /root/.inputrc /home/ansible/.inputrc
```

## 7. Fix Permissions

```bash
chown ansible:ansible /home/ansible/.bashrc
chown ansible:ansible /home/ansible/.inputrc

chown bgx4k3p:bgx4k3p /home/bgx4k3p/.bashrc
chown bgx4k3p:bgx4k3p /home/bgx4k3p/.inputrc
```
