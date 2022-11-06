---
layout: post
title: Deploy Spark History Server using Helm
date: 2021-09-13 17:16:00
tags: devops mlops
subclass: 'post tag-devops'
author: hellodk
categories: hellodk
cover: false
navigation: True
---



```bash
mkdir spark-3.1.2
```
```bash
cd spark-3.1.2/
```
```bash
wget https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
```
```bash
tar -xvf spark-3.1.2-bin-hadoop3.2.tgz
```
```bash
cd spark-3.1.2-bin-hadoop3.2
```
```bash
bash /bin/docker-image-tool.sh -r hellodk -t 3.1.2 -p ./kubernetes/dockerfiles/spark/bindings/python/Dockerfile build
```
```bash
docker images
```
```bash
docker push hellodk/spark-py:3.1.2
```
```bash
docker push hellodk/spark-py:3.1.2
```
```bash
docker push hellodk/spark:3.1.2
```

========================================
from pyspark.sql import SparkSession
spark = SparkSession.builder.\
       master("spark://192.168.0.123:7077").getOrCreate()
print("spark session created")


