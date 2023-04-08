---
title: Create Ansible User for AWX Tower
categories: linux
tags: linux ansible ubuntu awx
---

Quick guide to add new user for Ansible AWX

## 1. Add Ansible user

```bash
sudo useradd -m -s /bin/bash ansible
```

## 2. SUDO without password

```bash
echo "ansible ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansible
```

## 3. Add your Public SSH key

```bash
sudo mkdir /home/ansible/.ssh

echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDVt4bIndLhMKLTatW7vGKMB/rAOsZGXwyGKvRNdz/wBWm5QplzfQoI080eBbKf4yVDEq8hmVAsQbjkVNkdlyOZgzF2RvqO3PFQTDb/yiTCMekvDYNJJLY820IhCbgKIfAgvWLUluVOzgQ9kw7+W+lsOeTVGUA2AVThYxcbc8iu83M9ISXZW55NYmGp96gUgRvZ4rzS9tfIaIX5vH7ypNSAYrxVvA9/IDTKWcZ3PkKOlSGkwFyPZ95nzTp4Mwy0LIkzSTS5UK6g8Oiy8g8EEz0At7whAHNa6S2fl/J/LHR/IKAdWTcbkOgtUvZPXAv41Z9F6k1RFd9h01RVV3A8SJ94FUVQ9vuoM1jhZjULMEbIvRZVZpQR5TRIs5yKgl6cJllj2g9wRI227qdq1HEEF9eWsX40f3g3HkD90gSgGuicTTbnbOx26j84EALunqMVb8tKK0mv8xvBz2MzOoHsFyt93CH04iT29jBkn7Yxt/Vx+p1kQJcaWmOvHcELoi2tHWU= " | sudo tee /home/ansible/.ssh/authorized_keys
```

## 4. Fix Permissions

```bash
sudo chown -R ansible:ansible /home/ansible/
sudo chmod 700 /home/ansible/.ssh
```

## 5. Disable Password login

```bash
sudo usermod -L ansible
```
