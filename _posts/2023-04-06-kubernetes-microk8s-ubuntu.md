---
date: 2023-04-06 12:22
layout: post
title: Setup MicroK8s cluster on Ubuntu Server
subtitle: How-to
description: Step by step instructions to set MicroK8s cluster on Ubuntu 22.04.
image: https://bgx4k3p.github.io/test/assets/img/awx-large.png
category: linux
tags: linux awx ansible kubernetes helm microk8s
author: bgx4k3p
paginate: true
---

## 1. Install MicroK8s and Kubectl

```bash
sudo snap install microk8s --classic
sudo snap install kubectl --classic
```

## 2. Setup permissions

```bash
# Add current user and gain access to .kube caching
sudo usermod -a -G microk8s $USER
newgrp microk8s

# Verify the installation
kubectl version --client
microk8s status
```

## 3. Connect Kubectl with MicroK8s

```bash
cd $HOME
mkdir .kube
cd .kube
microk8s config > config
kubectl config get-contexts
```

## 4. Start the cluster and check status

```bash
microk8s start && sleep 5
microk8s kubectl get nodes
microk8s kubectl get pods -A

```

## 5. Setup Dashboard

```bash
# Enable Addons
microk8s enable dashboard

# Create Default token
microk8s kubectl create token default

## OPTIONAL - Use RBAC
# Enable RBAC Addon
microk8s enable rbac

# Create the Admin user: dashboard-adminuser.yaml
cd ~
cat << EOF > dashboard-adminuser.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
EOF

microk8s kubectl apply -f dashboard-adminuser.yaml

# Create the Admin role binding: adminuser-rolebinding.yaml

cd ~
cat << EOF > adminuser-rolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
EOF

microk8s kubectl apply -f adminuser-rolebinding.yaml

# Copy the Token for MicroK8s 1.23 or older
microk8s kubectl -n kube-system describe secret $(microk8s kubectl -n kube-system get secret | grep admin-user | awk '{print $1}') | grep token

# Copy the for MicroK8s 1.24 or newer
microk8s kubectl create token default

```

## 6. Port Forward

```bash
# Lookup the IP and Port of the container
kubectl get service -A

# Port Forward
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0 &> /dev/null &
```

## 7. Access the Dashboard

<https://IP:10443/>
