---
layout: post
title: Deploy EBS CSI Driver for Kubernetes
date: 2021-09-17 08:45:00
tags: devops mlops
subclass: 'post tag-devops'
author: hellodk
categories: hellodk
cover: false
navigation: True
---

##### EBSCSI

###### Add the EBS CSI helm repository
```bash
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
```
###### Update the helm repository
```shell
helm repo update
```
###### Create the trust-policy.json
```shell
vim trust-policy.json
```
###### Create the Amazon EKS role
```shell
aws iam create-role --role-name AmazonEKS_EBS_CSI_DriverRole     --assume-role-policy-document file://"trust-policy.json"
```
###### Attach the Amazon EKS policy to the role we just created above
```shell
aws iam attach-role-policy   --policy-arn arn:aws:iam::AWS_ACCOUNT_ID:policy/AmazonEKS_EBS_CSI_Driver_Policy   --role-name AmazonEKS_EBS_CSI_DriverRole
```

```shell
aws iam attach-role-policy --policy-arn arn:aws:iam::XXXXXXXXXXXX:policy/AmazonEKS_EBS_CSI_Driver_Policy   --role-name AmazonEKS_EBS_CSI_DriverRole
```

```shell
helm upgrade -install aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver --namespace kube-system --set image.repository=XXXXXXXXXXXX.dkr.ecr.us-west-2.amazonaws.com/eks/aws-ebs-csi-driver --set enableVolumeResizing=true --set enableVolumeSnapshot=true --set serviceAccount.controller.create=true --set serviceAccount.controller.name=ebs-csi-controller-sa
```

```shell
kubectl get po -n kube-system
```

```shell
git clone https://github.com/kubernetes-sigs/aws-ebs-csi-driver.git
```

```shell
kubectl get sc
```

```shell
cd aws-ebs-csi-driver/examples/kubernetes/dynamic-provisioning/
```

```shell
kubectl apply -f specs/
```

```shell
kubectl get pods --watch
```

```shell
kubectl get po
```

```shell
kubectl get pvc
```

```shell
kubectl get sc
```

```shell
kubectl patch storageclass gp2 -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```

```shell
kubectl patch storageclass ebs-sc -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

```shell
kubectl get sc
```

