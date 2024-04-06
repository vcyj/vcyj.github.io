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






待完成。。。。。。