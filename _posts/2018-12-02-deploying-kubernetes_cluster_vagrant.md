---
layout: post
title: Deploying Kubernetes Cluster on Vagrant
date:  2018-12-02 12:30:00
tags: devops
subclass: 'post tag-devops'
categories: 'dk'
navigation: True
logo: 'assets/images/utilities/home_icon.png'
cover: 'assets/images/kubernetes/kubernetes_logo.png'
---
In this blog post we will cover the installation of a Kubernetes Cluster over 3 virtual machines spawned using Virtualbox and Vagrant.

This can be also useful to install Kubernetes over Bare Metal server or any other form of Virtual Machines as well.

##### Assumptions
	- Runing on Ubuntu 20.04 or Windows 10 or later
	- If running on Windows 10, disable Hyper-V
	- Vagrant and Virtualbox are already installed.
	- We have 3 Centos 7 virtual machines running.


##### Pre-requisites
There are a set of pre-requisites before we go ahead and start our Kubernetes Installation. Please follow the below steps for completing the pre-requisites.

1. Set the host-names for all 3 machines with the below commands. These hostnames help in node identification and DNS resolutions.
	```bash
	sudo hostnamectl set-hostname kubem
    sudo hostnamectl set-hostname worker1
    sudo hostnamectl set-hostname worker2
    ```
2. Disable selinux
To avoid the complexities to adding firewall rules, we disable the selinux.
```bash
set selinux 0
```
or, edit the file /etc/sysconfig/selinux to permanently disable selinux
```bash
sudo sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config
```

3. Disable swap memory - Kubernetes does not goes well with Swap, because memory swapping is allowed to occur on a host system, and idea of kubernetes is to tightly pack instances to as close to 100% utilized as possible. Hence if the scheduler sends a pod to a machine it should never use swap at all otherwise it can lead to performance and stability issues within Kubernetes.
```bash
swapoff -a
```
or
```bash
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

4. Set net bridge  for proper traffic routing
```bash
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

5. Reload sysctl
```bash
sysctl --system
```

6. Set DNS entries in /etc/hosts - Allows local DNS resolution to speed up things.
```bash
192.168.10.60 kubem
192.168.10.61 worker1
192.168.10.62 worker2
```

##### Docker Installation
Now the pre-requisites are done, let's go ahead and install docker
1. Add the Docker Repository
```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
2. Update the apt repository
```bash
yum update -y
```

3. Install Docker along with it's dependencies
```
yum install -y yum-utils device-mapper-persistent-data lvm2 docker -y
```

4. Use systemctl utility to configure the docker service and verify the status
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

5. Verify Docker service status and version
```bash
sudo systemctl status docker
docker version
docker info
```

##### Installing kubelet, kubeadm, kubectl

1. Add Kubernetes Repository
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```

2. Install Kubernetes Utilities - kubeadm, kubectl and kubelet
```bash
yum install -y kubelet kubeadm kubectl
```

3. Using systemctl enable and start the kubelet service
```bash
sudo systemctl enable kubelet
sudo systemctl start kubelet
```

4. Initialize the kubernetes cluster
```bash
kubeadm init --apiserver-advertise-address=172.31.19.193 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.244.0.0/12
```
Copy the joining token command and we will run the joining token on the worker1 and worker2.

5. Exit out of root user and become any non-root user and execute the below steps
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

6. Verify kubectl commands
```bash
kubectl get nodes
```

7. Creating the CNI and Dashboard
```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

8. Verify if all the nodes have become ready in a few minutes
```bash
kubectl get nodes
```

9. Deploy the Kubernetes Dashboard
```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

10. Patch the kubernetes-dashboard service from ClusterIP to NodePort
```bash
kubectl -n kubernetes-dashboard patch svc kubernetes-dashboard --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'
```

11. Get the secret toekn to login to the kubernetes dashboard
```bash
kubectl -n kube-system get secret
kubectl -n kube-system describe secret namespace-contoller-token-xyxyx
```
Now use this token to login to the cluster
https://Node IP:NodePORT


8. Verify our kubernetes cluster
```bash
kubectl get nodes
```

9. Run some workloads and see if the pods are getting provisioned
```bash
kubectl create deployment nginx --image nginx --replicas 4 --port 80
```

10. Verify if the pods have been created or not
```bash
kubectl get pods -o wide
```