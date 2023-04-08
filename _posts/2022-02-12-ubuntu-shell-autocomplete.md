---
title: "Add Autocomplete for BASH shell for Ubuntu"
categories: linux
tags: linux ubuntu bash
---

How to add useful autocomplete feature to BASH shell on Ubuntu

## 1. Switch to root user

I usually create it under **root** and then replicate to any other users on the system. It's a matter of preference.

```bash
sudo su
```

## 2. Create .inputrc file in the user home folder

```bash
echo '"\e[A": history-search-backward' | sudo tee /root/.inputrc
echo '"\e[B": history-search-forward' | sudo tee -a /root/.inputrc
echo '"\e[C": forward-char' | sudo tee -a /root/.inputrc
echo '"\e[D": backward-char' | sudo tee -a /root/.inputrc
echo 'set completion-ignore-case On' | sudo tee -a /root/.inputrc
```

## 3. Copy the file to other users

```bash
sudo cp -f /root/.inputrc /home/bgx4k3p/.inputrc
sudo cp -f /root/.inputrc /home/ansible/.inputrc
```

## 4. Fix Permissions

```bash
sudo chown ansible:ansible /home/ansible/.inputrc
sudo chown bgx4k3p:bgx4k3p /home/bgx4k3p/.inputrc
``
