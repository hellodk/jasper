---
layout: post
title: Deploy NFS Server on Ubuntu 20.04
date: 2021-09-19 22:22:00
tags: devops
subclass: 'post tag-devops'
author: hellodk
categories: hellodk
cover: false
navigation: True
---
##### The NFS Server setup on an Ubuntu 20.04 system will primarily comprise of the below steps
```java
1. Updating the apt repository
2. Installing the NFS Kernel Server package
3. Creating export directory for the NFS server
```
```shell
sudo apt update
```
```shell
sudo apt install nfs-kernel-server
```

###### Creating the export directory for the NFS server
```shell
sudo mkdir -p /mnt/nfs_share
```



helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner     --set nfs.server=x.x.x.x     --set nfs.path=/exported/path
  954  helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
  955  helm repo update
  956  helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner  --set nfs.server=192.168.0.9 --set nfs.path=/mnt/nfs_share
  957  vim ~/.kube/config 
  958  helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner  --set nfs.server=192.168.0.9 --set nfs.path=/mnt/nfs_share
  959  vim nfs-server.yaml 
  960  kubectl get sc
