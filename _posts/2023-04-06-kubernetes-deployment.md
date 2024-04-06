---
layout:     post
title:      KUBERNETES详解
subtitle:   Deployment的使用
date:       2024-04-06
author:     caoyj
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - KUBERNETES
    - Deployment
---

## 前言

好记性不如烂笔头，将自己的个人在使用k8s过程中的一些积累记录下来，既方便于个人梳理，也方便后期查询，如果可以帮上其他遇到类似问题的人就更好了。

## 正文

Deployment是一类工作负载（workloads）它的提供了声明式管理ReplicaSet以及pods的方式。deployment通过spec来描述期望的状态，deployment controller新建RepicaSet，按照一个受控的速率，将当前的pod状态变为期望的状态。

> 勿手动直接管理ReplicaSets

下面是列举的Deployments的常规用法：

### 创建

创建一个文件如下：

nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

1、创建名为nginx-deployment的deployment：

```shell
kubectl apply -f nginx-deployment.yaml
```

2、查看deployment状态

```shell
kubectl get deployments
```

>上述命令不指定命名空间，即返回默认命名空间下deployments

展示信息如下：
- **NAME** 罗列出默认命名空间内的deployment名称.
- **READY** 展示可用的副本数量. 格式： ready/desired.
- **UP-TO-DATE** 展示副本更新到期望值的数量.
- **AVAILABLE** 展示可用的副本数量.
- **AGE** 展示应用运行时间.

3、查看rollout status

```shell
kubectl rollout status deployment/nginx-deployment
```

4、查看 ReplicaSet

```shell
kubectl get rs
```

 ReplicaSet的名称格式：[DEPLOYMENT-NAME]-[HASH]

5、查看


### 更新

### 回滚

### 扩容

### 清理





待完成。。。。。。