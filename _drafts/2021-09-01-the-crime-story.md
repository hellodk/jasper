---
layout: post
navigation: True
title: The Crime Story - committed in love with EKS
date: 2021-09-02 10:18:00
tags: devops databases
subclass: 'post tag-devops tag-databases'
author: hellodk
categories: hellodk
cover: assets/images/kubernetes/kubernetes_logo.png
---
###### 
EKS is a managed Kubernetes offereing by AWS, and we've been playinig around it for quiet sometime.

Here I take the oportunity to list out the crimes we did with handling of EKS - Definitely in Staging Environement

Docker Images
Scan


PRIVATE repository
Patched Images
We have the control
Safe from reaching the pull limits as in case of Docker Image Pull limits
Single Repository for the entire organisation(Production/Staging)
Less Management Overhead
More control over the image life cycles
Prevents repetition of the same images across AWS Accounts or Repositories - Cost-Effective
All images should be customised to the minimum of
timezones should be updated and also alert should be made to track timezones across the pods/containers
If there is a patch available, then it should be patched as well
Pipeline for the automatic finding of images and building and patching
TAGS
git merge to be under strict scrutiny and via Pull Requests Only
Disable public images to be used in the Docker Environment
Ensure Adequate Subnets are there in the k8s cluster
cluster base dns

public
limit to office IP Addresses
private
Networking
NAT Gateway vs Internet Gateway Model
Cluster Autoscaling
Istio Ingress Gateways and Load Balancers
Istio Upgrades
Later Implementation of the observability tools
Late implementatino of the monitoring stack and then later scaling and then alerting mechanisms and alert group forming
ALB
Private Load Balancers
TCP connection Handling for Stateful Services
Certificate Integrations
Jenkins Pipelines
Monitoring
Spot Instance Termination Handler
Alerta dashboard
Alerting channel
Logging
EFK
Indexes
Disk Sizes
Application Deployment Pipelines
Keep a track of your limits
EC2 Spot Limits
Service Limits
EKS Upgrades
cycle
k8s manifests compatibilities
Team Coordination and Git repository access
k8s public access private access or vpc level access?
BI/DS Apps on k8s? Use VPC Endpoints and save cost

Do not create multiple repositories for the same thing
CDN
Provisions to remove completed/failed pods via k8s jobs
GP3 volumes to be default storage class
PV & PVC and Dynamically growing volumes
Cluster Base DNS
Volume in correct AZ Issue or Volume Affinity
Network Policies should always be in deny mode, open only what you need

cost tagiing for resopurces to be used as kubecost

Blog:
———
EKS - Containers should be in the correct time zone and ensure you put down the alerts

systemctl

