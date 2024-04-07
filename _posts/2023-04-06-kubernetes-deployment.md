---
layout:     post
title:      KUBERNETES详解
subtitle:   Deployment的使用
date:       2024-04-08
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

5、查看pods的labels信息

```shell
kubectl get pods --show-labels
```

```shell

```

### 更新

Deployment的更新仅针对Pod template (that is, .spec.template)改变，比如labels和image的更新，普通的扩容缩容不会触发rollout（滚动更新）

1、镜像更新，更新为nginx:1.16.1

```shell
kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1

```

output如下：

```
deployment.apps/nginx-deployment image updated
```

通过edit更新，更新字段 *.spec.template.spec.containers[0].image* from nginx:1.14.2 to nginx:1.16.1

```shell
kubectl edit deployment/nginx-deployment
```


2、查询rollout的状态

```shell
kubectl rollout status deployment/nginx-deployment
```
output如下：

```
Waiting for rollout to finish: 2 out of 3 new replicas have been updated...
```

或者

```
deployment "nginx-deployment" successfully rolled out
```
若上线成功

此时，执行命令，查看副本情况，新副本扩容完毕，旧副本缩容完毕

```shell
kubectl get rs
```

output如下：

```
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-1564180365   3         3         3       6s
nginx-deployment-2035384211   0         0         0       36s
```

此时，执行命令，查看pod情况，也能看到新的pod

下次你想要更新pod，只需要更新Deployment中的Pod template即可。

若你要查看Deployment的细节信息

执行命令如下(其他场景可以指定namespcae)：

```shell
kubectl describe deployments
```
output如下：

```
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Thu, 04 Apr 2024 21:32:58 -0700
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 2
Selector:               app=nginx
Replicas:               10 desired | 10 updated | 10 total | 10 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.16.1
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  nginx-deployment-86dcfdf4c6 (0/0 replicas created)
NewReplicaSet:   nginx-deployment-848dd6cfb5 (10/10 replicas created)
Events:          <none>
```

通过 *RollingUpdateStrategy* 的字段，设置了max surge以及max unavailable，分别代表最大可以承受的，最大可以不通信的，他们的比较对象都是*desired number* 相当于max surge，同时可以运行125% * desird number ，同时有25%可以不在线，就是75% * desired number。

它的执行顺序，新创建新的之后杀死旧的，在新的没有创建之前，不会杀旧的pod。



### 回滚

### 扩容

### 清理

### 状态

#### 失败

- 资源配额不足
- Readiness Probe failure（就绪性探针失败）
- 拉去镜像错误
- 权限不足
- limit ranges
- 





待完成。。。。。。