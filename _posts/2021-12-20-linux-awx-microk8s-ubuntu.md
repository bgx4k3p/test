---
date: 2022-12-20 12:26:40
layout: post
title: Install AWX with MicroK8s cluster on Ubuntu Server
subtitle: How-To Guide
description: Step by step instructions to set up AWX Tower for running automated Ansible playbooks and server administration.
image: /img/awx-large.png
optimized_image: /img/awx-large.png
category: linux
tags:
  - linux
  - awx
  - ansible
  - kubernetes
  - helm
  - microk8s
author: bgx4k3p
---

Works on post, but not on home page: image: ../assets/img/awx-large.png

## 1. Prerequisites

- Ubuntu Server with MicroK8s cluster: <https://bgx4k3p.github.io/blog/linux/2021/12/24/kubernetes-microk8s-ubuntu.html>
- Make sure to have over 6Gb RAM and 4CPUs! AWX won't install properly otherwise.

## 2. Enable Required AddOns

```bash
ansible@kube:~$ microk8s enable dns hostpath-storage ingress rbac helm
```

## 3. Install AWX Operator

AWX Operator installs AWX and PostgreSQL container automatically. Helm charts make the install and upgrade very easy.

```bash
ansible@kube:~$ microk8s helm repo add awx-operator https://ansible.github.io/awx-operator/
ansible@kube:~$ microk8s helm repo update
ansible@kube:~$ microk8s helm install -n awx --create-namespace awx awx-operator/awx-operator

# Output
NAME: awx
LAST DEPLOYED: Mon Apr  3 00:07:39 2023
NAMESPACE: awx
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
AWX Operator installed with Helm Chart version 1.4.0

# Check status
ansible@kube:~$ kubectl get pods -n awx
ansible@kube:~$ kubectl get pods -A
```

## 4. Install AWX

```bash
# Switch to the AWX namespace
ansible@kube:~$ kubectl config set-context --current --namespace=awx

# Create AWX configuration and apply
ansible@kube:~$ cd ~
ansible@kube:~$ cat << EOF > awx.yaml
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
spec:
  service_type: nodeport
EOF

ansible@kube:~$ kubectl apply -f awx.yaml

# Optional: Open another terminal to monitor the install
ansible@kube:~$ kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager

```

Wait until all 6 AWX pods are ready, takes a couple of minutes.

```bash
# Check status of pods
ansible@kube:~$ kubectl get pods -n awx
```

Example:

```bash
ansible@kube:~$ kubectl get pods -n awx
NAME                                               READY   STATUS            RESTARTS   AGE
awx-operator-controller-manager-5678bcf484-snqnk   2/2     Running           0          4m10s
awx-postgres-13-0                                  1/1     Running           0          107s
awx-96d4765c-rz8n4                                 0/4     PodInitializing   0          62s
```

## 5. Port Forward

```bash
# Find the Port/IP
ansible@kube:~$ kubectl get service -A

# Output
ansible@kube:~$ kubectl get service -A

NAMESPACE     NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes                                        ClusterIP   10.152.183.1     <none>        443/TCP                  9m58s
kube-system   metrics-server                                    ClusterIP   10.152.183.66    <none>        443/TCP                  7m56s
kube-system   kubernetes-dashboard                              ClusterIP   10.152.183.76    <none>        443/TCP                  7m52s
kube-system   dashboard-metrics-scraper                         ClusterIP   10.152.183.207   <none>        8000/TCP                 7m52s
kube-system   kube-dns                                          ClusterIP   10.152.183.10    <none>        53/UDP,53/TCP,9153/TCP   7m23s
awx           awx-operator-controller-manager-metrics-service   ClusterIP   10.152.183.21    <none>        8443/TCP                 6m26s
awx           awx-postgres-13                                   ClusterIP   None             <none>        5432/TCP                 3m24s
awx           awx-service                                       NodePort    10.152.183.67    <none>        80:31589/TCP             2m42s

# Port Forward (Optional)
ansible@kube:~$ microk8s kubectl port-forward -n awx service/awx-service 31589:80 --address 0.0.0.0 &> /dev/null &

```

## 6. Login AWX

```bash
# Get the Admin password
ansible@kube:~$ echo Username: admin$'\n'Password: `kubectl  get secret awx-admin-password -o jsonpath='{.data.password}' | base64 --decode`
```

Login: <http://x.x.x.x:31589>
