---
layout:     post
title:      KUBERNETES详解
subtitle:   ReplicaSet的使用
date:       2024-04-07
author:     caoyj
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - KUBERNETES
    - ReplicaSet
---

## 前言

第一篇Deployment的文章没有写完，却已经有了退缩的意思，万事开头难，先把ReplicaSet的头给开了。

## 正文

ReplicaSet的主要用途是管理副本，管理pods的数量以及期望状态和当前状态，管理pods的扩容以及所容。
不可以直接对ReplicaSet进行操作，通过Deployment对Rp进行管理。
文章脉络：简要介绍什么是ReplicaSet,罗列他的使用方式，最后补充所有的细节知识

### 概念

ReplicaSet被设计的目的是在任何给定时间里都可以维护一组副本pods的运行。因此，它常用于确保一组相同的定义了具体数量的pods的可用性。

### 工作机制


