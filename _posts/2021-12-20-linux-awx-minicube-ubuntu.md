---
date: 2021-12-20 12:24
layout: post
title: Install AWX with Minikube on Ubuntu Server
subtitle: How-to
description: How to setup Minicube cluster on on Ubuntu 21.04.
image: https://bgx4k3p.github.io/test/assets/img/awx-large.png
category: linux
tags: linux awx ansible kubernetes minicube
author: bgx4k3p
paginate: true
---


## 1. Install Minicube

```bash
# Install Minikube dependencies
sudo apt install -y curl wget apt-transport-https ca-certificates

# Download the latest Minikube binary
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

# Move the file in your Bin folder
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
sudo chmod +x /usr/local/bin/minikube

# Check if is working
minikube version
```

## 2. Install Docker

```bash
# Install Docker dependencies
sudo apt-get install -y ca-certificates gnupg lsb-release unzip make
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
docker -v
```

## 3. Setup Docker permissions

```bash
# Create new group and add the current user to it
sudo groupadd docker
sudo usermod -aG docker $USER #ansible
newgrp docker
```

## 4. Install Kubectl

```bash
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
kubectl version
```

## 5. Setup Cluster

Make sure to have enough RAM! AWX won't install properly if less than 5Gb.

```bash
# Start the cluster
minikube start --addons=ingress --cni=flannel --install-addons=true \
    --kubernetes-version=stable \
    --vm-driver=docker --wait=false \
    --cpus=4 --memory=5g


# check status
minikube status
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
```

## 6. Install AWX Operator

```bash
# Get latest release
git clone --depth 1 https://github.com/ansible/awx-operator
cd awx-operator

# Create Deploy configuration
export NAMESPACE=awx
make deploy
cd /home/$USER/awx-operator/config/manager
/home/$USER/awx-operator/bin/kustomize edit set image controller=quay.io/ansible/awx-operator:0.15.0
/home/$USER/awx-operator/bin/kustomize build /home/$USER/awx-operator/config/default | kubectl apply -f -

# Check if running
kubectl get pods -n $NAMESPACE

```

## 7. Install AWX

```bash
# Change context to the dedicated namespace
kubectl config set-context --current --namespace=$NAMESPACE

# Create configuration and deploy
cd ~
cat << EOF > awx.yaml
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
spec:
  service_type: nodeport
EOF

kubectl apply -f awx.yaml

# Check status
kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
kubectl get svc -l "app.kubernetes.io/managed-by=awx-operator"
```

Should look like this:

```bash
ansible@kube:~$ kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
NAME                   READY   STATUS              RESTARTS   AGE
awx-7d4f664875-24qpl   0/4     ContainerCreating   0          46s
awx-postgres-0         1/1     Running             0          55s

```

Once all 6 pods are running, we should now be able to access AWX.

## 8. Get the Admin password

Need to get the admin password first

```bash
echo Username: admin$'\n'Password: `kubectl get secret awx-admin-password -o jsonpath="{.data.password}" | base64 --decode`
```

## 9. Setup Proxy Redirect

If you want to access AWX from another machine, the internal container port needs to be exposed first.

```bash
# Check the AWX service to see what port is listening on, usually 31850
minikube service awx-service --url -n $NAMESPACE

# Another way
kubectl config set-context --current --namespace=$NAMESPACE

# Example of what it should look like
ansible@kube:~$ kubectl get service
NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
awx-operator-controller-manager-metrics-service   ClusterIP   10.107.165.190   <none>        8443/TCP       24m
awx-postgres                                      ClusterIP   None             <none>        5432/TCP       10m
awx-service                                       NodePort    10.96.83.234     <none>        **80:31850**/TCP   10m


# Start redirect
nohup minikube tunnel &
kubectl port-forward svc/awx-service --address 0.0.0.0 31850:80 &> /dev/null &
```

AWX should be accessible on port 80 from another machine now.
